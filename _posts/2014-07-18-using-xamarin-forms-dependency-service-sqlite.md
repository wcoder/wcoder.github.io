---
layout: post
title: Используем Xamarin.Forms DependencyService на примере SQLite
date: 2014-07-18 14:29
tags:

---

> Xamarin.Forms включает DependencyService, чтобы позволить общему коду использовать интерфейсы на конкретных платформах, для предоставления доступа к функциям iOS, Android и Windows Phone SDK, из вашего PCL или Shared проекта.

## Как работает DependencyService

Есть три части реализации `DependencyService`:

* Interface - интерфейс в общем коде, который определяет функциональность, которая требуется от кода платформы.
* Registration - реализация интерфейса на каждой конкретной платформе, вместе с атрибутом сборки, регистров класса так, чтобы `DependencyService` смог создать его экземпляры.
* Location - вызов `DependencyService.Get<>` в общем коде, чтобы получить специфичный для платформы экземпляр интерфейса во время выполнения, позволяя тем самым общему коду доступ к базовой платформе.

## Реализация и использование

### 1. Интерфейс

Создание интерфейса в проекте с общим кодом.

```csharp
using SQLite.Net;

namespace ExamplePortable.Core
{
	public interface ISQLite
	{
		SQLiteConnection GetConnection();
	}
}
```

### 2. Реализация

Реализация интерфейса для конкретной платформы.

```csharp
# using ...

[assembly: Dependency(typeof(SQLiteWinPhone))]

namespace ExamplePortable.WinPhone
{
	public class SQLiteWinPhone : ISQLite
	{
		public SQLiteConnection GetConnection()
		{
			// код метода
		}
	}
}
```

### 3. Использование

Вызываем метод для получения экземпляра класса.

```csharp
var sqLiteConnection = DependencyService.Get<ISQLite>().GetConnection();
```

## Используемые источники

* [Accessing Native Features via the DependencyService](http://wz2.ru/lfmp)
* [Working with a Local Database](http://wz2.ru/lfmo)
