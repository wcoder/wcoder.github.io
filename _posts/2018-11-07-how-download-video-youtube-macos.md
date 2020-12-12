---
layout: post
title: Как скачать видео с YouTube в macOS?
date: 2018-11-07 18:40
draft: true
tags:
- macos
- youtube
- заметка
- памятка
---

Для этого, вам придется выполнить лишь два простых действия:

1. Установить утилиту [youtube-dl](http://rg3.github.io/youtube-dl/);
2. Вызвать команду для скачивания.

Далее подробнее:

## Установка

- Откройте Terminal
- Выполните команду установки (при условии, что у вас уже установлен [Homebrew](https://brew.sh/))

```sh
brew install youtube-dl
```

## Скачивание

Простой пример скачивания видео:

```sh
youtube-dl https://www.youtube.com/watch?v=bS5P_LAqiVg
```

## Дополнительно

**youtube-dl** имеет массу конфигураций для различных вариантов использования, вот некоторые из них:

Скачать как аудио:

```sh
youtube-dl -x https://www.youtube.com/watch?v=--HaFAtC17U
```

Вывести список доступных форматов скачивания:

```sh
youtube-dl -F https://www.youtube.com/watch?v=bS5P_LAqiVg
```

Скачивание в выбранном формате (код формата можно получить из команды выше):

```sh
youtube-dl -f 137 https://www.youtube.com/watch?v=bS5P_LAqiVg
```

Скачать весь плейлист:

```sh
youtube-dl https://www.youtube.com/playlist?list=PLpVeA1tdgfCBrwbhK1RPdpJTLYOOrMyty
```

Скачать только выбранные видео из плейлиста:

```sh
youtube-dl --playlist-items 1,3,6 https://www.youtube.com/playlist?list=PLpVeA1tdgfCBrwbhK1RPdpJTLYOOrMyty
```

## Заключение

Утилита **youtube-dl**  так же доступна на Windows и Linux, подробнее [здесь](https://github.com/rg3/youtube-dl#installation). 

Проект на GitHub: [rg3/youtube-dl](https://github.com/rg3/youtube-dl)
