---
layout: post
title: Определяем что документ был загружен с помощью JavaScript
redirect_to: "https://ypakala.com"
date: 2015-08-23 20:19
tags:
- сниппет
- javascript
---

Определяем без каких-либо фреймворков и библиотек:

``` js
HTMLDocument.prototype.ready = function () {
	return new Promise(function(resolve, reject) {
		if (document.readyState === 'complete') {
			resolve(document);
		} else {
			document.addEventListener('DOMContentLoaded', function() {
				resolve(document);
			});
		}
	});
}
```

Пример использования:

```js
document.ready().then(function() {
	// ...
});
```
