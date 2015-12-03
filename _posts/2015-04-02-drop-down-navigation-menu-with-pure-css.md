---
layout: post
title: Меню навигации с выпадающим списком на чистом CSS
date: 2015-04-02 03:57
tags:
- css
---

Чтобы сделать меню навигации с выпадающим списком на чистом CSS, мы воспользуемся возможностями CSS3, а именно, псевдоклассом `:checked`. 

Этот псевдосласс применяется к элементам интерфейса, таким как переключатели (checkbox) и флажки (radio), когда они находятся в положение "включено". Переключение элементов в такое состояние происходит с помощью атрибута *checked* тега `<input>` или пользователем.

### Разметка

```html
<nav class="nav-main">
	<div class="logo">Website</div>
	<ul>
		<li>
			<input type="radio" name="nav-group" class="nav-option" id="home">
			<label for="home" class="nav-item">Home</label>
			<label for="nav-close" class="nav-close"></label>
			<div class="nav-content">
				<div class="nav-sub">
					<ul>
						<li><a href="#">More about us 1</a></li>
						<li><a href="#">More about us 2</a></li>
						<li><a href="#">More about us 3</a></li>
					</ul>
				</div>
			</div>
		</li>
		<li>
			<input type="radio" name="nav-group" class="nav-option" id="css">
			<label for="css" class="nav-item">CSS</label>
			<label for="nav-close" class="nav-close"></label>
			<div class="nav-content">
				<div class="nav-sub">
					<ul>
						<li><a href="#">More about us 1</a></li>
						<li><a href="#">More about us 2</a></li>
						<li><a href="#">More about us 3</a></li>
					</ul>
				</div>
			</div>
		</li>
		<li>
			<input type="radio" name="nav-group" class="nav-option" id="dropdown">
			<label for="dropdown" class="nav-item">Dropdown</label>
			<label for="nav-close" class="nav-close"></label>
			<div class="nav-content">
				<div class="nav-sub">
					<ul>
						<li><a href="#">More about us 1</a></li>
						<li><a href="#">More about us 2</a></li>
						<li><a href="#">More about us 3</a></li>
					</ul>
				</div>
			</div>
		</li>
	</ul>
	
	<input type="radio" name="nav-group" id="nav-close" class="nav-close-option">
</nav>
```

### Стили

```css
.nav-main {
	width: 100%;
	background-color: #222;
	height: 70px;
	color: #fff;
}

.nav-main .logo {
	float: left;
	height: 40px;
	padding: 15px 30px;
	font-size: 1.4em;
	line-height: 40px;
}

.nav-main > ul {
	margin: 0;
	padding: 0;
	float: left;
	list-style-type: none;
}

.nav-main > ul > li {
	float: left;
}

.nav-option {
	display: none;
}

.nav-option:checked ~ .nav-content {
	max-height: 400px;
	-webkit-transition: max-height 0.4s ease-in;
	-moz-transition: max-height 0.4s ease-in;
	transition: max-height 0.4s ease-in;
}

.nav-option:checked + label {
	background-color: #444;
}

.nav-option:checked ~ .nav-close {
	display: block;
}

.nav-item {
	display: inline-block;
	padding: 15px 20px;
	height: 40px;
	line-height: 40px;
	margin: 0;
}

.nav-item:hover {
	background-color: #444;
	cursor: pointer;
}

.nav-content {
	position: absolute;
	top: 70px;
	overflow: hidden;
	max-height: 0;
	background-color: #222;
	color: #fff;
}

.nav-content a {
	color: #fff;
	text-decoration: none;
}

.nav-content a:hover {
	text-decoration: underline;
}

.nav-sub {
	padding: 20px;
}

.nav-sub ul {
	padding: 0;
	margin: 0;
	list-style-type: none;
}

.nav-sub ul a {
	display: inline-block;
	padding: 5px 0;
}

.nav-close {
	display: none;
	position: absolute;
	top: 70px;
	left: 0;
	height: 100%;
	width: 100%;
}

.nav-close-option {
	display: none;
}
```

### Результат:

[Смотреть результат](http://jsfiddle.net/evgeniypakalo/u1wqLtpj/embedded/result/)
