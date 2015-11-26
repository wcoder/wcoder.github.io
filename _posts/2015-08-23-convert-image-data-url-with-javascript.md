---
layout: post
title: Конвертирование изображения в DataURL с помощью JavaScript
data: 2015-08-23 20:38
tags: сниппет, javascript
---

``` js
function getDataUri(url, callback, isRaw) {
	isRaw = isRaw || false;
	var image = new Image();

	image.onload = function () {
		var canvas = document.createElement('canvas');
		canvas.width = this.naturalWidth; // или 'width' если вы хотите особенный размер или масштабирование
		canvas.height = this.naturalHeight; // или 'height' если вы хотите особенный размер или масштабирование

		canvas.getContext('2d').drawImage(this, 0, 0);

		if (isRaw) {
			callback(canvas.toDataURL('image/png').replace(/^data:image\/(png|jpg);base64,/, ''));
		} else {
			callback(canvas.toDataURL('image/png'));
		}
	};

	image.src = url;
}
```

Пример использования:

``` js
getDataUri('/logo.png', function(dataUri) {
    // ...
});
```
