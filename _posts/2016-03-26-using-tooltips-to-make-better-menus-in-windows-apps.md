---
layout: post
title: Используем подсказки в UWP приложениях
redirect_to: "https://ypakala.com"
date: 2016-03-26 17:40
tags:
- xaml
- uwp
- сниппет
- перевод
---

> Перевод статьи [Using Tooltips to make better menus in Windows apps](https://blog.kulman.sk/using-tooltips-to-make-better-menus-in-windows-apps/)

Если вы используете Windows приложения с навигационным меню, состоящим из иконок, то, вы наверное заметили, что в некоторых из этих приложений при наведении на иконку, отображается текст подсказки над иконкой. Это удобно для пользователей, т.к. позволяет им быстро понять смысл иконки без необходимости нажимать на них или расширять меню (если доступно).

![Подсказки в меню](https://blog.kulman.sk/images/tooltips.gif)

Реализовать подобное поведение очень просто, благодаря `ToolTipService` доступному в Windows 8.1 и Windows 10 UWP. Вы можете добавить `<ToolTipService.ToolTip>` содержищий любые элементы XAML разметки.

Пример простой текстовой подсказки:

```xml
<Grid Margin="50" Background="BlueViolet" Height="50" Width="50">

	<ToolTipService.ToolTip>
		<TextBlock Text="Квадрат" />
	</ToolTipService.ToolTip>

</Grid>
```

Пример подсказки, состоящей из нескольких элементов:

```xml
<ToolTipService.ToolTip>
	<StackPanel>
		<TextBlock Text="Квадрат" />
		<Rectangle Width="100" Height="100" Fill="Black" />
	</StackPanel>
</ToolTipService.ToolTip>
```

#### Дополнительно

* [MSDN ToolTipService Class](https://msdn.microsoft.com/en-us/library/system.windows.controls.tooltipservice(v=vs.110).aspx)

