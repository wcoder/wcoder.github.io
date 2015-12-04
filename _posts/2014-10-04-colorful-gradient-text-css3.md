---
layout: post
title: Красочный градиент текста с помощью CSS
date: 2014-10-04 01:42
tags:
- css
- перевод
---

### HTML

```html
<h1>Gradient text</h1>
```

### CSS

```css
h1 {
	display: table;
	margin: 0 auto;
	font-family: "Roboto Slab";
	font-weight: 400;
	font-size: 5em;
	background: linear-gradient(330deg, #e05252 0%, #99e052 25%, #52e0e0 50%, #9952e0 75%, #e05252 100%);
	-webkit-background-clip: text;
	-webkit-text-fill-color: transparent;
	line-height: 200px;
}
```

### Демо

[Смотреть результат](http://jsfiddle.net/evgeniypakalo/0kmwdb9n/embedded/result/)

[Оригинал](http://wz2.ru/lfol)
