---
layout: post
title: Временное отключение экранной заставки при просмотре видео c помощью Caffeine
date: 2015-02-27 03:32
tags:
- программы
- linux
- ubuntu
- elementary os
- linux mint
- заставка
- настройка
---

Утилита **Caffeine** позволяет заблокировать запуск скринсейвера, переход компьютера в ждущий и спящий режимы. Можно задать список приложений для которых Caffeine будет автоматически активирован. Данный функционал может быть полезен для комфортного просмотра фильмов, прослушивания музыки и т.п.

## Установка

```bash
sudo add-apt-repository ppa:caffeine-developers/ppa
sudo apt-get update
sudo apt-get install caffeine python-glade2
```

### Источники

* [Caffeine](http://www.webupd8.org/2012/05/2-ways-to-temporarily-disable.html)
