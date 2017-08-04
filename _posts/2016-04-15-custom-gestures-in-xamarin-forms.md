---
layout: post
title: Пользовательские жесты в приложениях Xamarin.Forms
date: 2016-04-15 17:07
original_url: http://www.xamarinhelp.com/custom-gestures-in-xamarin-forms/
tags:
- c#
- xamarin forms
---

Xamarin Forms приходит с ограниченным количеством распознаваемых жестов, в частности `TapGestureRecognizer`. Часто возникают задачи, где стандартных средств недостаточно, поэтому вам нужно будет реализовать пользовательские жесты самостоятельно. В этом примере я добавил в Xamarin.Forms проект два события `Pressed` и `Released`.

Пример приложения [Sample-XF-Gestures](https://github.com/adamped/sample-xf-gestures) расположен на GitHub.


## Создаем Gesture Recognizers для Xamarin.Forms

В нашем основном проекте, мы должны создать распознавателей жестов (Gesture Recognizers) для использования их в Xamarin.Forms. Я сделал два примера `PressedGestureRecognizer` и `ReleasedGestureRecognizer`.

```csharp
public class PressedGestureRecognizer : Element, IGestureRecognizer
{
	public static readonly BindableProperty CommandProperty = BindableProperty.Create("Command",
		typeof(ICommand), typeof(PressedGestureRecognizer), (object)null, BindingMode.OneWay,
		(BindableProperty.ValidateValueDelegate)null, (BindableProperty.BindingPropertyChangedDelegate)null,
		(BindableProperty.BindingPropertyChangingDelegate)null, (BindableProperty.CoerceValueDelegate)null,
		(BindableProperty.CreateDefaultValueDelegate)null);

	public static readonly BindableProperty CommandParameterProperty = BindableProperty.Create("CommandParameter",
		typeof(object), typeof(PressedGestureRecognizer), (object)null, BindingMode.TwoWay,
		(BindableProperty.ValidateValueDelegate)null, (BindableProperty.BindingPropertyChangedDelegate)null,
		(BindableProperty.BindingPropertyChangingDelegate)null, (BindableProperty.CoerceValueDelegate)null,
		(BindableProperty.CreateDefaultValueDelegate)null);

	public ICommand Command
	{
		get { return (ICommand)this.GetValue(CommandProperty); }
		set { this.SetValue(CommandProperty, (object)value); }
	}

	public object CommandParameter
	{
		get { return this.GetValue(CommandParameterProperty); }
		set { this.SetValue(CommandParameterProperty, value); }
	}
}
```

Для `ReleasedGestureRecognizer` то же самое.

## Добавляем рендереры для платформ

Далее нам нужно на каждой из платформ создать рендереры для тех элементов управления, для которых мы хотим добавить эти жесты. В этом примере я использую `Label`.

#### UWP

```csharp
[assembly: ExportRenderer(typeof(Label), typeof(LabelRender))]
namespace Mobile.UWP.Renderer
{
	public class LabelRender : LabelRenderer
	{
		protected override void OnElementChanged(ElementChangedEventArgs<Label> e)
		{
			base.OnElementChanged(e);

			if (e.OldElement == null)
			{
				if (!e.NewElement.GestureRecognizers.Any())
					return;

				if (e.NewElement.GestureRecognizers.Any(x => x.GetType() == typeof(PressedGestureRecognizer)))
					Control.PointerPressed += Control_PointerPressed;

				if (e.NewElement.GestureRecognizers.Any(x => x.GetType() == typeof(ReleasedGestureRecognizer)))
					Control.PointerReleased += Control_PointerReleased;
			}
		}

		private void Control_PointerPressed(object sender, Windows.UI.Xaml.Input.PointerRoutedEventArgs e)
		{
			foreach (var recognizer in this.Element.GestureRecognizers.Where(x => x.GetType() == typeof(PressedGestureRecognizer)))
			{
				var gesture = recognizer as PressedGestureRecognizer;
				if (gesture != null)
					if (gesture.Command != null)
						gesture.Command.Execute(gesture.CommandParameter);
			}
		}

		private void Control_PointerReleased(object sender, Windows.UI.Xaml.Input.PointerRoutedEventArgs e)
		{
			foreach (var recognizer in this.Element.GestureRecognizers.Where(x => x.GetType() == typeof(ReleasedGestureRecognizer)))
			{
				var gesture = recognizer as ReleasedGestureRecognizer;
				if (gesture != null)
					if (gesture.Command != null)
						gesture.Command.Execute(gesture.CommandParameter);
			}
		}
	}
}
```

#### Android

```csharp
[assembly: ExportRenderer(typeof(Label), typeof(LabelRender))]
namespace Mobile.Droid.Renderer
{
    public class LabelRender: LabelRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Label> e)
        {
            base.OnElementChanged(e);
            if (e.OldElement == null)
            {
                if (!e.NewElement.GestureRecognizers.Any())
                    return;
                if (e.NewElement.GestureRecognizers.Any(x => x.GetType() == typeof(PressedGestureRecognizer)
                                                            || x.GetType() == typeof(ReleasedGestureRecognizer)))
                    Control.Touch += Control_Touch;
            }
        }

        private void Control_Touch(object sender, TouchEventArgs e)
        {
            switch (e.Event.Action)
            {
                case MotionEventActions.Down:
                    foreach (var recognizer in this.Element.GestureRecognizers.Where(x => x.GetType() == typeof(PressedGestureRecognizer)))
                    {
                        var gesture = recognizer as PressedGestureRecognizer;
                        if (gesture != null)
                            if (gesture.Command != null)
                                gesture.Command.Execute(gesture.CommandParameter);
                    }
                    break;
                case MotionEventActions.Up:
                    foreach (var recognizer in this.Element.GestureRecognizers.Where(x => x.GetType() == typeof(ReleasedGestureRecognizer)))
                    {
                        var gesture = recognizer as ReleasedGestureRecognizer;
                        if (gesture != null)
                            if (gesture.Command != null)
                                gesture.Command.Execute(gesture.CommandParameter);
                    }
                    break;
                default:
                    break;
            }
        }
    }
}
```

#### iOS

```csharp
[assembly: ExportRenderer(typeof(Label), typeof(LabelRender))]
namespace Mobile.iOS.Renderer
{
    public class LabelRender : LabelRenderer
    {
        private class TouchLabel : UILabel
        {
            Label Element = null;
            public TouchLabel(Label element)
            {
                Element = element;
                this.Text = element.Text;
            }
            public override void TouchesBegan(NSSet touches, UIEvent evt)
            {
                base.TouchesBegan(touches, evt);
                foreach (var recognizer in this.Element.GestureRecognizers.Where(x => x.GetType() == typeof(PressedGestureRecognizer)))
                {
                    var gesture = recognizer as PressedGestureRecognizer;
                    if (gesture != null)
                        if (gesture.Command != null)
                            gesture.Command.Execute(gesture.CommandParameter);
                }
            }
            public override void TouchesCancelled(NSSet touches, UIEvent evt)
            {
                base.TouchesCancelled(touches, evt);
                foreach (var recognizer in this.Element.GestureRecognizers.Where(x => x.GetType() == typeof(ReleasedGestureRecognizer)))
                {
                    var gesture = recognizer as ReleasedGestureRecognizer;
                    if (gesture != null)
                        if (gesture.Command != null)
                            gesture.Command.Execute(gesture.CommandParameter);
                }
            }
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Label> e)
        {
            if (Control == null)
                SetNativeControl(new TouchLabel(Element) { });

            base.OnElementChanged(e);

            if (e.OldElement == null)
            {
                if (!e.NewElement.GestureRecognizers.Any())
                    return;

                Control.UserInteractionEnabled = true;
            }
        }
    }
}
```

Эти рендереры аттачат нативные события на вызовы соответствующих команд `Gesture Recognizer`ов.

## Использование

Теперь, когда мы все настроили, мы можем легко добавить распознавание жестов для любого `Label`.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage
	xmlns="http://xamarin.com/schemas/2014/forms"
	xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
	xmlns:gesture="clr-namespace:Mobile.Gestures;assembly=Mobile"
	x:Class="Mobile.View.MainPage" BackgroundColor="Black">

	<Label Text="Press Me" WidthRequest="250" HeightRequest="100" HorizontalTextAlignment="Center" VerticalTextAlignment="Center" TextColor="{Binding TextColor}" BackgroundColor="{Binding BackgroundColor}" VerticalOptions="Center" HorizontalOptions="Center">
		<Label.GestureRecognizers>
			<gesture:PressedGestureRecognizer Command="{Binding PressedGestureCommand}" />
			<gesture:ReleasedGestureRecognizer Command="{Binding ReleasedGestureCommand}" />
		</Label.GestureRecognizers>
	</Label>
</ContentPage>
```
