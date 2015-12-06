---
layout: post
title: Замена Thread.Sleep в Windows Store приложениях
date: 2014-03-21 20:40
tags:
- C#
- windows
- .Net
---

Пусть вам требуется имитировать медленный вызов некоторой службы.

Обычно для этих целей используется вызов `Thread.Sleep`:

```
System.Threading.Thread.Sleep(3000);
```

Но, в Windows Store приложениях такого метода нет.

Для этих целей используется "асинхронная пауза" по средствам вызова асинхронного метода [Task.Delay](http://msdn.microsoft.com/en-us/library/system.threading.tasks.task.delay.aspx):

```
await Task.Delay(TimeSpan.FromSeconds(30));
```

Асинхронный метод продолжит свое выполнение через 30 секунд, не блокируя основной поток.

#### Пример

```
// ...

await Task.Delay(TimeSpan.FromSeconds(10));

data = await _service.LoadData(); // выполнится через 10 секунд

// ...
```
