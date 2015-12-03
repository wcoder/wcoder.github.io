---
layout: post
title: Памятка по использованию MVVM Light в проекте Xamarin Forms
date: 2014-11-23 14:00
tags:
- C#
- .Net
- XAML
- Xamarin
- ios
- android
- windows phone
---

### Создание проекта Xamarin Forms

Создаем новый Xamarin Foms проект в Visual Studio 2013.

![Создание проекта Xamarin Forms](https://raw.githubusercontent.com/wcoder/blog/master/xamarin.forms_mvvmlight/1.PNG)

Структура сгенерированного проекта:

![Структура сгенерированного проекта](https://raw.githubusercontent.com/wcoder/blog/master/xamarin.forms_mvvmlight/2.PNG)

### Установка MVVM Light

Для проекта с общим кодом `MyNewApp`, устанавливаем NuGet пакет `MVVM Light libraries only (PCL)`

![MVVM Light](https://raw.githubusercontent.com/wcoder/blog/master/xamarin.forms_mvvmlight/3.png)

![MVVM Light](https://raw.githubusercontent.com/wcoder/blog/master/xamarin.forms_mvvmlight/4.PNG)

Соглашаемся с лицензией, нажав кнопку `I Accept`.

После успешной установки в References проекта можно увидеть:

![References](https://raw.githubusercontent.com/wcoder/blog/master/xamarin.forms_mvvmlight/7.PNG)

### Использование MVVM Light

Создаем страницу Xamarin.Forms:

![Xamarin.Forms page](https://raw.githubusercontent.com/wcoder/blog/master/xamarin.forms_mvvmlight/8.PNG)

Немного подправим файл `App.cs`:

```csharp
using MyNewApp.Views;
using Xamarin.Forms;

namespace MyNewApp
{
	public class App
	{
		public static Page GetMainPage()
		{
			return new MainPage();
		}
	}
}
```

Создадим для этой страницы вью-модель:

```csharp
using GalaSoft.MvvmLight;

namespace MyNewApp.ViewModels
{
	public class MainPageViewModel : ViewModelBase
	{
	}
}
```

В корне создадим локатор:

```csharp
using GalaSoft.MvvmLight.Ioc;
using Microsoft.Practices.ServiceLocation;
using MyNewApp.ViewModels;

namespace MyNewApp
{
	public class ViewModelLocator
	{
		static ViewModelLocator()
		{
			ServiceLocator.SetLocatorProvider(() => SimpleIoc.Default);
			SimpleIoc.Default.Register<MainPageViewModel>();
		}
		
		public MainPageViewModel Main
		{
			get
			{
				return ServiceLocator.Current.GetInstance<MainPageViewModel>();
			}
		}
	}
}
```

Взглянем на структуру проекта:

![Структура проекта](https://raw.githubusercontent.com/wcoder/blog/master/xamarin.forms_mvvmlight/10.PNG)

Снова немного подправим файл `App.cs`:

```csharp
using MyNewApp.Views;
using Xamarin.Forms;

namespace MyNewApp
{
	public class App
	{
		private static ViewModelLocator _locator;
		
		public static ViewModelLocator Locator
		{
			get
			{
				return _locator ?? (_locator = new ViewModelLocator());
			}
		}
		
		public static Page GetMainPage()
		{
			return new MainPage();
		}
	}
}
```

Подключим вью-модель на странице:

```csharp
using Xamarin.Forms;

namespace MyNewApp.Views
{
	public partial class MainPage : ContentPage
	{
		public MainPage()
		{
			InitializeComponent();
			BindingContext = App.Locator.Main;
		}
	}
}
```

![demo](https://raw.githubusercontent.com/wcoder/blog/master/xamarin.forms_mvvmlight/11.PNG)

Готово!

[Скачать исходники](https://github.com/wcoder/blog/raw/master/xamarin.forms_mvvmlight/MyNewApp.zip)
