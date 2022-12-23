---
layout: post
title: "Xamarin.iOS: How to open app's folder in Files.app programmatically"
date: 2022-12-23 15:05
locale: en_US
tags:
- xamarin.ios
- ios
- .net
- заметка
---


## Prerequisite

Also note, that there is a prerequisite. You need to make app directory "public" so it shows on its own in the "On my iPhone" section in Files app. This requires `Info.plist` modification, and you can find [documentation about this here](https://learn.microsoft.com/en-us/xamarin/ios/app-fundamentals/file-system#sharing-with-the-files-app). Check it out to enable this for your app.

## How to open Files app

Once you have enabled the public Documents folder, you need to construct a particular URL and use the `UIApplication` shared object to open it.

To get path to the app documents folder you can use .NET API, in a shared code for example:
```cs
var documentsPath = Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData);
```
> documentsUrl = /var/mobile/Containers/Data/Application/DDC2D6E0-XXXX-XXXX-XXXX-D73463E1BA12/Documents

That is an analog of pretty standard way in the iOS development world:
```cs
var documentsUrl = NSFileManager.DefaultManager.GetUrls(NSSearchPathDirectory.DocumentDirectory, NSSearchPathDomain.User).FirstOrDefault();
```
> documentsUrl = /var/mobile/Containers/Data/Application/DDC2D6E0-XXXX-XXXX-XXXX-D73463E1BA12/Documents

Now just open it:
```cs
OpenPathInFiles(documentsPath);
```

The Files app can be opened with the URL scheme `shareddocuments://`.
The function below can open the Files app, assuming the user hasn't deleted it:
```cs
private void OpenPathInFiles(string path)
{
    var sharedUrl = NSUrl.FromString($"shareddocuments://{path}");
    if (sharedUrl != null && UIApplication.SharedApplication.CanOpenUrl(sharedUrl))
    {
        UIApplication.SharedApplication.OpenUrl(sharedUrl);
    }
}
```

Using this approach, you can also navigate to folders inside your app's Documents folder:

```cs
var downloadsPath = Path.Combine(documentsPath, "Downloads");

OpenPathInFiles(downloadsPath);
```
> downloadsPath = /var/mobile/Containers/Data/Application/DDC2D6E0-XXXX-XXXX-XXXX-D73463E1BA12/Documents/Downloads

## Resources

- [Open your app's Documents folder programmatically in Files app](https://nemecek.be/blog/145/open-your-apps-documents-folder-programmatically-in-files-app)

