---
layout: post
title: Получаем размер файла в UWP приложении
date: 2016-01-19 23:32
tags:
- C#
- UWP
- сниппет
---

Класс [StorageFile](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.storagefile.aspx) предоставляет различные методы, с помощью которых мы можем получить размер файла. И этот метод `GetBasicPropertiesAsync` пример:

```csharp
public async Task<ulong> GetFileSize(StorageFile file)
{
	var basicProperties = await file.GetBasicPropertiesAsync();
	return basicProperties.Size;
}
```
