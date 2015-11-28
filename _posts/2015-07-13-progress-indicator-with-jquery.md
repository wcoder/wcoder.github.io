---
layout: post
title: Прогресс выполнения Ajax запроса в jQuery
date: 2015-07-13 17:51
tags:
- ajax
- jquery
- html5
- сниппет
---

Вторая версия XMLHttpRequest (XMLHttpRequest2) поддерживает события прогресса... для загрузки или скачивания!

Это очень просто реализовать если вам знаком jQuery, пример кода ниже:

``` js
$.ajax({
	type: 'POST',
	url: "/",
	data: {},
	beforeSend: function(XMLHttpRequest)
	{
		// прогресс загрузки на сервер
		XMLHttpRequest.upload.addEventListener("progress", function(evt){
			if (evt.lengthComputable) {  
				var percentComplete = evt.loaded / evt.total;
				// делать что-то...
			}
		}, false);
		// прогресс скачивания с сервера
		XMLHttpRequest.addEventListener("progress", function(evt){
			if (evt.lengthComputable) {  
				var percentComplete = evt.loaded / evt.total;
				// делать что-то...
			}
		}, false);
	},
	success: function(data){
		// делать что-то при успешном завершении...
	}
});
```

Для jQuery > 1.5.1

``` js
$.ajax({
	xhr: function()
	{
		var xhr = new window.XMLHttpRequest();
		// прогресс загрузки на сервер
		xhr.upload.addEventListener("progress", function(evt){
			if (evt.lengthComputable) {
				var percentComplete = evt.loaded / evt.total;
				// делать что-то...
				console.log(percentComplete);
			}
		}, false);
		// прогресс скачивания с сервера
		xhr.addEventListener("progress", function(evt){
			if (evt.lengthComputable) {
				var percentComplete = evt.loaded / evt.total;
				// делать что-то...
				console.log(percentComplete);
			}
		}, false);
		return xhr;
	},
	type: 'POST',
	url: "/",
	data: {},
	success: function(data){
		// делать что-то при успешном завершении...
	}
});
```

Конечно же никто не мешает использовать чистый `XMLHttpRequest`, без лишнего jQuery.

На этом все :)
