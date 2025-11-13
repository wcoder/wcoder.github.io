---
layout: post
title: Убираем KMP Box в KMPlayer
redirect_to: "https://ypakala.com"
date: 2014-04-26 16:37
tags:
- windows
- настройка
- программы
---

KMP Box - абсолютно никому не нужная фигня и думаю все, кто первый раз его увидели, хотели сразу же его больше не видеть.

Выглядит она так (уродство справа):

![Убираем KMP Box в The KMPlayer](https://raw.githubusercontent.com/wcoder/blog/master/kmpbox/kmp.png)

## Как убрать?

1. С правами администратора, открываем для редактирования файл: `C:/Windows/System32/drivers/etc/hosts`
2. В самый конец файла добавляем следующую строку: `127.0.0.1    player.kmpmedia.net`
3. Сохраняем файл.
4. Profit!
