---
layout: post
title: Получение изображения по URL в Xamarin.iOS
date: 2018-01-16 21:40
tags:
- xamarin.ios
- c#
- сниппет
---

Простой способ для получения `UIImage` из URL-адреса:

```csharp
public static UIImage UIImageFromUrl(string uri)
{
	using (var url = new NSUrl(uri))
	using (var data = NSData.FromUrl(url))
		return UIImage.LoadFromData(data);
}
```
