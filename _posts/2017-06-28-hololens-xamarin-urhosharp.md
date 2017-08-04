---
layout: post
title: Введение в UrhoSharp для HoloLens
date: 2017-06-28 13:00
original_url: https://xamarinhelp.com/hololens-xamarin-urhosharp/
tags:
- urhosharp
- hololens
- xamarin
- c#
- 3d
- перевод
---

HoloLens является устройством дополнительной реальности, которое позволяет комбинировать реальный мир с виртуальными объектами. Когда разрабатывают под HoloLens, зачастую используют Unity3D и HoloLens SDK. Unity это коммерческий продукт и не бесплатный для коммерческого использования. Однако, другим вариантом будет использование [UrhoSharp](https://github.com/xamarin/urho), портированной Xamarin версии открытого движка Urho3D для .Net.

## Установка

Даже если у вас нет лишней кучи денег для покупки HoloLens, есть эмулятор, который поможет вам легко начать. Скачать его можно [здесь](https://developer.microsoft.com/en-us/windows/mixed-reality/install_the_tools).

После этого, вы можете создать свой первый UrhoSharp проект, используя готовый шаблон:

![Создаем UrhoSharp проект в Visual Studio](https://xamarinhelp.com/wp-content/uploads/2017/05/HoloLensUrhoProjectCreate.png)

Выберите HoloLens Emulator и запустите приложение.

![Запуск проекта на эмуляторе HoloLens](https://xamarinhelp.com/wp-content/uploads/2017/05/HoloLensEmulator.png)

Вы должны будете увидеть запущенный эмулятор и Луну на орбите Земли.

![Луна на орбите Земли в эмуляторе HoloLens](https://xamarinhelp.com/wp-content/uploads/2017/05/HoloLensEmulatorRunning.png)

## Портал устройства

Прежде чем углубиться в код, стоит отметить портал устройства. Нажмите кнопку "открыть портал устройства" на панели эмулятора.

![Панель эмулятора](https://xamarinhelp.com/wp-content/uploads/2017/05/DevicePortal-1.png)

Вам будет представлен портал, который имеет много полезной информации и функций, таких как ведение журнала и т.п.

![Портал устройства](https://xamarinhelp.com/wp-content/uploads/2017/05/DevicePortalSite.png)

## Приложение HoloLens

В этом простом примере у вас есть Луна и Земля. Две сферы, одна вращается вокруг другой. В [введение в UrhoSharp](https://xamarinhelp.com/introduction-urhosharp-xamarin-forms/) и [перемещение 3D объектов в UrhoSharp](https://xamarinhelp.com/urhosharp-3d-moving-object/) я вдавался в подробности создания базовых объектов.

Отличием приложения для HoloLens, будет наследование от `SteroApplication`.

```csharp
public class HelloWorldApplication : StereoApplication
```

Как и в любом UrhoSharp приложении, у вас есть сцена, и вы создаете дочерние объекты. После того, как дочерний элемент будет создан, вы можете добавлять к нему текстуры и действия. Чтобы получить больше информации об этом, посмотрите предыдущие статьи по UrhoSharp.

```csharp
// создаем узел "Земля"
earthNode = Scene.CreateChild();
```

HoloLens принимает ввод с помощью жестов, поэтому мы можем переопределить эти события. Чтобы имитировать жесты в эмуляторе, обратите внимание на [эту статью](https://developer.microsoft.com/en-us/windows/mixed-reality/using_the_hololens_emulator).

```csharp
public override void OnGestureManipulationStarted() =>
	earthPosBeforeManipulations = earthNode.Position;
public override void OnGestureManipulationUpdated(Vector3 relativeHandPosition) =>
	earthNode.Position = relativeHandPosition + earthPosBeforeManipulations;

public override void OnGestureTapped() { }
public override void OnGestureDoubleTapped() { }
```

Конечно, HoloLens позволяет намного больше чем просто отображение виртуальных объектов. Он также принимает голосовой ввод, распознавание жестов, и коммуникация с веб-сервисами. Примером может быть использование Azure Cognitive Services для распознавания лиц.

## Узнать больше

Этот был очень простой взгляд на то, как начать работать с HoloLens и UrhoSharp. Чтобы начать вникать глубже, посмотрите на [примеры использования UrhoSharp с HoloLens](https://github.com/xamarin/urho-samples/tree/master/HoloLens), которые включают в себя танцующего мутанта, взаимодействие с когнитивными сервисами и многое другое.

