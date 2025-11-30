---
layout: post
title: Конвертирование StorageFile в массив байтов в универсальных приложениях Windows
redirect_to: "https://ypakala.com"
date: 2015-09-18 16:23
tags:
- сниппет
- c#
- windows
---

Асинхронный метод для конвертирования StorageFile в массив байтов:

``` csharp
public static async Task<byte[]> GetBytesAsync(StorageFile file)
{
	byte[] fileBytes = null;
	if (file == null) return null;
	using (var stream = await file.OpenReadAsync())
	{
		fileBytes = new byte[stream.Size];
		using (var reader = new DataReader(stream))
		{
			await reader.LoadAsync((uint)stream.Size);
			reader.ReadBytes(fileBytes);
		}
	}
	return fileBytes;
}
```

В качестве аргумента передаем `StorageFile`, на выходе получаем массив.
