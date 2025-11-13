---
layout: post
title: Включаем опцию анализа кода в Xamarin Studio
redirect_to: "https://ypakala.com"
date: 2015-12-20 15:45
original_url: http://ghost-azureb4c8.azurewebsites.net/resharper-ish-assistance-in-xamarin-studio/
tags:
- перевод
- xamarin studio
---

Все, кто использует Xamarin Studio, могли заметить один из&nbsp;существенных недостатков по&nbsp;сравнению с&nbsp;Visual Studio&nbsp;&mdash; отсутствие Resharper.

Но, у&nbsp;Xamarin Studio есть опция, включающая некоторую дополнительную помощь (анализ кода) в&nbsp;стиле Resharper. Сделать это можно так:
`Preferences -> Text Editor -> Source Analysis` установить опцию `Enable source analysis of open files`.

![Включаем опцию анализа исходного кода открытого файла](https://raw.githubusercontent.com/wcoder/blog/master/2015-12-20-xamarin-studio/1%20-%20EnableSourceAnalysis.png)

После включения опции анализа кода, над вертикальной полосой прокрутки можно увидеть индикатор потенциальных проблем, курсор при этом может выводить подсказки с&nbsp;описанием проблемы. Окрашенные, волнистые подчеркивания используются для выделения проблемных участков.

![Подсказки проблемных участков](https://raw.githubusercontent.com/wcoder/blog/master/2015-12-20-xamarin-studio/2%20-%20StaticAnalysisSquggleAndToolTip.png)

Клавиатурное сочетание `⌥ + ⏎` (для Mac OS) или `Alt+Enter` (для Windows) позволит быстро исправить проблему.

![Исправление проблемы](https://raw.githubusercontent.com/wcoder/blog/master/2015-12-20-xamarin-studio/3%20-%20QuickFix-1.png)

Или щелкните правой кнопкой мыши по&nbsp;проблемной области кода и&nbsp;выберите опцию `Fix`.

![Исправление мышью](https://raw.githubusercontent.com/wcoder/blog/master/2015-12-20-xamarin-studio/4%20-%20FixStaticAnalysisOptions.png)

Другие полезные ссылки для большей производительности с Xamarin Studio можно увидеть на странице оригинала статьи.
