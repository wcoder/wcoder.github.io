---
layout: post
title: Конвертирование StorageFile в BitmapImage в универсальных приложениях Windows
date: 2015-12-27 14:10
tags:
- сниппет
- c#
- uwp
---

`StorageFile` может быть сконвертирован в `BitmapImage` используя данную функцию:

```csharp
public class ImageUtils
{
	public static async Task<BitmapImage> StorageFileToBitmapImage(StorageFile savedStorageFile)
	{
		using (IRandomAccessStream fileStream = await savedStorageFile.OpenAsync(Windows.Storage.FileAccessMode.Read))
		{
			BitmapImage bitmapImage = new BitmapImage();
			bitmapImage.DecodePixelHeight = 100;
			bitmapImage.DecodePixelWidth = 100;
			await bitmapImage.SetSourceAsync(fileStream);
			return bitmapImage;
		}
	}
}
```

Использовать следующим образом:

```csharp
BitmapImage imageSource= await ImageUtils.StorageFileToBitmapImage(savedStorageFile);
```
