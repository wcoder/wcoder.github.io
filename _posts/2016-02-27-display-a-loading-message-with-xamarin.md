---
layout: post
title: Выводим индикатор загрузки в iOS c помощью Xamarin
date: 2016-02-27 23:50
original_url: https://developer.xamarin.com/recipes/ios/standard_controls/popovers/display_a_loading_message/
tags:
- xamarin
- ios
- сниппет
- перевод
---

Этот пост покажет как отобразить индикатор загрузки поверх всех элементов на странице.

## Рецепт

"Loading..." появляется при выполнении работ с сетью (или других длительных задач) и выглядит так:

![Наложение выглядит так](https://raw.githubusercontent.com/wcoder/blog/master/display-a-loading-message-with-xamarin/Loading.png)

Для отображения наложения, выполните следующие действия:

Добавьте класс `LoadingOverlay` в ваш проект:

```csharp
public class LoadingOverlay : UIView {
	// объявление элементов
	UIActivityIndicatorView activitySpinner;
	UILabel loadingLabel;
	
	public LoadingOverlay (CGRect frame) : base (frame)
	{
		// настройка частей
		BackgroundColor = UIColor.Black;
		Alpha = 0.75f;
		AutoresizingMask = UIViewAutoresizing.All;
		
		nfloat labelHeight = 22;
		nfloat labelWidth = Frame.Width - 20;
		
		// вывести в центре
		nfloat centerX = Frame.Width / 2;
		nfloat centerY = Frame.Height / 2;
		
		// создает и настраивает индикатор активности
		activitySpinner = new UIActivityIndicatorView(UIActivityIndicatorViewStyle.WhiteLarge);
		activitySpinner.Frame = new CGRect(
			centerX - (activitySpinner.Frame.Width / 2) ,
			centerY - activitySpinner.Frame.Height - 20 ,
			activitySpinner.Frame.Width,
			activitySpinner.Frame.Height);
		activitySpinner.AutoresizingMask = UIViewAutoresizing.All;
		AddSubview (activitySpinner);
		activitySpinner.StartAnimating();
		
		// создает и настраивает выводимый текст
		loadingLabel = new UILabel(new CGRect(
			centerX - (labelWidth / 2),
			centerY + 20,
			labelWidth,
			labelHeight
			));
		loadingLabel.BackgroundColor = UIColor.Clear;
		loadingLabel.TextColor = UIColor.White;
		loadingLabel.Text = "Loading...";
		loadingLabel.TextAlignment = UITextAlignment.Center;
		loadingLabel.AutoresizingMask = UIViewAutoresizing.All;
		AddSubview(loadingLabel);
	}
	
	/// <summary>
	/// Прячет контрол и удаляет его из родителя
	/// </summary>
	public void Hide()
	{
		UIView.Animate (
			0.5, // продолжительность
			() => { Alpha = 0; },
			() => { RemoveFromSuperview(); }
		);
	}
}
```

Создайте поле в классе для хранения ссылки на оверлей:

```csharp
LoadingOverlay loadPop;
```

Прежде чем начинать длительную задачу, создайте экземпляр оверлея и добавьте его на страницу:

```csharp
var bounds = UIScreen.MainScreen.Bounds;

// отображает индикатор загрузки в UI потоке с правильными размерами
loadPop = new LoadingOverlay(bounds);
View.Add (loadPop);
```

Когда задача завершена, спрятать индикатор можно вызовом метода `Hide()`:

```csharp
loadPop.Hide();
```

[Исходный код](https://github.com/xamarin/recipes/tree/master/ios/standard_controls/popovers/DisplayLoadingMessage)
