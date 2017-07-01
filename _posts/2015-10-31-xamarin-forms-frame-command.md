---
layout: post
title: Выполняем команду при нажатии на Frame
date: 2015-10-31 23:10
tags:
- Xamarin.Forms
- mvvm
---

Для примера, у нас есть следующая разметка:

```xaml
<Frame BackgroundColor="#f39c12" Padding="5">
    <StackLayout VerticalOptions="Center">
        <Image Source="Image/Icon/Play.png" HorizontalOptions="Center" />
        <Label Text="Play" HorizontalOptions="Center" />
    </StackLayout>
</Frame>
```

### Задача:

Необходимо обработать нажатие на Frame с помощью команды.

### Решение:

```xaml
<Frame.GestureRecognizers>
    <TapGestureRecognizer Command="{Binding PlayCommand}" />
</Frame.GestureRecognizers>
```

### Результат:

```xaml
<Frame BackgroundColor="#f39c12" Padding="5">
    <Frame.GestureRecognizers>
        <TapGestureRecognizer Command="{Binding PlayCommand}" />
    </Frame.GestureRecognizers>
    <StackLayout VerticalOptions="Center">
        <Image Source="Image/Icon/Play.png" HorizontalOptions="Center" />
        <Label Text="Play" HorizontalOptions="Center" />
    </StackLayout>
</Frame>
```
