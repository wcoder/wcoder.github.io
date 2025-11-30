---
layout: post
title: Как поменять значения двух переменных в JavaScript?
redirect_to: "https://ypakala.com"
date: 2014-08-11 08:09
tags:
- javascript
---

### Дано

```
var a = 100;
var b = "текст";
```

### Классическая реализация обмена

```
var temp = a;
a = b;
b = temp;
```

### Краткая реализация

Без введения дополнительной переменной:

```
b = [a, a = b][0];
```

#### Пример

```
var a = 100;
var b = "текст";

console.log(a, b);

b = [a, a = b][0]; // swap

console.log(a, b);

// вывод:
100 'текст'
текст 100
```
