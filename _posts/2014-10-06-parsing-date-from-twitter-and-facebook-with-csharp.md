---
layout: post
title: Парсинг даты из Twitter и Facebook в C#
redirect_to: "https://ypakala.com"
date: 2014-10-06 01:40
tags:
- api
- twitter
- facebook
- c#
- .net
---

Порой, при запросе к API твитера или фейсбука имеем следующий формат возвращаемой даты:

```
Wed Jan 11 23:48:15 +0000 2014
```

Для дальнейшей работы с ней, ее необходимо распарсить. Воспользуемся для этого следующим классом:

``` csharp
public static class Extensions
{
	public static DateTime ParseTwitterDateTime(this string date)
	{
		const string format = "ddd MMM dd HH:mm:ss zzzz yyyy";
		return DateTime.ParseExact(date, format, CultureInfo.InvariantCulture);
	}

	public static DateTime ParseFacebookDateTime(this string date)
	{
		const string format = "ddd, dd MMM yyyy HH:mm:ss zzzz";
		return DateTime.ParseExact(date, format, CultureInfo.InvariantCulture);
	}
}
```
