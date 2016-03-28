---
layout: post
title: Более лучший способ выбора изображений из галерии или камеры в UWP приложениях
date: 2016-03-20 22:00
tags:
- C#
- windows phone
- UWP
- перевод
---

> Перевод статьи [Choosing an image from gallery or camera a bit better in Universal Windows apps](https://blog.kulman.sk/choosing-an-image-from-gallery-or-camera-in-uwp/)

При разработке Windows Phone приложений могут возникнуть случаи, когда нужно разрешить пользователю выбрать какую-либо фотографию из галереи или сделать новый снимок с помощью камеры телефона. Одним из примеров этого может быть процесс регистрации, когда пользователь может выбрать фото для профиля.

В Windows Phone 8.1, эта задача решается довольно просто, просто использовать `FileOpenPicker`. Это позволяет выбрать фотографию из галереи или сделать новую фотографию. Просто взгляните на эту анимацию, показывающую, как пользователь выбирает новую фотографию с помощью камеры телефона.

![Выбор фотографии в Windows Phone 8.1](https://blog.kulman.sk/images/wpa81.gif)

Код этого относительно простой, хотя модель `AndContinue` может заставить помучаться

```csharp
var openPicker = new FileOpenPicker
{
	ViewMode = PickerViewMode.Thumbnail,
	SuggestedStartLocation = PickerLocationId.PicturesLibrary
};
openPicker.FileTypeFilter.Add(".jpg");
openPicker.PickSingleFileAndContinue();
```

В Windows 10 Mobile, `FileOpenPicker ` был изменен, чтобы быть более настраиваемым. Это делает процесс выбора новых фотографий с помощью камеры телефона полностью скрытым. Для простого пользователя нет шансов обнаружить его, просто взгляните на эту анимацию:

![Выбор фотографии с камера в Windows 10 Mobile](https://blog.kulman.sk/images/uwp.gif)

Так, как сделать этот процесс немного более удобным для пользователя? Мое решение - вместо вызова `FileOpenPicker` показать `Flyout` с двумя опциями: **выбрать из галлереи** и **сделать фото**. Вариант **выбрать из галереи** просто вызывает `FileOpenPicker`.

```csharp
var openPicker = new FileOpenPicker
{
	ViewMode = PickerViewMode.Thumbnail,
	SuggestedStartLocation = PickerLocationId.PicturesLibrary
};
openPicker.FileTypeFilter.Add(".jpg");
var file = await openPicker.PickSingleFileAsync();

if (file != null)
{
	await ProcessFile(file);
}
```

а опция **сделать фото** с использованием *CameraCaptureUI* для непосредственной съемки

```csharp
var ccu = new CameraCaptureUI();
var file = await ccu.CaptureFileAsync(CameraCaptureUIMode.Photo);

if (file != null)
{
	await ProcessFile(file);
}
```

Результат может выглядеть так. Не забудьте добавить опцию для удаления фотографии, если она уже была выбрана.

![Результат](https://blog.kulman.sk/images/uwp2.gif)
