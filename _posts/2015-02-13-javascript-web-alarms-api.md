---
layout: post
title: Расширенный JavaScript API - Web Alarms API
redirect_to: "https://ypakala.com"
date: 2015-02-13 05:39
tags:
- javascript
---

Web Alarms API предоставляет доступ к настройкам оповещений на устройстве пользователя, при помощи которых вы можете настроить уведомления или запустить некоторое приложение в определенное время. Примерами использования данного API являются: запуск будильников, календарей или других приложений, которые выполняют различные действия в определенное время.

> Данный API находится на стадии набросков W3C.

## Посмотрим на API

```js
var alarmId;

// регистрирует новое уведомление по указанной дате и возвращает объект типа AlarmRequest
var request = navigator.alarms.add(
    new Date("July 12, 2014 01:30:23"),
    "respectTimezone",
    {somedata: "data"}
);

// оповещение успешно зарегистрировано
request.onsuccess = function (e) {
    alarmId = e.target.result;
};

// обнаружена ошибка при регистрации
request.onerror = function (e) {
    alert(e.target.error.name);
};
```

```js
// удаление оповещения
var request = navigator.alarms.remove(alarmId);

// успешно удалено
request.onsuccess = function (e) {
    alert("alarm removed");
};

// ошибка при удалении
request.onerror = function (e) {
    alert(e.target.error.name);
};
```

```js
// вызывается, когда происходит вызов уведомления.
navigator.alarms.addEventListener("alarm", function (e) {
    alert("alarm fired: " + JSON.stringify(e));
}, false);

// возвращает все зарегистрированные оповещения
var request = navigator.alarms.getAll();
request.onsuccess = function (e) {
    alert(JSON.stringify(e.target.result));
};
request.onerror = function (e) {
    alert(e.target.error.name);
};
```

> Поподробнее о Web Alarms API можно прочитать в его [спецификации](http://www.w3.org/TR/web-alarms/).
