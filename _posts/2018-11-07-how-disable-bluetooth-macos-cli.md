---
layout: post
title: Как выключить Bluetooth в macOS?
date: 2018-11-07 19:00
tags:
- macos
- bluetooth
- заметка
- памятка
- cli
---

Возможно у вас возникнет ситуация включения/выключение **Bluetooth (блютуз)** вашей **macOS** без использования мыши и пользовательского интерфейса, тогда вам на помощь придет консольная утилита [blueutil](https://github.com/toy/blueutil), которая просто и быстро решит ваши задачи.

## Установка

```sh
brew install blueutil
```

## Использование

**Включение bluetooth**:

```sh
blueutil -p 1
```

**Выключение bluetooth**:

```sh
blueutil -p 0
```
