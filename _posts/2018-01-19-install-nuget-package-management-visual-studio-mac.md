---
layout: post
title: Установка NuGet Package Manager для Visual Studio for Mac
date: 2018-01-19 23:15
tags:
- visual studio
- macos
- заметка
- nuget
---

Чтобы иметь больше возможностей для работы с NuGet в Visual Studio for Mac вам понадобиться установить расширение, для этого вам необходимо выполнить следующие действия:

### Скачать расширение

Переходим к репозиторию с расширением и находим ссылку на подходящую вам версию пакета: 

[mrward/monodevelop-nuget-extensions](https://github.com/mrward/monodevelop-nuget-extensions)

Например для версии студии выше 7.3, скачиваем по данной ссылке:
```
https://github.com/mrward/monodevelop-addins/blob/gh-pages/7.3/MonoDevelop.PackageManagement.Extensions_0.12.6.mpack
```

### Установка расширения

В Visual Studio откройте **&laquo;Extensions Manager&raquo;**

![Visual Studio for Mac меню открытия Extensions Manager](https://raw.githubusercontent.com/wcoder/blog/master/2018-01-18-vs-macos-nuget/1.png)

В открывшемся окне, нажмите кнопку **&laquo;Install from file&raquo;**

![Install from file](https://raw.githubusercontent.com/wcoder/blog/master/2018-01-18-vs-macos-nuget/2.png)

В открывшемся окне выберите ранее скачанный пакет расширения:

![Диалог выбора пакета расширения](https://raw.githubusercontent.com/wcoder/blog/master/2018-01-18-vs-macos-nuget/3.png)

Далее кнопку &laquo;Install&raquo; для подтверждения установки

![Подтверждение установки](https://raw.githubusercontent.com/wcoder/blog/master/2018-01-18-vs-macos-nuget/4.png)

После успешной установки, вы увидите расширение в списке установленных:

![Список установленных расширений](https://raw.githubusercontent.com/wcoder/blog/master/2018-01-18-vs-macos-nuget/5.png)

Теперь **перезапускаем Visual Studio** для вступления изменений в силу.

Готово.

### Проверяем результат

Для того, чтобы открыть NuGet Package Manager: кликните правой кнопкой мыши на названии проекта или солюшена и нажмите &laquo;Manage Packages&raquo;.

![Пункт Manage Packages в контекстном меню](https://raw.githubusercontent.com/wcoder/blog/master/2018-01-18-vs-macos-nuget/6.png)

Расширение дает дополнительные возможности по управлению NuGet пакетами как для проекта, так и для всего решения целиком.

![Окно управления пакетами](https://raw.githubusercontent.com/wcoder/blog/master/2018-01-18-vs-macos-nuget/7.png)

На этом все. Теперь вы сможете можете легко управлять NuGet библиотеками как и версии для Windows.
