---
layout: post
title: Понимание размера framework библиотек в iOS
date: 2018-04-27 12:30
tags:
- ios
- заметка
---


## App Thinning

Apple оптимизирует пакет вашего приложения с помощью [процесса утоньшения][1]. Ваши пользователи загружают только код и ресурсы, необходимые конкретно для их устройства. Вы можете [сгенерировать отчет о размере вашего приложения][2] с помощью Xcode или посмотреть отчет «Размер файлов магазина приложений» в iTunes Connect, чтобы узнать, каков будет фактический размер начальной загрузки вашего приложения. Обратите внимание, что эти цифры показывают максимальный размер, который будут загружать ваши пользователи. Дополнительную информацию о том, как оптимизируются обновления, см. далее.

## Архитектура процессора

Обычно framework включает в себя мульти-архитектурный бинарный код, который содержит части для `armv7`, `arm64`, `i386` и `x86_64` архитектур процессора. ARM используется для физического iOS устройства, в то время как `i386` и `x86_64` только для симулятора и будут вырезаны из вашего приложения во время сборки и архивации. Когда пользователь скачивает ваше приложение из App Store, он получает только ту архитектуру, которую требует его устройство.

![](https://help.apple.com/xcode/mac/current/en.lproj/Art/app_thinning_2x.png)

## Bitcode

[Bitcode][3] включен в ARM архитектуры, но не влияет на конечный размер вашего приложения. Bitcode - это неоптимизированное промежуточное представление кода, которое Apple использует для потенциальной перекомпиляции и повторной оптимизации бинарника вашего приложения после отправки в App Store. Как метаданные, предназначенные исключительно для оптимизации сборки, bitcode никогда не загружается вашими пользователями.

Если в вашем проекте bitcode отключен, нет необходимости удалять его из фреймворков, которые его содержат. Независимо от того, использует ли ваше приложение bitcode, версия, которую App Store предоставляет вашим пользователям, никогда его не содержит.

## Обновления приложений

При обновлении приложения ваши пользователи загружают только файлы, которые были изменены в этом обновлении. Эти частичные обновления обычно известны как «дельта-обновления». Если ресурсы и двоичные файлы в вашем приложении не изменились, они не будут перезагружены. См. [Уменьшение размера файла обновлений iOS приложений][4], для получения дополнительной информации о том, как использовать преимущества дельта-обновлений.

## Отладочные символы

Обычно framework содержат файлы `dSYM` и `BCSymbolMap`. Они используются для [обозначения сбоев][6] и не включены в приложение, которое загружают пользователи.

## Дополнительно
- [Reducing the size of my App][5]
- [Overview of Dynamic Libraries][7]


[1]: https://help.apple.com/xcode/mac/current/#/devbbdc5ce4f "What is app thinning?"
[2]: https://developer.apple.com/library/content/qa/qa1795/_index.html#//apple_ref/doc/uid/DTS40014195-CH1-MEASURE "Measure Your App"
[3]: https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppDistributionGuide/AppThinning/AppThinning.html#//apple_ref/doc/uid/TP40012582-CH35-SW2 "Bitcode"
[4]: https://developer.apple.com/library/content/qa/qa1779/_index.html "Reducing Download Size for iOS App Updates"
[5]: https://developer.apple.com/library/content/qa/qa1795/_index.html "Reducing the size of my App"
[6]: https://developer.apple.com/library/content/technotes/tn2151/_index.html#//apple_ref/doc/uid/DTS40008184-CH1-SYMBOLICATION "Symbolicating Crash Reports"
[7]: https://developer.apple.com/library/content/documentation/DeveloperTools/Conceptual/DynamicLibraries/100-Articles/OverviewOfDynamicLibraries.html "What Are Dynamic Libraries?"
