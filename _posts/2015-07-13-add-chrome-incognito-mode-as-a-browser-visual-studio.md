---
layout: post
title: Visual Studio - Добавляем Chrome в режиме инкогнито
date: 2015-07-13 26:33
tags:
- программы
- настройка
- visual studio
- google chrome
- internet explorer
---

Небольшая полезняшка для веб-разработчика в Visual Studio. Вы знаете, как VS подхватывает установленные браузеры и позволяет использовать их из выпадающего списка?

![Список доступных браузеров в VS](https://raw.githubusercontent.com/wcoder/blog/master/vs-chrome-incognito/1.png)

Если вы делаете что-нибудь с куками или кэшированием и хотели бы обезопасить себя от очистки истории браузера, лучшим решением было бы запускать браузер в режиме инкогнито. Ниже покажу как это сделать самостоятельно:

### Поехали...

Развернуть список и выбрать пункт `Browse With...`

![Модальное окно Browse With](https://raw.githubusercontent.com/wcoder/blog/master/vs-chrome-incognito/2.png)

Добавить Chrome из стандартного или пользовательского места расположения:

* System: `C:\Program Files (x86)\Google\Chrome\Application\`
* User: `C:\Users\UserName\AppData\Local\Google\Chrome\Application`

Затем добавить `--incognito` как параметр командной строки и название браузера что-то вроде "Google Chrome - Incognito".

Вы можете сделать то же самое с Firefox и Internet Explorer.

Ниже добавляется Internet Explorer с параметром `-private`.

![Internet Explorer в режиме private](https://raw.githubusercontent.com/wcoder/blog/master/vs-chrome-incognito/3.png)

![IE красуется в списке](https://raw.githubusercontent.com/wcoder/blog/master/vs-chrome-incognito/4.png)

Прилагаю ссылку на источник: [Visual Studio Web Development Tip - Add Chrome Incognito Mode as a Browser](http://www.hanselman.com/blog/VisualStudioWebDevelopmentTipAddChromeIncognitoModeAsABrowser.aspx)

На этом все :)
