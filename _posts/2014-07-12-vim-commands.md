---
layout: post
title: Полезные команды Vim
redirect_to: "https://ypakala.com"
date: 2014-07-12 00:34
tags:
- памятка
- vim
- редакторы
---

#### Привести концы строк в файле к виду dos или unix соответственно

```
:set fileformat=dos
:set fileformat=unix
```

#### Задать размер табуляции в 4 пробела

```
:set tabstop=4
:set expandtab
```

#### Конвертация кодировки файла

```
:set fenc=cp1251<CR>
:set fenc=koi8-r<CR>
:set fenc=ibm866<CR>
:set fenc=utf-8<CR>
```

#### Поиск и замена текста

Заменяем текст BEFORE не текст AFTER.

```
:%s/BEFORE/AFTER/g
```

#### Нумерация строк

```
:set number
```

#### Смена темы оформления ([wiki](http://vim.wikia.com/wiki/Switch_color_schemes))

```
:colorscheme <тема>
```

Где <тема> имя темы, TAB работает как авто-дополнение.

[Остальные команды](http://ru.wikibooks.org/wiki/Vim)
