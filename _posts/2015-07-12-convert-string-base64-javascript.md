---
layout: post
title: Переводим строку в base64 и обратно на JavaScript
date: 2015-07-12 23:38
tags:
- javascript
- сниппет
---

**Base64** – способ кодирования произвольных двоичных данных в ASCII текст. 

Ниже представлен вариант, как сделать кодирование/декодирование данных в браузере на js:

```js
function utf8_to_b64(str) {
    return window.btoa(unescape(encodeURIComponent(str)));
}
```

```js
function b64_to_utf8(str) {
    return decodeURIComponent(escape(window.atob(str)));
}
```

### Использование

```js
utf8_to_b64('✓ à la mode'); // JTI1dTI3MTMlMjUyMCUyNUUwJTI1MjBsYSUyNTIwbW9kZQ==
b64_to_utf8('JTI1dTI3MTMlMjUyMCUyNUUwJTI1MjBsYSUyNTIwbW9kZQ=='); // "✓ à la mode"

utf8_to_b64('I \u2661 Unicode!'); // SSUyNTIwJTI1dTI2NjElMjUyMFVuaWNvZGUlMjUyMQ==
b64_to_utf8('SSUyNTIwJTI1dTI2NjElMjUyMFVuaWNvZGUlMjUyMQ=='); // "I ♡ Unicode!"
```

### Дополнительные ресурсы

* [MDN WindowBase64](https://developer.mozilla.org/en-US/docs/Web/API/WindowBase64/btoa)
* [base64-js](https://github.com/beatgammit/base64-js)
* [Base64-онлайн декодировщик](http://base64.ru/)
