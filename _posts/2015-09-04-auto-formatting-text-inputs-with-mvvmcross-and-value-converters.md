---
layout: post
title: Автоматические форматирование текстового поля с помощью MvvmCross и конвертеров
date: 2015-09-04 11:51
original_url: http://www.gregshackles.com/auto-formatting-text-inputs-with-mvvmcross-and-value-converters/
tags:
- c#
- mvvm
- mvvmcross
- перевод
---

Конверторы это одна из мощнейших функций вашего MVVM фреймворка и с ними может быть очень весело экспериментировать.

В моем приложении у меня есть текстовые поля различных типов и необходимо найти простой путь для автоматического форматирования таких полей, как номера телефонов и кредитных карт.

Для подобной задачи использование конвертеров было довольно тривиальным. Здесь приведен пример конвертера телефонных номеров:

``` csharp
public class PhoneNumberValueConverter : MvxValueConverter
{
	public override object Convert(object value, Type targetType, object parameter, CultureInfo culture)
	{
		var numbers = Regex.Replace(value.ToString(), @"\D", "");

		if (numbers.Length <= 3)
			return numbers;
		if (numbers.Length <= 7)
			return string.Format("{0}-{1}", numbers.Substring(0, 3), numbers.Substring(3));

		return string.Format("({0}) {1}-{2}", numbers.Substring(0, 3), numbers.Substring(3, 3), numbers.Substring(6));
	}

	public override object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
	{
		return Regex.Replace(value.ToString(), @"\D", "");
	}
}
```

Да, я понимаю, что это только для телефонных номеров США, но это все что мне нужно сейчас. С вводом текста в поле он форматируется на лету без изменения значения в модели представления.

Вот аналогичный конвертер для номеров кредитных карт, разделяет число на четыре группы разделенных пробелом:

``` csharp
public class CreditCardNumberValueConverter : MvxValueConverter
{
	public override object Convert(object value, Type targetType, object parameter, CultureInfo culture)
	{
		var builder = new StringBuilder(Regex.Replace(value.ToString(), @"\D", ""));

		foreach (var i in Enumerable.Range(0, builder.Length / 4).Reverse())
			builder.Insert(4*i + 4, " ");

		return builder.ToString().Trim();
	}

	public override object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
	{
		return Regex.Replace(value.ToString(), @"\D", "");
	}
}
```

Вот, описанное в действии:

![Анимированный пример](http://www.gregshackles.com/content/images/2014/12/vc-3.gif)

На этом все!
