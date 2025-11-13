---
layout: post
title: Реализуем Sliding Panel с использованием Xamarin.Forms & MAUI
redirect_to: "https://ypakala.com"
date: 2018-02-04 22:18
original_url: https://julianocustodio.com/2018/01/26/sliding-panel/
tags:
- mobile
- xamarin forms
- MAUI
- перевод
---

Привет, в этом посте я продемонстрирую, как вы можете создать Sliding Panel в своем Xamarin.Forms приложении.

Для этого примера было создано 4 варианта скользящих панелей (Sliding Panel):

- [PageUp](#pageup)
- [PageRight](#pageright)
- [PageDown](#pagedown)
- [PageLeft](#pageleft)

На следующем рисунке показано, как были названы панели и значки, используемые в этом примере.

![Схема реализации](https://julianocustodiosite.files.wordpress.com/2018/01/12.png)

## PageUp

Создайте `AbsoluteLayout`, который появится при загрузке страницы и `StackLayout`, называемый **«PageUp»**, который будет панелью, которая будет скользить сверху вниз.

> Обратите внимание, что для оконки открытия/закрытия определены `GestureRecognizers`.

### XAML

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage
    xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:local="clr-namespace:DemoSlidingPanel"
    x:Class="DemoSlidingPanel.MainPage">

    <AbsoluteLayout
        x:Name="Page"
        VerticalOptions="FillAndExpand" >

        <StackLayout
            AbsoluteLayout.LayoutBounds="0,0,1,1"
            AbsoluteLayout.LayoutFlags="All"
            BackgroundColor="White">
            <Image
                Source="DownBlue.png"
                HorizontalOptions="CenterAndExpand"
                VerticalOptions="StartAndExpand">
                <Image.GestureRecognizers>
                    <TapGestureRecognizer Tapped="DownBlue_Tapped" />
                    <PanGestureRecognizer PanUpdated="DownBlue_Tapped" />
                </Image.GestureRecognizers>
            </Image>
        </StackLayout>

        <StackLayout
            x:Name="PageUp"
            AbsoluteLayout.LayoutBounds="0,0,1,1"
            AbsoluteLayout.LayoutFlags="All"
            Orientation="Vertical"
            VerticalOptions="FillAndExpand"
            Spacing="0">
            <StackLayout
                VerticalOptions="FillAndExpand"
                BackgroundColor="#006df0">
                <Image
                    Source="UpWhite.png"
                    HorizontalOptions="CenterAndExpand"
                    VerticalOptions="EndAndExpand">
                    <Image.GestureRecognizers>
                        <TapGestureRecognizer Tapped="UpWhite_Tapped" />
                        <PanGestureRecognizer PanUpdated="UpWhite_Tapped" />
                    </Image.GestureRecognizers>
                </Image>
            </StackLayout>
        </StackLayout>
    </AbsoluteLayout>

</ContentPage>
```

### Код

При инициализации страницы присвойте `-1000` свойству `TranslationY`, ранее созданной панели. Это позволит отобразить эту страницу первой.
Создайте методы `UpWhite_Tapped` и `DownBlue_Tapped`, которые будут вызываться при захвате `GestureRecognizers`.

`PageUp.TranslateTo` будет использоваться для выполнения группового перехода. Введите `0` для координаты `x` и `y` для отображения панели, а `-Page.Height` в координате `y`, чтобы скрыть панель.

`Easing.SinIn` - эффект, используемый для перехода.

```csharp
using System;
using System.Text;
using Xamarin.Forms;

namespace DemoSlidingPanel
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();

            PageUp.TranslationY = -1000;
        }

        async void UpWhite_Tapped(object sender, EventArgs e)
        {
            await PageUp.TranslateTo(0, -Page.Height, 500, Easing.SinIn);
        }

        async void DownBlue_Tapped(object sender, EventArgs e)
        {
            await PageUp.TranslateTo(0, 0, 500, Easing.SinIn);
        }
    }
}
```

## PageRight

Создайте `AbsoluteLayout`, который появится при загрузке страницы и `StackLayout` с именем **«PageRight»**, который будет панелью, которая будет перемещаться справа налево.

> Обратите внимание, что для оконки открытия/закрытия определены `GestureRecognizers`.

### XAML

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage
    xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:local="clr-namespace:DemoSlidingPanel"
    x:Class="DemoSlidingPanel.MainPage">

    <AbsoluteLayout
        x:Name="Page"
        VerticalOptions="FillAndExpand">
        <StackLayout
            AbsoluteLayout.LayoutBounds="0,0,1,1"
            AbsoluteLayout.LayoutFlags="All"
            BackgroundColor="White">
                <Image
                    Source="LeftBlue.png"
                    HorizontalOptions="EndAndExpand"
                    VerticalOptions="CenterAndExpand">
                    <Image.GestureRecognizers>
                        <TapGestureRecognizer Tapped="LeftBlue_Tapped" />
                        <PanGestureRecognizer PanUpdated="LeftBlue_Tapped" />
                    </Image.GestureRecognizers>
                </Image>
        </StackLayout>

        <StackLayout
            x:Name="PageRight"
            AbsoluteLayout.LayoutBounds="0,0,1,1"
            AbsoluteLayout.LayoutFlags="All"
            Orientation="Vertical"
            VerticalOptions="FillAndExpand"
            Spacing="0">
            <StackLayout
                VerticalOptions="FillAndExpand"
                BackgroundColor="#006df0">
                <Image
                    Source="RightWhite.png"
                    HorizontalOptions="StartAndExpand"
                    VerticalOptions="CenterAndExpand">
                    <Image.GestureRecognizers>
                        <TapGestureRecognizer Tapped="RightWhite_Tapped" />
                        <PanGestureRecognizer PanUpdated="RightWhite_Tapped" />
                    </Image.GestureRecognizers>
                </Image>
            </StackLayout>
        </StackLayout>

    </AbsoluteLayout>
</ContentPage>
```

### Код

При инициализации страницы назначьте `1000` свойству `TranslationX` ранее созданной панели. Это позволит отобразить эту страницу первой.
Создайте методы `LeftBlue_Tapped` и `RightWhite_Tapped`, которые будут вызваны при захвате `GestureRecognizers`.

`PageRight.TranslateTo` будет служить для выполнения группового перехода. Введите `0` в координатах `x` и `y`, чтобы отобразить панель и `Page.Width` в координате `x`, чтобы скрыть панель.

`Easing.SinIn` - эффект, используемый для перехода.

```csharp
using System;
using System.Text;
using Xamarin.Forms;

namespace DemoSlidingPanel
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();

            PageRight.TranslationX = 1000;
        }

        async void LeftBlue_Tapped(object sender, EventArgs e)
        {
            await PageRight.TranslateTo(0, 0, 500, Easing.SinIn);
        }

        async void RightWhite_Tapped(object sender, EventArgs e)
        {
            await PageRight.TranslateTo(Page.Width, 0, 500, Easing.SinIn);
        }
    }
}
```

## PageDown

Создайте `AbsoluteLayout`, который появится при загрузке страницы, и `StackLayout` с именем **«PageDown»**, который будет панелью, которая будет скользить снизу вверх.

> Обратите внимание, что для оконки открытия/закрытия определены `GestureRecognizers`.

### XAML

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage
    xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:local="clr-namespace:DemoSlidingPanel"
    x:Class="DemoSlidingPanel.MainPage">

    <AbsoluteLayout
        x:Name="Page"
        VerticalOptions="FillAndExpand">
        <StackLayout
            AbsoluteLayout.LayoutBounds="0,0,1,1"
            AbsoluteLayout.LayoutFlags="All"
            BackgroundColor="White">
            <Image
                Source="UpBlue.png"
                HorizontalOptions="CenterAndExpand"
                VerticalOptions="EndAndExpand">
                <Image.GestureRecognizers>
                    <TapGestureRecognizer Tapped="UpBlue_Tapped" />
                    <PanGestureRecognizer PanUpdated="UpBlue_Tapped" />
                </Image.GestureRecognizers>
            </Image>
        </StackLayout>

        <StackLayout
            x:Name="PageDown"
            AbsoluteLayout.LayoutBounds="0,0,1,1"
            AbsoluteLayout.LayoutFlags="All"
            Orientation="Vertical"
            VerticalOptions="FillAndExpand"
            Spacing="0">
            <StackLayout
                VerticalOptions="FillAndExpand"
                BackgroundColor="#006df0">
                <Image
                    Source="DownWhite.png"
                    HorizontalOptions="CenterAndExpand"
                    VerticalOptions="StartAndExpand">
                    <Image.GestureRecognizers>
                        <TapGestureRecognizer Tapped="DownWhite_Tapped" />
                        <PanGestureRecognizer PanUpdated="DownWhite_Tapped" />
                    </Image.GestureRecognizers>
                </Image>
            </StackLayout>
        </StackLayout>

    </AbsoluteLayout>
</ContentPage>
```

### Код

При инициализации страницы назначьте `1000` для свойства `TranslationY` ранее созданной панели. Это позволит отобразить эту страницу первой.
Создайте методы `UpBlue_Tapped` и `DownWhite_Tapped`, которые будут вызваны при захвате `GestureRecognizers`.

`PageDown.TranslateTo` будет служить для выполнения группового перехода. Введите `0` в координатах `x` и `y`, чтобы отобразить панель и `Page.Height` в координате `y`, чтобы скрыть панель.

`Easing.SinIn` - эффект, используемый для перехода.

```csharp
using System;
using System.Text;
using Xamarin.Forms;

namespace DemoSlidingPanel
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();

            PageDown.TranslationY = 1000;
        }

        async void UpBlue_Tapped(object sender, EventArgs e)
        {
            await PageDown.TranslateTo(0, 0, 500, Easing.SinIn);
        }

        async void DownWhite_Tapped(object sender, EventArgs e)
        {
            await PageDown.TranslateTo(0, Page.Height, 500, Easing.SinIn);
        }
    }
}
```

## PageLeft

Создайте `AbsoluteLayout`, который появится при загрузке страницы, и `StackLayout`, называемый **«PageLeft»**, который будет панелью, которая будет скользить снизу вверх.

> Обратите внимание, что для оконки открытия/закрытия определены `GestureRecognizers`.

### XAML

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage
    xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:local="clr-namespace:DemoSlidingPanel"
    x:Class="DemoSlidingPanel.MainPage">

    <AbsoluteLayout
        x:Name="Page"
        VerticalOptions="FillAndExpand">
        <StackLayout
            AbsoluteLayout.LayoutBounds="0,0,1,1"
            AbsoluteLayout.LayoutFlags="All"
            BackgroundColor="White">
                <Image
                    Source="LeftBlue.png"
                    HorizontalOptions="EndAndExpand"
                    VerticalOptions="CenterAndExpand">
                    <Image.GestureRecognizers>
                        <TapGestureRecognizer Tapped="LeftBlue_Tapped" />
                        <PanGestureRecognizer PanUpdated="LeftBlue_Tapped" />
                    </Image.GestureRecognizers>
                </Image>
        </StackLayout>

        <StackLayout
            x:Name="PageLeft"
            AbsoluteLayout.LayoutBounds="0,0,1,1"
            AbsoluteLayout.LayoutFlags="All"
            Orientation="Vertical"
            VerticalOptions="FillAndExpand"
            Spacing="0">
            <StackLayout
                VerticalOptions="FillAndExpand"
                BackgroundColor="#006df0">
                <Image
                    Source="LeftWhite.png"
                    HorizontalOptions="EndAndExpand"
                    VerticalOptions="CenterAndExpand">
                    <Image.GestureRecognizers>
                        <TapGestureRecognizer Tapped="LeftWhite_Tapped" />
                        <PanGestureRecognizer PanUpdated="LeftWhite_Tapped" />
                    </Image.GestureRecognizers>
                </Image>
            </StackLayout>
        </StackLayout>

    </AbsoluteLayout>
</ContentPage>
```

### Код

При инициализации страницы присвойте `-1000` свойству `TranslationX` ранее созданной панели. Это позволит отобразить эту страницу первой.
Создайте методы `LeftWhite_Tapped` и `RightBlue_Tapped`, которые будут вызываться при захвате `GestureRecognizers`.

`PageLeft.TranslateTo` будет использоваться для выполнения группового перехода. Введите `0` в координатах `x` и `y` для отображения панели и `-Page.Width` в координате `x`, чтобы скрыть панель.

`Easing.SinIn` - эффект, используемый для перехода.

```csharp
using System;
using System.Text;
using Xamarin.Forms;

namespace DemoSlidingPanel
{
    public partial class MainPage : ContentPage
    {
        public MainPage()
        {
            InitializeComponent();

            PageLeft.TranslationX = -1000;
        }

        async void LeftWhite_Tapped(object sender, EventArgs e)
        {
            await PageLeft.TranslateTo(-Page.Width, 0, 500, Easing.SinIn);
        }

        async void RightBlue_Tapped(object sender, EventArgs e)
        {
            await PageLeft.TranslateTo(0, 0, 500, Easing.SinIn);
        }
    }
}
```

## Результат

<img alt="Результат" src="https://julianocustodiosite.files.wordpress.com/2018/01/ezgif-com-gif-maker1.gif?w=300&h=533&zoom=2" width="300">

Исходный код [доступен на Github](https://github.com/juucustodio/SlidingPanel-Xamarin.Forms).
