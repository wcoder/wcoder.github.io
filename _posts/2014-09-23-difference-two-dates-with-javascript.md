---
layout: post
title: Разность между двумя датами на JavaScript
redirect_to: "https://ypakala.com"
date: 2014-09-23 02:18
tags:
- javascript
---

### Функция

```js
var differenceTwoDates = function(date1, date2) {
    var oneDay = 864E5;
    var diff = Math.abs(date1.getTime() - date2.getTime());
    return Math.round(diff / oneDay);
};
```

### Применение

```js
var oneDate = new Date("12/12/2014");
var secondDate = new Date("12/10/2014");    
var difference = differenceTwoDates(oneDate, secondDate);
```

### Демо

<iframe width="100%" height="300" src="//jsfiddle.net/evgeniypakalo/dz1vp32q/embedded/result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>
