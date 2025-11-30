---
layout: post
title: Индикатор загрузки на чистом CSS
redirect_to: "https://ypakala.com"
date: 2014-09-22 07:43
tags:
- css
- анимация
---

### HTML

```html
<div class="loader"></div>
```

### CSS

```css
@-webkit-keyframes loader {
	100% {
		-webkit-transform: rotate(360deg);
		transform: rotate(360deg);
	}
}

@-moz-keyframes loader {
	100% {
		-moz-transform: rotate(360deg);
		transform: rotate(360deg);
	}
}

@keyframes loader {
	100% {
		transform: rotate(360deg);
	}
}

.loader {
	width: 35px;
	height: 35px;
	border: 6px solid #000;
	border-left-color: #333;
	border-bottom-color: #555;
	border-right-color: transparent;
	border-radius: 100%;
	-webkit-animation: loader 0.5s infinite linear;
	-moz-animation: loader 0.5s infinite linear;
	animation: loader 0.5s infinite linear;
}
```

### Демо

[Смотреть результат](http://jsfiddle.net/evgeniypakalo/qo677c64/embedded/result/)
