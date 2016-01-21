---
layout: post
title: Узнаем размера файла по URL с помощью C#
date: 2015-12-29 23:40
original_url: http://windowsapptutorials.com/tips/general-tips/retrieve-file-size-from-remote-url-using-c-sharp/
tags:
- C#
- сниппет
- перевод
---

Класс `System.Net.HttpWebRequest`, системного API, позволяет программно получить информацию о файле. Размер каждого файла/страницы, как правило, доступны в заголовке ответа от сервера, который вы можете получить если сделаете запрос.

Заголовок содержит свойство `Content-Length`, которое показывает размер файла в байтах. Позже его можно сконвертировать в Мб или Гб.

Вот исходный код, который может быть для этого использован:

```csharp
private static string GetFileSize(Uri uriPath)
{
	var webRequest = HttpWebRequest.Create(uriPath);
	webRequest.Method = "HEAD";
	
	using (var webResponse = webRequest.GetResponse())
	{
		var fileSize = webResponse.Headers.Get("Content-Length");
		var fileSizeInMegaByte = Math.Round(Convert.ToDouble(fileSize) / 1024.0 / 1024.0, 2);
		return fileSizeInMegaByte + " MB";
	}
}
```
