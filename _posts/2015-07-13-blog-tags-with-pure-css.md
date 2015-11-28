---
layout: post
title: Теги для блога на чистом CSS
date: 2015-07-13 17:30
tags:
- css
---

Картинка для затравки:

![Теги для блога на чистом CSS](https://raw.githubusercontent.com/wcoder/blog/cd282b6d58c5fc62d299d7723452f13929c37177/blog-tags/tags.png)

### HTML

``` html
<h2>Example 1</h2>
<a href="#" class="tag">Front-end development</a>

<h2>Example 2</h2>
<ul class="tags">
	<li><a href="#" class="tag">HTML</a></li>
	<li><a href="#" class="tag">CSS</a></li>
	<li><a href="#" class="tag">JavaScript</a></li>
</ul>
```

### CSS
```css
.tags {
	list-style: none;
	margin: 0;
	overflow: hidden; 
	padding: 0;
}

.tags li {
	float: left; 
}

.tag {
	background: #333;
	border-radius: 3px 0 0 3px;
	color: #fff;
	display: inline-block;
	height: 26px;
	line-height: 26px;
	padding: 0 20px 0 23px;
	position: relative;
	margin: 0 10px 10px 0;
	text-decoration: none;
	-webkit-transition: color 0.2s;
}

.tag::before {
	background: #fff;
	border-radius: 10px;
	box-shadow: inset 0 1px rgba(0, 0, 0, 0.25);
	content: '';
	height: 6px;
	left: 10px;
	position: absolute;
	width: 6px;
	top: 10px;
}

.tag::after {
	background: #fff;
	border-bottom: 13px solid transparent;
	border-left: 10px solid #333;
	border-top: 13px solid transparent;
	content: '';
	position: absolute;
	right: 0;
	top: 0;
}

.tag:hover {
	background-color: crimson;
}

.tag:hover::after {
	border-left-color: crimson; 
}
```

### Демо

<iframe width="100%" height="300" src="//jsfiddle.net/evgeniypakalo/esupq16m/embedded/result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>
