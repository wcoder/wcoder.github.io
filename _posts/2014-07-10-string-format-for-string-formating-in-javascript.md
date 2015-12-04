---
layout: post
title: String.Format для форматирования строк в JavaScript
date: 2014-07-10 15:19
tags:
- сниппет
- javascript
---

При использовании JavaScript иногда встает вопрос о необходимости форматирования строки. Но, к сожалению, никакой стандартной функции для этого нет.

Поэтому, пришлось ее написать:

```js
String.prototype.format = String.prototype.f = function(){
	var args = arguments;
	return this.replace(/\{(\d+)\}/g, function(m,n){
		return args[n] ? args[n] : m;
	});
};
```

Пример использования:

```js
var s1 = "My name: {0}".f("Neo");
console.log(s1);
var s2 = "My name: {0}, age: {1}!".f("Neo", 20);
console.log(s2);
```
