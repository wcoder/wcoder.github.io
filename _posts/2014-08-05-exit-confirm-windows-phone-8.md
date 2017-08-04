---
layout: post
title: Реализация подтверждения выхода из приложения в Windows Phone 8
date: 2014-08-05 01:23
tags:
- windows phone
- c#
- .net
---

В этом посте я покажу, как создать простую функцию для подтверждения выхода из приложения.

![windows phone 8, confirm](https://raw.githubusercontent.com/wcoder/blog/master/wp8-confirm/screen.png)

## Шаг 1. Создать функцию для очистки истории навигации:

```csharp
private void ClearBackEntries()
{
	while (NavigationService.BackStack != null
		&& NavigationService.BackStack.Any())
		NavigationService.RemoveBackEntry();
}
```

## Шаг 2. Обработать событие `BackKeyPress` на нужной вам странице:

```xml
<!--MainPage.xaml-->
<phone:PhoneApplicationPage
	...
	BackKeyPress="PhoneApplication_OnBackKeyPress">

	<!-- ...-->

</phone:PhoneApplicationPage>
```

```csharp
private void MainPage_OnBackKeyPress(object sender, CancelEventArgs e)
{
	// ...
}
```

## Шаг 3. Добавить код для вывода сообщения в обработчик события

```csharp
var message = "Are you sure?";
var title = "Exit?";

if (MessageBox.Show(message, title, MessageBoxButton.OKCancel) == MessageBoxResult.OK)
{
	ClearBackEntries();
}
else {
	e.Cancel = true;
}
```

[Скачать исходники](https://raw.githubusercontent.com/wcoder/blog/master/wp8-confirm/POCWP8Confirm.zip)

[Источник](http://wz2.ru/lfn0)
