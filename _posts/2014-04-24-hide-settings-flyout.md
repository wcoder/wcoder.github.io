---
layout: post
title: Скрытие Settings Flyout
redirect_to: "https://ypakala.com"
date: 2014-04-24 13:18
tags:
- windows 8
- winjs
- javascript
---

Возникла задача, при клике по кнопке во settings flyout, скрывать этот самый флайаут.

Решение нашлось, поковырявшись в файле `ui.js`:

```js
var hideSettingsFlyouts = function() {
	var elements = document.querySelectorAll(".win-settingsflyout");
	for (var i = 0, len = elements.length; i < len; i++) {
		elements[i].style.visibility = "hidden";
	}
};
```
