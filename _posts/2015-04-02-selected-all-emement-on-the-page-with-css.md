---
layout: post
title: Выделение всех элементов на странице с помощью CSS
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

[Смотреть результат](https://jsfiddle.net/evgeniypakalo/n12uordc/embedded/result)
