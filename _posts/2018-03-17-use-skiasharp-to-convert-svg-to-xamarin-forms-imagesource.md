---
layout: post
title: Использование SkiaSharp для конвертации SVG в Xamarin.Forms ImageSource
date: 2018-03-17 17:10
original_url: http://danielhindrikes.se/xamarin/use-skiasharp-to-convert-svg-to-xamarin-forms-imagesource/
tags:
- xamarin
- xamarin forms
- skiasharp
- svg
- перевод
---

При работе с изображениями в проекте вашего приложения должно быть творится ад, пока вам нужно много различных разрешений для каждого изображения. Но, что если мы могли бы использовать векторную графику для изображений? К сожалению, Xamarin.Forms не поддерживает этого из коробки. Но это возможно с помощью [SkiaSharp](https://github.com/mono/SkiaSharp). SkiaSharp - кросс-платформенный API для 2D-графики, для платформы .NET, основанное на [Skia Graphics Library](https://skia.org/) от Google.

Есть пакет для Xamarin.Forms, который мы можем добавить в ваш проект, если хотим добавить Skia холст к нашим представлениям. Это здорово, но есть небольшое ограничение. Для примера, мы не можем использовать его для иконок в TabbedPage. Эта заметка покажет, как создать вспомогательный класс, который с помощью SkiaSharp преобразует SVG в ImageSource.

Прежде всего, нам нужно установить два NuGet-пакета:

- SkiaSharp.View.Forms
- SkiaSharp.Svg

Наш вспомогательный класс будет принимать четыре аргумента: имя файла, ширину, высоту, цвет:

```csharp
public class SvgHelper
{
    public static ImageSource GetAsImageSource(string svgImage, float width, float height, Color color)
    {
    }
}
```

Чтобы сделать это правильно, нам нужно знать коэффициент масштаба для устройства, на котором работает приложение:

```csharp
var scaleFactor = 0;

#if __IOS__
    scaleFactor = (int)UIKit.UIScreen.MainScreen.Scale;
#elif __ANDROID__
     // В MainActivity добавлено статическое свойство Current.
     scaleFactor = MainActivity.Current.Resources.DisplayMetrics.Density
#endif
```

Следующий шаг, загрузка SVG изображения:

```csharp
var svg = new SkiaSharp.Extended.Svg.SKSvg();

#if __IOS__
    svg.Load(svgImage);
#elif __ANDROID__
    var assetStream = MainActivity.Current.Assets.Open(svgImage);
    svg.Load(assetStream);
#endif
```

Когда мы загрузили SVG, нам нужно с масштабировать его до размера, который вы передали методу:

```csharp
var svgSize = svg.Picture.CullRect;
var svgMax = Math.Max(svgSize.Width, svgSize.Height);

float canvasMin = Math.Min((int)(width * scaleFactor), (int)(height * scaleFactor));
var scale = canvasMin / svgMax;
var matrix = SKMatrix.MakeScale(scale, scale);
```

Теперь, пришло время для отрисовки изображения, для этого нам нужен холст. Его размер будет определен в растровом изображении, которое мы передадим конструктору `SKCanvas`. Мы также хотим использовать цвет, что бы не иметь несколько изображений для разных цветов.

```csharp
var bitmap = new SKBitmap((int)(width*scaleFactor), (int)(height*scaleFactor));

var paint = new SKPaint()
{
    ColorFilter = SKColorFilter.CreateBlendMode(color.ToSKColor(), SKBlendMode.SrcIn)
};

var canvas = new SKCanvas(bitmap);
canvas.DrawPicture(svg.Picture, ref matrix, paint);
```

Теперь у нас есть холст, следующим шагом будет конвертация холста в поток, чтобы мы могли создать из него Xamarin.Forms `ImageSource`.

```csharp
var image = SKImage.FromBitmap(bitmap);
var encoded = image.Encode();
var stream = encoded.AsStream();
var source = ImageSource.FromStream(() => stream);

return source;
```

Теперь мы можем использовать его, чтобы установить источник изображения.

```csharp
var image = new Image();
image.Source = SvgHelper. GetAsImageSource("my_icon.svg", 30, 30, Color.Black);
```

Если вы хотите использовать это в XAML, одним из решений будет дополнительно создать MarkupExtension:

```csharp
public class DrawImageExtension : IMarkupExtension
{
    public object ProvideValue(IServiceProvider serviceProvider)
    {
        var source = SvgHelper.GetAsImageSource(FileName, Width, Height, Color);

        return source;
    }

    public string FileName { get; set; }
    public float Width { get; set; }
    public float Height { get; set; }
    public Color Color { get; set; }
}
```

Нам просто нужно импортировать пространство имен для расширения, а затем уже использовать его так:

```xml
<ContentPage
     xmlns="http://xamarin.com/schemas/2014/forms"
     xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
     xmlns:ext="clr-namespace:MyApp.Extensions" x:Class="MyApp.MyPage">
     <StackLayout>
          <Image Source="{ext:DrawImage FileName=my_icon.svg, Width=25, Height=25, Color=White}" />
     </StackLayout>
</ContentPage>
```

На этом все!
