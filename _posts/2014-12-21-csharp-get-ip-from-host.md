---
layout: post
title: Получение IP-адреса по имени хоста с помощью C#
date: 2014-12-21 03:42
tags:
- C#
- .Net
- сниппет
---

Пример кода, для получения IP-адресов по имени хоста:

```csharp
using System.Net;

// ...

string howtogeek = "www.google.com";
IPAddress[] addresslist = Dns.GetHostAddresses(howtogeek);

foreach (IPAddress theaddress in addresslist)
{
	Console.WriteLine(theaddress.ToString());
}
```
