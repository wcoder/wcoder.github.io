---
layout: post
title: Как решить проблему выключения монитора при просмотре youtube в Linux Mint 17?
date: 2015-02-13 05:03
tags:
- linux
- linux mint
- ubuntu
- youtube
- html5
- настройка
---

Установил Linux Mint 17, открыл в хроме `http://youtube.com` и получил выключенный монитор. Как так вышло?!

Помогла установка плагина для флеша:

```
sudo apt-get install pepperflashplugin-nonfree
sudo update-pepperflashplugin-nonfree --install
```

Не совсем понятно, зачем он нужен, если с недавнего времяни ютуб использует html5 проигрыватель.

Может кто-нибудь знает ответ?
