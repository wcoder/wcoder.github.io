---
layout: post
title: C# клонирование объектов в PCL
date: 2015-03-21 23:25
original_url: http://mallibone-blog.azurewebsites.net/post/cloning-objects-in-the-pcl
tags:
- перевод
- C#
- .Net
- PCL
---

![Клонирование](https://habrastorage.org/files/9f3/c8b/e62/9f3c8be623364c939661893b1f4ac244.jpg)

Обычно при клонировании объекта в .NET, я просто использую `BinaryFormatter`, он будет автоматически выполняться глубокое копирование объекта. Но при написании кода на PCL мы не имеем возможности использовать `BinaryFormatter` и поэтому приходится искать другие варианты.

Я не большой поклонник делать копирование вручную, путем создания нового экземпляра класса. Это чревато ошибка связанными с последующим изменением существующих классов.

Поэтому, для решения этой задачи, я начал использовать [JSON.Net](http://www.newtonsoft.com/json).
Итак, давайте посмотрим на то, как это может быть реализовано:

``` csharp
public static T Clone<T>(this T source)
{
	if (Object.ReferenceEquals(source, null))
	{
		return default(T);
	}

	// In the PCL we do not have the BinaryFormatter
	return JsonConvert.DeserializeObject<T>(JsonConvert.SerializeObject(source));
}
```

Теперь, когда мы должны клонировать объект, делаем это так.

``` csharp
var original = new NestedObject();

var clone = original.Clone();
```
