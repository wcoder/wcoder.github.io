---
layout: post
title: Реализация INotifyPropertyChanged для WPF приложений
date: 2014-04-30 12:50
tags:
- wpf
- C#
- сниппет
---

[INotifyPropertyChanged](http://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx) - используется для уведомления представления об изменениях свойств объекта.

```csharp
using System.ComponentModel;
using System.Runtime.CompilerServices;

namespace ExampleINotifyPropertyChanged
{
	class PropertyChangedBase : INotifyPropertyChanged
	{
		public event PropertyChangedEventHandler PropertyChanged;
		
		protected virtual void NotifyPropertyChanged([CallerMemberName]string propertyName = null)
		{
			if (PropertyChanged != null)
			{
				PropertyChanged(this, new PropertyChangedEventArgs(propertyName));
			}
		}
	}
}
```

Реализация классом интерфейса предполагает генерацию события [PropertyChanged](http://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.propertychanged.aspx) каждый раз, когда значение свойства объекта изменяется. 
Такое поведение позволяет привязкам данных отслеживать состояние объекта и обновлять данные пользовательского интерфейса при изменении значения связанного свойства.

Пример реализации свойства модели представления:

```csharp
private string _amount;

public string Amount
{
	get { return _amount; }
	set
	{
		_amount = value;
		NotifyPropertyChanged();
	}
}
```

### Полезные материалы

* [Data binding overview (XAML)](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh758320.aspx)
