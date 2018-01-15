---
layout: post
title: Как открыть Visual Studio Code из терминала в macOS?
date: 2018-01-15 21:00
tags:
- visual studio code
- macos
- заметка
- настройка
---

Откройте Visual Studio Code и нажмите сочетание клавиш `Command` + `Shift` + `P`.
Введите значение `shell` в поле ввода, теперь из предложенных вариантов выберите вариант:

> Shell Command : Install 'code' command in PATH

![Как открыть Visual Studio Code из терминала в macOS?](https://raw.githubusercontent.com/wcoder/blog/master/2018-01-15-code/Ng886.png)

Вот и все.

Теперь откройте терминал и наберите комманду:

```
$ code .
```

