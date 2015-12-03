---
layout: post
title: Памятка по System Tray в Windows Phone 8
date: 2014-11-21 15:07
tags:
- windows phone
- xaml
- памятка
---

### Показать System Tray

```xml
<phone:PhoneApplicationPage
    ...
    xmlns:shell="clr-namespace:Microsoft.Phone.Shell;assembly=Microsoft.Phone"
    shell:SystemTray.IsVisible="True">
    ...
```

### Цвет фона

```
shell:SystemTray.BackgroundColor="Blue"
```

### Цвет текста

```
shell:SystemTray.ForegroundColor="Red"
```

### Прозрачность

```
shell:SystemTray.Opacity="0.5"
```

### Индикатор прогресса ([?](http://wz2.ru/lfp2))

```xml
<shell:SystemTray.ProgressIndicator>
    <shell:ProgressIndicator IsIndeterminate="True" IsVisible="True" Text="Click me..." />
</shell:SystemTray.ProgressIndicator>
```
