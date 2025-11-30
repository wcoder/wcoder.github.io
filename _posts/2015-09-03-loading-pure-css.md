---
layout: post
title: Индикатор загрузки на чистом CSS
redirect_to: "https://ypakala.com"
date: 2015-09-03 11:28
tags:
- css
- scss
---

## HTML

``` html
<div class="abs-center">
    <figure class="loader"></figure>
</div>
```

## CSS (SCSS)
Для удобного изменения размера кружков индикатора используется SCSS.

``` scss
// размер большего круга
$dot-size: 35px;

body {
	background-color: #333;
}

.abs-center {
	position: absolute;
	left: 50%;
	top: 50%;
	transform: translate(-50%, -50%);
}

.loader {
	position: relative;
	left: 30px;
	margin: 0;
	padding: 0;
	width: $dot-size;
	height: $dot-size;
	background-color: rgba(255, 255, 255, 0.9);
	border-radius: 50%;
	animation-duration: 1.2s;
	animation-name: rotate;
	animation-iteration-count: infinite;
	animation-timing-function: linear;
	transform-origin: 18px 50px;
	&:after {
		content: "";
		position: absolute;
		top: 30px;
		right: 36px;
		width: $dot-size - 5;
		height: $dot-size - 5;
		background-color: rgba(255, 255, 255, 0.8);
		border-radius: 50%;
		box-shadow: 13px 35px 0px -2px rgba(255, 255, 255, 0.6);
	}
	&:before {
		content: "";
		position: absolute;
		top: 79px;
		right: -4px;
		width: $dot-size - 16;
		height: $dot-size - 16;
		background-color: rgba(255, 255, 255, 0.4);
		border-radius: 50%;
		box-shadow: 23px -15px 0px -2px rgba(255, 255, 255, 0.2);
	}
}

// animation
@keyframes rotate {
	from {
		transform: rotate(0deg);
	}
	to {
		transform: rotate(360deg);
	}
}
```

## Рузультат

<p data-height="268" data-theme-id="18502" data-slug-hash="QjwyNY" data-default-tab="result" data-user="wcoder" class='codepen'>See the Pen <a href='http://codepen.io/wcoder/pen/QjwyNY/'>QjwyNY</a> by Evgeniy Pakalo (<a href='http://codepen.io/wcoder'>@wcoder</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>
