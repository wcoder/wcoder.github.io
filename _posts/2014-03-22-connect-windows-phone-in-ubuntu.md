---
layout: post
title: Подключение Windows Phone в Ubuntu
redirect_to: "https://ypakala.com"
date: 2014-03-22 00:32
tags:
- windows phone
- ubuntu
- linux mint
- elementory os
---

Открываем терминал и прописываем следующее:

```
sudo add-apt-repository ppa:langdalepl/gvfs-mtp
sudo apt-get update
sudo apt-get upgrade
```

Перезагружаемся. Подключаем телефон.

Работоспособность проверена на Ubuntu-подобных системах (Linux Mint, elementory OS)

### Источники
* [Getting MTP enabled devices to work with Ubuntu?](http://askubuntu.com/questions/87667/getting-mtp-enabled-devices-to-work-with-ubuntu/308366#308366)
* [Upgrade to GVFS with MTP support in Ubuntu 12.10 or 12.04 to easily connect Android 4.0+ devices](http://www.webupd8.org/2013/01/upgrade-to-gvfs-with-mtp-support-in.html)
