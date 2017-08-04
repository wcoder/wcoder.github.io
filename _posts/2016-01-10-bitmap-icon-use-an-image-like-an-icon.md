---
layout: post
title: BitmapIcon - используем изображение как иконку
date: 2016-01-10 01:11
original_url: http://grogansoft.com/blog/?p=1186
tags:
- uwp
- xaml
- сниппет
---

В UWP (Universal Windows Platform) приложениях вы можете использовать `BitmapIcon` для того чтобы использовать изображение, как будто это иконка/текст. Это очень полезно, если у вас есть картинка, фон которой должен меняться в зависимости от темы.

Вот простой пример использования:

```xml
<Button x:Name="MyButton" Foreground="{StaticResource MainColourBrush}" Background="{x:Null}" Tapped="MyButton_Tapped">
	<Button.Content>
		<BitmapIcon UriSource="Images/UI/image.png" Foreground="{StaticResource MainColourBrush}" />
	</Button.Content>
</Button>
```
Белая картинка будет окрашена в `MainColourBrush`, так же, как вы могли изменить цвет текста.
