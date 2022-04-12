---
layout: post
title: Простое клонирование/копирование массива в JavaScript
date: 2014-12-20 09:23
tags:
- перевод
- javascript
---

Чтобы клонировать/копировать содержимое массива, все, что вам нужно сделать, это вызвать метод [slice](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/slice), передав `0` в качестве первого аргумента:

```js
var clone = myArray.slice(0);
```

Код выше создает копию исходного массива; имейте в виду, если в вашем массиве существуют объекты - они хранятся как ссылки; т.е. код выше не делает "deep" клонирование содержимого массива.

Чтобы добавить копирование, как нативный метод к массивам, вы могли бы сделать что-то вроде этого:

```js
Array.prototype.clone = function () {
	return this.slice(0);
};
```

Давайте напишем рекурсивную функцию, чтобы копировать массив, который потенциально может иметь вложенные объекты или массивы:

```js
function arrayClone (arr) {
    var i, copy;
    
    if (Array.isArray(arr)) {
        copy = arr.slice(0);
        for (i = 0; i < copy.length; i++) {
            copy[i] = arrayClone(copy[i]);
        }
        return copy;
    } else if (typeof arr === 'object') {
        throw 'Cannot clone array containing an object!';
    } else {
        return arr;
    }
}
```

Если вы используете [Underscore](http://underscorejs.org/#every) -- можете сделать это еще короче:

```js
function arrayClone (arr) {  
    if (_.isArray(arr)) {
        return _.map(arr, arrayClone);
    } else if (typeof arr === 'object') {
        throw 'Cannot clone array containing an object!';
    } else {
        return arr;
    }
}
```

С версии ECMAScript 2015 был введен метод [Object.assign()](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/assign), с его помощью глубокое клонирование можно реализовать еще проще:

```js
var clone = Object.assign([], myArray);
```


### Заключение

Если вам нужно глубокое копирование произвольно-вложенных объектов и/или массивов, крошечный [node-clone](https://github.com/pvorb/node-clone) прекрасно с этим справиться. Он будет правильно обрабатывать даже копирование объектов с циклическими ссылками!

### Источники

* [Clone Arrays with JavaScript](http://davidwalsh.name/javascript-clone-array)
* [How to Clone a (Nested) Array in Javascript](http://blog.andrewray.me/how-to-clone-a-nested-array-in-javascript/)
