---
layout: post
title: Нумерация строк в highlight.js
date: 2016-01-26 22:25
tags:
- javascript
- opensource
- проекты
---

Думаю большинство из тех, кто из всех доступных аналогов для подсветки исходного кода выбрал **highlight.js** сталкивались с отсутствием в нем нумерации строк. Так вот, с тем же столкнулся и я, после чего, написал плагин, который добавляет в highlight.js нумерацию строк.

[Демо](http://wcoder.github.io/highlightjs-line-numbers.js/)

## Установка
Плагин доступен для установки через:

### bower

```
bower install highlightjs-line-numbers.js
```

### npm

```
npm install highlightjs-line-numbers.js
```

Или можно скачать последнюю версию прямо из [GitHub](https://github.com/wcoder/highlightjs-line-numbers.js/archive/master.zip).

## Подключение

Заметным отличием **highlight.js** от аналогов является его простота подключения и использования, эту же простоту я старался поддержать и в этом плагине. Поэтому подключить его не составит труда.

Подключаем файл плагина на страницу, **после highlight.js**:

```html
<script src="path/to/highlightjs-line-numbers.min.js"></script>
```

Инициализируем, **после highlight.js**:

```js
hljs.initLineNumbersOnLoad();
```

Все, это все, что нужно, чтобы отобразить нумерацию строк.

Теперь немного подсластим, добавив стилей:

```css
.hljs-line-numbers {
	text-align: right;
	border-right: 1px solid #ccc;
	color: #999;
	-webkit-touch-callout: none;
	-webkit-user-select: none;
	-khtml-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;
}
```

Ммм, теперь намного лучше.

## jQuery
Так же имеется возможность использования совместно с jQuery:

```js
$(document).ready(function() {
	$('code.hljs').each(function(i, block) {
		hljs.lineNumbersBlock(block);
	});
});
```

Репозиторий на [GitHub](https://github.com/wcoder/highlightjs-line-numbers.js). :metal:

На этом все.







