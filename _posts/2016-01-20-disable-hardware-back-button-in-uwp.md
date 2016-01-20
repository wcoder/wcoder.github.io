---
layout: post
title: Отключаем аппаратную кнопку Back в UWP приложении
date: 2016-01-20 23:25
tags:
- C#
- UWP
- сниппет
---

Иногда бывают ситуации, когда вы хотите запросить что-то у пользователя перед выходом со страницы или фрейма приложения. Поэтому вам нужно контролировать нажатие по кнопке *Back* в это время.

Ниже код, чтобы отключить стандартное поведение кнопки *Back* и выполнять нужное вам действие по нажатию по ней:

```csharp

// конструктор страницы
public MainPage()
{
	this.InitializeComponent();
	this.NavigationCacheMode = NavigationCacheMode.Required;

	// подписываемся на событие нажатия
	HardwareButtons.BackPressed += HardwareButtons_BackPressed;
}

// обработчик события нажатия
private void HardwareButtons_BackPressed(object sender, BackPressedEventArgs e)
{
		// сообщаем, что мы будем обрабатывать нажатие,
		// тем самым отключая стандартное поведение
		e.Handled = true;

		// здесь вы можете добавить свой код и выполнить любую задачу
}
```
