---
layout: post
title: Таймер в Xamarin Forms
date: 2017-04-09 10:07
original_url: https://xamarinhelp.com/xamarin-forms-timer/
tags:
- Xamarin.Forms
- C#
- сниппет
- перевод
---


Таймер будет выполнять кусок кода в некотором временнном интервале. 
В Xamarin, каждая из платформ предоставляет свой собственный таймер и таймер Xamarin Forms использует их. Это не `System.Threading.Timer` из обычного профиля на основе PCL, однако если вы используете .NET Standard вы будете иметь доступ к `System.Threading.Timer`.

Однако в центре внимания этой статьи мы рассмотрим таймер Xamarin Forms.

![Таймер в Xamarin Forms](https://cloud.githubusercontent.com/assets/766193/24835513/ee05767e-1d0c-11e7-8951-5c41cd3760e9.png)

#### Таймер в Xamarin Forms

Для начала, вот основной таймер.

```csharp
Device.StartTimer(TimeSpan.FromSeconds(30), () =>
{
	// что-то делаем здесь...

	return true; // True = повторить снова, False = остановить таймер
});
```

На уровне платформы, используются следующие.

#### iOS

```csharp
public void StartTimer(TimeSpan interval, Func<bool> callback)
{
	NSTimer timer = NSTimer.CreateRepeatingTimer(interval, t =>
	{
		if (!callback())
			t.Invalidate();
	});
	NSRunLoop.Main.AddTimer(timer, NSRunLoopMode.Common);
}
```

#### Android

```csharp
public void StartTimer(TimeSpan interval, Func<bool> callback)
{
	var handler = new Handler(Looper.MainLooper);
	handler.PostDelayed(() =>
	{
		if (callback())
			StartTimer(interval, callback);

		handler.Dispose();
		handler = null;
	}, (long)interval.TotalMilliseconds);
}
```

#### UWP

```csharp
public void StartTimer(TimeSpan interval, Func<bool> callback)
{
	var timer = new DispatcherTimer { Interval = interval };
	timer.Start();
	timer.Tick += (sender, args) =>
	{
		bool result = callback();
		if (!result)
			timer.Stop();
	};
}
```

#### Таймер

В то время как таймер прост в использовании, есть ряд моментов, которые необходимо учитывать при его использовании.

#### Жизненный цикл

Жизненный цикл таймера - это то, о чем нужно помнить. Таймер представляет собой глобальную систему синхронизации, то есть любая ссылка, которую вы помещаете в код, будет ссылаться до тех пор, пока таймер не остановится. Следовательно, вы должны поставить условие внутри таймера, чтобы остановить его, когда он больше не требуется. Невыполнение этого может привести к утечке памяти.

#### Многопоточность

Любой код, запущенный в таймере, будет запущен в основном потоке пользовательского интерфейса. Убедитесь, что вы не блокируете поток пользовательского интерфейса и не выполняете интенсивных вычислений. Убедитесь, что код перемещается в фоновый поток, если это необходимо.

#### Состояние приложения

Если ваше приложение настроено в фоновом режиме, таймер будет продолжать работать, пока он не будет по крайней мере прерван или убит.
  
