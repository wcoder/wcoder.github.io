---
layout: post
title: Получаем размер файла в UWP приложении
date: 2016-01-19 23:32
tags:
- c#
- uwp
- сниппет
---

Класс [StorageFile](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.storagefile.aspx) предоставляет различные методы для работы с файлами. Так с помощью метода `GetBasicPropertiesAsync` мы можем получить размер файла:

```csharp
public async Task<ulong> GetFileSize(StorageFile file)
{
	var basicProperties = await file.GetBasicPropertiesAsync();
	return basicProperties.Size;
}
```
