---
layout: post
title: Aккордеон на чистом CSS
date: 2014-11-04 02:11
tags:
- css
---

### HTML

```html
<div class="container">
	<ul class="accordion">
		<li>
			<a href="#first" class="accordion-header">First</a>
			<div class="accordion-content" id="first">
				<p>Lorem ipsum dolor sit amet, consectetur adipisicing elit.</p>
			</div>
		</li>
		<li>
			<a href="#second" class="accordion-header">Second</a>
			<div class="accordion-content" id="second">
				<p>Ex veritatis quod reiciendis et dicta.</p>
			</div>
		</li>
		<li>
			<a href="#third" class="accordion-header">Third</a>
			<div class="accordion-content" id="third">
				<p>Vitae quibusdam quae nesciunt, fuga tenetur! </p>
			</div>
		</li>
	</ul>
</div>
```

### CSS

```css
.container {
	width: 100%;
	max-width: 400px;
	border: 1px solid #ccc;
}
.accordion {
	width: 100%;
	padding: 0;
	margin: 0;
	list-style-type: none;
}
.accordion-header {
	display: block;
	padding: 12px 20px;
	background-color: #bbb;
	color: #fff;
	text-decoration: none;
	font-size: 1.2em;
	text-transform: uppercase;
	text-shadow: 1px 1px 0 rgba(0, 0, 0, .1);
	margin-bottom: 5px;
}
.accordion-content p {
	margin: 20px;
}
.accordion-content {
	padding: 0;
	height: 0;
	overflow: hidden;
	-webkit-transition: height 400ms ease;
	transition: height 400ms ease;
}
.accordion-content:target {
	height: 150px;
	padding: 20px;
}
```

### Демо

[Смотреть результат](http://jsfiddle.net/evgeniypakalo/rh56oefp/embedded/result/)
