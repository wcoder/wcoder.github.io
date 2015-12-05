---
layout: post
title: Вкладки на чистом CSS
date: 2015-04-23 03:00
tags:
- css
---

Чтобы сделать вкладки на чистом CSS, мы воспользуемся возможностями CSS3, а именно, псевдоклассом `:checked`.

Этот псевдосласс применяется к элементам интерфейса, таким как переключатели (checkbox) и флажки (radio), когда они находятся в положение "включено". Переключение элементов в такое состояние происходит с помощью атрибута *checked* тега `<input>` или пользователем.

Ранее мы уже резовали [меню навигации с выпадающим списком](https://wcoder.github.io/notes/drop-down-navigation-menu-with-pure-css/) пользуясь этим же методом.

## Разметка

```html
<ul class="tabs">
	<li>
		<input type="radio" name="tabs" id="tab-1" checked>
		<label for="tab-1">First</label>
		<div class="tab-content">
			Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
			tempor incididunt ut labore et dolore magna aliqua.
		</div>
	</li>
	<li>
		<input type="radio" name="tabs" id="tab-2">
		<label for="tab-2">Second</label>
		<div class="tab-content">
			Ut enim ad minim veniam,
			quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
			consequat.
		</div>
	</li>
	<li>
		<input type="radio" name="tabs" id="tab-3">
		<label for="tab-3">Third</label>
		<div class="tab-content">
			Excepteur sint occaecat cupidatat non
			proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
		</div>
	</li>
</ul>
```

## Стили
```css
.tabs {
	list-style-type: none;
	padding: 0;
	margin: 0;
	position: relative;
}
.tabs:after {
	content: "";
	clear: both;
	display: block;
	height: 240px;
}
.tabs li {
	float: left;
}
.tabs li > input {
	display: none;
}
.tabs li > label {
	display: inline-block;
	border: 1px solid #999;
	border-right-width: 0;
	border-bottom-width: 0;
	height: 30px;
	line-height: 30px;
	padding: 5px 20px;
	cursor: pointer;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;
}
.tabs li:last-child > label {
	border-right-width: 1px;
}
.tabs .tab-content {
	display: none;
	position: absolute;
	left: 0;
	padding: 20px;
	border: 1px solid #999;
	height: 200px;
	overflow-y: auto;
}

/* Функциональность: */

.tabs li > input:checked + label {
	background-color: #999;
}
.tabs li > input:checked ~ .tab-content {
	display: block;
}
```

## Результат

<iframe width="100%" height="300" src="//jsfiddle.net/evgeniypakalo/pqq3a94h/embedded/result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>
