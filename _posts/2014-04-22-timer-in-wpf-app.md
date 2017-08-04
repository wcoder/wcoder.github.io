---
layout: post
title: Таймер в WPF приложении
date: 2014-04-22 10:42
tags:
- .net
- c#
- wpf
- сниппет
---

В отличии от WinForms, где для использования таймера есть соответствующий контрол, в WPF приложениях для этих целей используется класс [DispatcherTimer](http://msdn.microsoft.com/en-gb/library/system.windows.threading.dispatchertimer).

### Пример

```csharp
using System.Windows.Threading;
//...

//  установка таймера
timer = new DispatcherTimer();
timer.Tick += new EventHandler(timer_Tick);
timer.Interval = new TimeSpan(0,0,1);
timer.Start();

//...

private void timer_Tick(object sender, EventArgs e)
{
    // код здесь
}

```
