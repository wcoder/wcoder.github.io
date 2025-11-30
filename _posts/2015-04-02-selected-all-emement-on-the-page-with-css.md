---
layout: post
title: Выделение всех элементов на странице с помощью CSS
redirect_to: "https://ypakala.com"
date: 2015-04-02 02:47
tags:
- css
---

Простой пример выделения всех элементов на странице:

``` css
*:not(body):not(html) {
	box-sizing:border-box;
	border: 1px dashed #000;
}
```

## Результат

<iframe width="100%" height="300" src="//jsfiddle.net/evgeniypakalo/n12uordc/embedded/result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>
