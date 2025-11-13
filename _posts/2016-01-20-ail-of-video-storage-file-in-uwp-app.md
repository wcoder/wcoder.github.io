---
layout: post
title: Получаем миниатурю видео-файла в UWP приложении
redirect_to: "https://ypakala.com"
date: 2016-01-20 12:20
original_url: http://windowsapptutorials.com/tips/storagefile/how-to-get-thumbnail-of-video-storage-file-in-windows-phone-app/
tags:
- c#
- uwp
- сниппет
- перевод
---

Класс [StorageFile](https://msdn.microsoft.com/en-us/library/windows/apps/windows.storage.storagefile.aspx) предоставляет нам метод, называемый `GetThumbnailAsync` с помощью которого мы можем получить миниатюру файла в Windows (phone) 8.1 и UWP приложении.

> Этот метод работает только для файлов, расположенных в публичных директориях вашего устройства. И не работает для файлов, расположеных в контейнере с данными приложения или локальном хранилище приложения.

Поэтому данный метод, будет полезен, когда вам нужно извлечь все видео файлы, которые хранятся в галерее телефона или вы работаете с видео, полученным из метода `FileOpenPicker`.

Код для получения миниатюры видео файла выглядит так:

```csharp
public async Task<BitmapImage> FetchThumbnail(StorageFile videofile)
{
	var thumbnail = await videofile.GetThumbnailAsync(ThumbnailMode.VideosView);

	var inputBuffer = new Windows.Storage.Streams.Buffer(2048);

	IBuffer buf;
	IRandomAccessStream stream=new InMemoryRandomAccessStream();

	while ((buf = (await thumbnail.ReadAsync(inputBuffer, inputBuffer.Capacity, InputStreamOptions.None))).Length > 0) {
		await stream.WriteAsync(buf);
	}

	var image = new BitmapImage();
	image.SetSource(stream);

	return image;
}
```

Если вам нужно сохранить миниатюру в локальном хранилище вашего приложения вы можете использовать код, указанный ниже:

```csharp
public async void SaveThumbnail(StorageFile videofile, string thumbnailfilename)
{
	var thumbnail = await videofile.GetThumbnailAsync(ThumbnailMode.VideosView);

	var saveFile = await ApplicationData.Current.LocalFolder.CreateFileAsync(thumbnailfilename, CreationCollisionOption.GenerateUniqueName);
	var inputBuffer = new Windows.Storage.Streams.Buffer(2048);

	using (var destFileStream = await saveFile.OpenAsync(FileAccessMode.ReadWrite))
	{
		IBuffer buf;
		while ((buf = (await thumbnail.ReadAsync(inputBuffer, inputBuffer.Capacity, InputStreamOptions.None))).Length > 0) {
			await destFileStream.WriteAsync(buf);
		}
	}
}
```

Я надеюсь этот пост будет полезным для вас.
