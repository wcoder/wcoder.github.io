---
layout: post
title: Настройка цвета строки заголовка в приложении Windows 10 на XAML
date: 2015-09-26 14:22
tags: windows 10, xaml, c#
---

В приложениях Windows 10 имеется возможность настроить цвета строки заголовка (например, приложений почты и календаря) вместо стандартного белого (или какой-то другой цвет, после обновления системы).

![стандартная, белая строка заголовка](https://raw.githubusercontent.com/wcoder/blog/master/win10-title-xaml/1.png)

Изменить цвет заголовка в Windows 10 приложениях на XAML — просто. В конструкторе для вашего главного XAML-файла (т. е... MainPage. xaml. cs), вы получите ссылку на строку заголовка из объекта `ApplicationView`, метода `GetForCurrentView()`. Вы можете установить цвета как заголовка окна так и цвет элементов управления окном с помощью установки соответствующих свойств:

``` csharp
public MainPage()
{
	this.InitializeComponent();

	var t = ApplicationView.GetForCurrentView().TitleBar;
	t.BackgroundColor = Colors.Indigo;
	t.ForegroundColor = Colors.White;
	t.ButtonBackgroundColor = Colors.Indigo;
	t.ButtonForegroundColor = Colors.White;
}
```

Вот все, что нужно. Вы можете с легкостью реализовать пользовательскую настройку цвета строки заголовка для вашего приложения.

![итоговый цвет строки заголовка](https://raw.githubusercontent.com/wcoder/blog/master/win10-title-xaml/2.png)

[Скачать исходный код](https://github.com/wcoder/blog/blob/master/win10-title-xaml/CustomizeTitleBarColors.zip?raw=true)

Источник: [Customize Title Bar Colors In Windows 10 XAML Apps](http://www.sluniverse.com/ffn/index.php/2015/08/customize-title-bar-color-in-windows-10-xaml-apps/)
