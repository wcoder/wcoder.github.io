---
layout: post
title: Удаление параметров копирования/вставки из UITextField в Xamarin.iOS
date: 2018-04-25 18:10
tags:
- xamarin.ios
- заметка
- сниппет
---

В некоторых случаях нам нужно отключить опции копирования и вставки для полей ввода `UITextField` в iOS, чтобы сделать это, вы должны выполнить следующие шаги:

### Шаг 1

Создайте класс, который наследуется от `UITextField`.

```csharp
[Register(nameof(MyCustomTextField))]
public class CustomTextField : UITextField
```

На этом этапе мы должны помнить о том, чтобы зарегистрировать наш класс, используя атрибут **Register**.

### Шаг 2

Вам необходимо перегрузить метод `CanPerform` и вернуть `false`, чтобы указать, что этот параметр недоступен в элементе управления.

В следующем коде показано как это сделать и для других возможных параметров:

```csharp
using Foundation;
using ObjCRuntime;
using System.Linq;

// ...

public override bool CanPerform(Selector action, NSObject withSender)
{
    switch (action.Name)
    {
        case "paste:":
        case "cut:":
        case "copy:":
        case "_share:":
        case "_define:":
            return false;
    }

    return base.CanPerform(action, withSender);
}
```

Как можно заметить, мы сравниваем имя пришедшего экземпляра `Selector` (что собирается отобразиться) с теми, которые мы хотим игнорировать.

### Шаг 3

И, наконец, вы можете использовать этот класс в любом месте, где вам нужен подобный функционал.

### Результат

Пример, где стандартные параметры отключены и добавлены собственные:

![Демо](https://raw.githubusercontent.com/wcoder/blog/master/uitextfield-paste/image.gif)

### Источники

- [RegisterAttribute](https://developer.xamarin.com/api/type/MonoTouch.Foundation.RegisterAttribute/)
- [canPerformAction(_:withSender:)](https://developer.apple.com/documentation/uikit/uiresponder/1621105-canperformaction)
- [Каждому View по всплывающему меню – Яндекс.Карты iOS](https://medium.com/yandex-maps-ios/tooltip-menu-for-every-view-1aede0d4d3e7)
