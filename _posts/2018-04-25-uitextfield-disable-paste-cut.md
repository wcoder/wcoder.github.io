---
layout: post
title: Удаление параметров копирования / вставки в UITextField
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
public override bool CanPerform(ObjCRuntime.Selector action, Foundation.NSObject withSender)
{
    if (action == new ObjCRuntime.Selector("paste:")
        || action == new ObjCRuntime.Selector("cut:")
        || action == new ObjCRuntime.Selector("copy:")
        || action == new ObjCRuntime.Selector("_share:")
        || action == new ObjCRuntime.Selector("_define:"))
    {
        return false;
    }
    return base.CanPerform(action, withSender);
}
```

Вы можете заметить, что вам нужно создать экземпляр класса **Selector**, чтобы сравнить каждый из параметров, которые вы хотите игнорировать, и что идентификатор каждого из параметров - это строка, которую вы должны передать в конструкторе экземпляра селектора.

### Шаг 3

И, наконец, вы можете использовать этот класс в любом месте, где вам нужен подобный функционал.

### Результат

Пример, где стандартные параметры отключены и добавлены собственные:

![Демо](https://raw.githubusercontent.com/wcoder/blog/master/uitextfield-paste/image.gif)

### Источники

- [RegisterAttribute](https://developer.xamarin.com/api/type/MonoTouch.Foundation.RegisterAttribute/)
- [canPerformAction(_:withSender:)](https://developer.apple.com/documentation/uikit/uiresponder/1621105-canperformaction)
