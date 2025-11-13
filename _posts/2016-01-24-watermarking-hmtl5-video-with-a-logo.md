---
layout: post
title: HTML5 видео с водяным знаком
redirect_to: "https://ypakala.com"
date: 2016-01-24 16:25
tags:
- css
- html5
- сниппет
---

## HTML
Все что нам понадобиться - обернуть плеер:

```html
<div class="video-wrapper">
	<video controls>
		<source src="http://images.all-free-download.com/footage_preview/mp4/piano_keys_300.mp4" type="video/mp4">
	</video>
</div>
```
и применить следующие стили:

## CSS

```css
.video-wrapper {
	width: 400px;
	position: relative;
}
.video-wrapper video {
	width: 100%; 
}
.video-wrapper:before {
	content: ' ';
	position: absolute;
	background: url('https://avatars0.githubusercontent.com/u/766193');
	background-size: cover;
	width: 50px;
	height: 50px;
	right: 20px;
	bottom: 20px;
	opacity: .7;
}
```

## Результат

<iframe width="100%" height="500" src="//jsfiddle.net/evgeniypakalo/hz4acj1x/embedded/result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>
