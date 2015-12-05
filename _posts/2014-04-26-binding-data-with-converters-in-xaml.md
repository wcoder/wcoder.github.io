---
layout: post
title: Использование конвертера при привязке данных в XAML
date: 2014-04-26 09:32
tags:
- wpf
- xaml
- C#
- .Net
- сниппет
---

## Использование

Добавим пространство имен:

```xml
xmlns:conv="clr-namespace:WpfApplication1.Converters"
```

Укажем ресурс:

```xml
<Window.Resources>
    <conv:TestContentToStringConverter x:Key="TestContentToString" />
</Window.Resources>
```

Использование конвертера:

```xml
<TextBlock Text="{Binding TestContent, Converter={StaticResource=TestContentToString}, ConverterParameter=test}" />
```

## Описание конвертера

Реализует интерфейс [IValueConverter](http://msdn.microsoft.com/en-us/library/system.windows.data.ivalueconverter.aspx) из `System.Windows.Data`.

```csharp
// ...
using System.Windows.Data;

namespace WpfApplication1.Converters
{
	class TestContentToStringConverter : IValueConverter
	{
		public object Convert(object value, Type targetType, object parameter, System.Globalization.CultureInfo culture)
		{
			// parameter - параметр, переданный конвертеру
			
			if (value != null)
			{
				return ((string) value).Trim();
			}
			return "";
		}
		
		public object ConvertBack(object value, Type targetType, object parameter, System.Globalization.CultureInfo culture)
		{
			throw new NotImplementedException();
		}
	}
}
```

## Полезное

* [Binding.Converter](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.data.binding.converter.aspx)
* [Binding.ConverterParameter](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/windows.ui.xaml.data.binding.converterparameter.aspx)

