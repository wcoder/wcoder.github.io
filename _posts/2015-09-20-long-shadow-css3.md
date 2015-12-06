---
layout: post
title: Делаем длинную тень для текста на CSS3
date: 2015-09-20 17:50
tags:
- заметка
- css
- scss
---

Для того, чтобы сделать тень для текста используется свойство `text-shadow`:

``` css
text-shadow: 1px 1px #972c16;
```

Но, для того чтобы сделать длинную тень, нужно использовать это свойство так:

``` css
.long-shadow {
	text-shadow: 0px 0px #972c16,
	1px 1px #972c16,
	2px 2px #972c16,
	3px 3px #972c16,
	4px 4px #972c16,
	5px 5px #972c16,
	/* ... */
	34px 34px #972c16,
	35px 35px #972c16;
}
```

Как види-те, регулировать длину тени и её цвет абсолютно не удобно. Поэтому автоматизируем процесс с помощью препроцессора SCSS:

``` scss
@function longShadow($color, $lenght: 40) {
	$val: 0px 0px $color;
	@for $i from 1 through $lenght {
		$val: #{$val}, #{$i}px #{$i}px #{$color};
	}
	@return $val;
}
```
Пример использования:

<p data-height="268" data-theme-id="18502" data-slug-hash="WQxrVX" data-default-tab="result" data-user="wcoder" class='codepen'>See the Pen <a href='http://codepen.io/wcoder/pen/WQxrVX/'>WQxrVX</a> by Evgeniy Pakalo (<a href='http://codepen.io/wcoder'>@wcoder</a>) on 
<a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>
