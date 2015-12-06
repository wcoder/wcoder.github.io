---
layout: post
title: Запрос данных с сервера используя HttpClient
date: 2014-04-09 20:30
tags:
- C#
- .Net
- сниппет
---

Используем класс [HttpClient](http://msdn.microsoft.com/en-us/library/windows/apps/windows.web.http.httpclient.aspx):

``` csharp
public async static Task<string> GetHttpResponse(string url)
{
	HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
	request.Headers.Add("User-Agent", "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)");
	
	HttpClient client = new HttpClient();
	HttpResponseMessage response = await client.SendAsync(request, HttpCompletionOption.ResponseHeadersRead);
	
	if (response.StatusCode == HttpStatusCode.OK)
		return await response.Content.ReadAsStringAsync();
	else
		throw new Exception("Error connecting to " + url +" ! Status: " + response.StatusCode);
}
```

Или еще более кратко:

``` csharp
public async static Task<string> GetHttpResponse(string url)
{
	HttpClient client = new HttpClient();
	return await client.GetStringAsync(url);
}
```

Если в запросе происходит ошибка, то [GetStringAsync](http://msdn.microsoft.com/en-us/library/windows/apps/windows.web.http.httpclient.getstringasync.aspx) бросит исключение `HttpResponseException`.

### Источники

* [How to access a web feed (Windows Runtime apps using C#/VB/C++ and XAML)](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh452994.aspx)
