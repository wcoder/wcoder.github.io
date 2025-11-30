---
layout: post
title: Воспроизведение видео в приложении Xamarin Forms
redirect_to: "https://ypakala.com"
date: 2018-02-01 23:10
original_url: https://julianocustodio.com/2018/01/18/videoplayer/
tags:
- android
- ios
- xamarin forms
- video
- перевод
---

Привет, в этом посте я продемонстрирую, как приложение Xamarin.Forms может воспроизводить видео.

Для этого примера я буду считать, что вы уже создали приложение Xamarin.Forms. Если у вас есть какие-либо вопросы по этому поводу, я рекомендую прочитать руководство [«Создание проекта Xamarin.Forms»](https://metanit.com/sharp/xamarin/2.1.php).

## Добавление NuGet пакета

Щелкните правой кнопкой мыши свое решение и выберите «Управление NuGet пакетами для решения»:

![Установка Nuget пакета](https://julianocustodiosite.files.wordpress.com/2018/01/11-e1516143419641.png)

Введите «**Rox.Xamarin.Video**» и выберите плагин, как показано на следующем рисунке:

![Найденый пакет](https://julianocustodiosite.files.wordpress.com/2018/01/2-2-e1516218281793.png)

Выберите все проекты и нажмите кнопку «Установить».

![Выделены все проекты](https://julianocustodiosite.files.wordpress.com/2018/01/2-21-e1516218331731.png)

## XAML

Добавим пространство имен `Rox.Xamarin.Video.Portable`, а затем элемент `VideoView` и настроим некоторые из его свойств:

- **AutoPlay** - автоматическое воспроизведение при загрузке страницы;
- **LoopPlay** - повтор проигрывания видео;
- **ShowController** - показывает панель c элементами управления;
- **Source** - устанавливает видео, которое вы хотите воспроизвести.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage
    xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:local="clr-namespace:DemoVideo"
    xmlns:rox="clr-namespace:Rox;assembly=Rox.Xamarin.Video.Portable"
    x:Class="DemoVideo.MainPage">

    <Grid>
        <rox:VideoView
            AutoPlay="True"
            LoopPlay="True"
            ShowController="True"
            Source="https://instagram.fsod3-1.fna.fbcdn.net/vp/a4483470041412903563bd594a7172f8/5A615E69/t50.2886-16/20845171_798391080343591_101942135397285888_n.mp4" />
    </Grid>

</ContentPage>
```

### Результат

![Результат](https://julianocustodiosite.files.wordpress.com/2018/01/ezgif-com-gif-maker-1.gif?w=299&h=532&zoom=2)

Вот и все, пример [доступен на GitHub](https://github.com/juucustodio/VideoPlayer-Xamarin.Forms)

