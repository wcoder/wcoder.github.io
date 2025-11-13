---
layout: post
title: Как посмотреть файлы iOS приложения в iPhone Simulator?
redirect_to: "https://ypakala.com"
date: 2018-01-20 00:00
tags:
- ios
- macos
- заметка
---


Для этого необходимо в терминале выполнить следующую команду:

```sh
open `xcrun simctl get_app_container booted <BUNDLE_IDENTIFIER> data` -a 
```
Где:

- `<BUNDLE_IDENTIFIER>` - уникальный идентификатор приложения (например `com.myapp.test`).

![Bundle Identifier в настройках iOS проекта](https://raw.githubusercontent.com/wcoder/blog/master/all/2.png)

Теперь вы можете посмотреть, какие файлы находятся в контейнере вашего приложения (например: база данных, скачанные файлы и т.п.)

Для открытия в Finder, добавим один параметр:

```sh
open `xcrun simctl get_app_container booted <BUNDLE_IDENTIFIER> data` -a Finder
```

![Пример содержимого контейнера в Finder](https://raw.githubusercontent.com/wcoder/blog/master/all/1.png)
