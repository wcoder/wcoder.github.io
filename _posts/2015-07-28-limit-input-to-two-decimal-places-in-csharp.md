---
layout: post
title: Ограничиваем ввод до двух знаков после запятой в C#
date: 2015-07-28 15:09
original_url: http://windowsapptutorials.com/tips/general-tips/limit-input-to-two-decimal-places-in-c/
tags:
- перевод
- C#
---

Иногда бывает нужно ограничить ввод до двух знаков после запятой.

Следующая функция возвращает `true`, если входное значения является действительным, в противном случае она возвращает значение `false`:

```csharp
class ValidAmount
{
	const int KEYCODE_FOR_DOT = 190;
	public static bool IsValidMoneyInput(String previousInput, String key, int keyCode)
	{
		if (!String.IsNullOrWhiteSpace(previousInput))
		{
			if (previousInput.Contains("."))
			{
				if (keyCode == KEYCODE_FOR_DOT)
				{
					return false;
				}
				else
				{
					String[] strings = previousInput.Split('.');
					if (strings[1].Length > 1)
					{
						return false;
					}
				}
			}
		}
		return true;
	}
}
```

Для использования данной функции, вам нужно навесить обработчик на событие `Key_Down` для текстового поля (TextBox) и поместить в него следующий код:

```csharp
private void Amounnt_OnKeyDown(object sender, KeyEventArgs e)
{
	e.Handled = !ValidAmount.IsValidMoneyInput(AmountToSend.Text, e.Key.ToString(), e.PlatformKeyCode);
}
```
