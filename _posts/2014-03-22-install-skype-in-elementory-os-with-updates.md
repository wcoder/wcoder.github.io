---
layout: post
title: Установка Skype в Elementary OS с возможностью обновления
date: 2014-03-22 00:26
tags:
- elementary os
- skype
- ubuntu
---

Открываем терминал и выполняем указанные ниже команды.
Добавление нового репозитория:

```
sudo apt-add-repository "deb http://archive.canonical.com/ precise partner" 
```

Установка:

```
sudo apt-get update sudo apt-get install skype 
```

Теперь skype можно будет автоматически обновлять.
