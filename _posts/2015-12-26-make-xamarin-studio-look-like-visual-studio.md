---
layout: post
title: Делаем Xamarin Studio похожей на Visual Studio
date: 2015-12-26 18:30
tags:
- xamarin studio
- visual studio
- перевод
---


Поздравьте меня! Я отмечаю свою первую неделю мобильной разработки без использования Visual Studio. Xamarin Studio была моей средой разработки в течении прошлой недели и я до сих пор наслаждаюсь ею. Я обнаруживаю новые возможности ежедневно и постоянно убеждаю свих коллег использовать их. Единственное, что позволит вам быстрее переключиться - сделать Xamarin Studio похожей на Visual Studio. :smile: Я покажу вам, как!

Во-первых, шрифт. Я привык к приятному шрифту *Consolas* в Visual Studio. Так, первым делом стало поменять шрифт на *Consolas*. Но эй, подождите... моя OSX не имеет стандартного шрифта с названием *Consolas*. Поэтому пришлось обратиться к гуглу и найти бесплатный шрифт для скачивания на [Font Palace](http://www.fontpalace.com/font-details/Consolas/). После скачивания и [установки шрифта](http://macs.about.com/od/usingyourmac/qt/How-To-Install-Fonts-In-Os-X.htm) на мой Mac, я установил его в настройках текстового редактора (`Xamarin Studio > Preferences > Environment > Fonts`):

![установка шрифта](https://raw.githubusercontent.com/wcoder/blog/master/2015-12-26/1-Screen-Shot-2015-12-18-at-15.20.27-768x241.png)

Еще одна вешь, которую я заметил: цветовая схема была другой. В Xamarin Studio, мы можете изменить опцию `Xamarin Studio > Preferences > Text Editor > Syntax Highlighting`. Изменить на `Visual Studio`:

![изменение цветовой схемы](https://raw.githubusercontent.com/wcoder/blog/master/2015-12-26/2-Screen-Shot-2015-12-18-at-15.25.32-768x265.png)

Конечно, мы можете выбрать другую цветовую схему, они так же хороши, или создать свою.

Следующее, что нужно изменить, настройки `Xamarin Studio > Preferences > Source Code`. В пункте `.NET Naming Policies` измените именование пространств имен - соблюдать структуру папок в иерархическом порядке. Так:

![политика именования](https://raw.githubusercontent.com/wcoder/blog/master/2015-12-26/3-Screen-Shot-2015-12-18-at-15.33.20.png)

И для форматирования C# кода (`Xamarin Studio > Preferences > Source Code > Code Formatting > C# source code`) вы можете выбрать предустановленные политики Visual Studio:

![форматирование C#](https://raw.githubusercontent.com/wcoder/blog/master/2015-12-26/4-Screen-Shot-2015-12-18-at-15.35.15.png)

Я должен признать, что я отключил преобразование табуляции в пробелы, чтобы использовать табуляцию - попросите членов вашей команды сделать то же самое, чтобы иметь одинаковые отступы.

Некоторые другие вещи, для включения:

- Source Analysis (анализ кода)
  ![анализ кода](https://raw.githubusercontent.com/wcoder/blog/master/2015-12-26/5-Screen-Shot-2015-12-18-at-15.38.38.png)
- Включить Code Folding (свертывание кода)
  ![свертывание кода](https://raw.githubusercontent.com/wcoder/blog/master/2015-12-26/6-Screen-Shot-2015-12-18-at-15.39.22.png)
- Format Document On Save (форматирование кода при сохранении, аналог `Ctrl+K+D` в Visual Studio)
  ![форматирование кода](https://raw.githubusercontent.com/wcoder/blog/master/2015-12-26/7-Screen-Shot-2015-12-18-at-15.40.23.png)

Конечно, есть и более интересные настройки для изменения правил именования, но все они более персональные.

Счастливой разработки с Xamarin Studio!

[Оригинал](http://stefandevo.com/2015/12/18/make-xamarin-studio-look-like-visual-studio/)  
