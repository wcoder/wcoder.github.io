---
layout: post
title: Генератор QR кодов
redirect_to: "https://ypakala.com"
date: 2014-06-28 10:00
tags:
- javascript
- проекты
---

Вчера набросал страничку для генерации QR кодов.

Для генерации решил не использовать свой бэкенд и воспользоваться возможностями гугла (ниже код запроса). Не используются сторонние JS и CSS, все свое - домашнее.

```js
/**
 * Скрипт для запроса QR кода из гугла
 * @param  {type} text текст
 * @param  {string} ecc коррекция ошибок (L, M, Q, H)
 * @param  {int} size размер изображения
 * @return {string} сформированный запрос к гуглу
 */
var getQRUrl = function(text, ecc, size){
	var url = 'http://chart.apis.google.com/chart?cht=qr&chs={s}x{s}&chld={e}|0&chl={t}';
	
	ecc = ecc || 'L';
	size = size || 200;
	text = encodeURIComponent(text).replace(/'/g,"%27").replace(/"/g,"%22");
	
	url = url.replace(/\{s\}/g, size).replace(/\{e\}/, ecc).replace(/\{t\}/, text);
	
	return url;
};
```
