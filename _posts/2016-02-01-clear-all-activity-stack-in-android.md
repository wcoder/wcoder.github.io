---
layout: post
title: Очищаем Activity стек в Android
date: 2016-02-01 19:55
original_url: http://tips.androidhive.info/2013/10/how-to-clear-all-activity-stack-in-android/
tags:
- android
- java
- перевод
- сниппет
---

Обычно, когда мы запускаем новую Activity, все предыдущие Activity будут храниться в стеке.
Так что, если вы хотите убить все предыдущие Activity, просто выполните следующий метод.

В API уровня 11 или выше, используйте в Intent флаги `FLAG_ACTIVITY_CLEAR_TASK` и `FLAG_ACTIVITY_NEW_TASK`, чтобы очистить весь стек:

```java
Intent intent = new Intent(OldActivity.this, NewActivity.class);
// установка флагов
intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK | Intent.FLAG_ACTIVITY_CLEAR_TASK)
startActivity(intent);
```
