---
layout: post
title: Простой таймер на чистом JavaScript
redirect_to: "https://ypakala.com"
date: 2015-06-14 22:29
tags:
- javascript
- сниппет
---

В этой короткой заметке хотел бы показать пример простого таймера на javascript.

#### Код функции:

```js
/**
 * https://gist.github.com/wcoder/fcee724b4f5b44062754
 * @param seconds - кол-во секунд
 * @param tick - функция, вызываемая каждую секунду
 * @param result - функция, вызываемая по истечении времени
 */
function timer (seconds, tick, result) {
	if (seconds > 0) {
		tick(seconds);
		seconds -= 1;
		setTimeout(function () {
			timer(seconds, tick, result);
		}, 1000);
	} else {
		result();
	}
}
```

#### Как использовать:

```js
timer(15, function (s) {
	console.log('Прошло ' + s + ' секунд!');
}, function () {
	console.log('Время вышло!');
});
```

#### Пример использования:

<iframe width="100%" height="300" src="//jsfiddle.net/evgeniypakalo/vg33Lo1j/embedded/result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>
