---
layout: post
title: Как убрать "Ticino" из контекстного меню Windows?
date: 2015-05-05 02:58
tags:
- помощь
---

Заметили в контекстном меню проводника Windows подобное?

![Ticino](https://i.stack.imgur.com/bJhvw.png)

Пункт "Ticino" добавляется в контекстное меню проводника Windows после установки [Visual Studio Code](https://code.visualstudio.com/). Он позволяет открыть выбранный файл для редактирования.

После удаления VS Code, этот элемент не удаляется из контекстного меню. Поэтому приходится убирать его вручную. Открываем `regedit` и в следующих местах:

* `HKEY_CLASSES_ROOT\*\shell\Ticino`
* `HKEY_CLASSES_ROOT\directory\background\shell\Ticino`
* `HKEY_CLASSES_ROOT\directory\shell\Ticino`

Если находите "Ticino", удалите его.
Profit!
