---
layout: post
title: Шаг за шагом добавляем Lottie анимации в Xamarin Forms приложение
redirect_to: "https://ypakala.com"
date: 2017-07-24 15:56
original_url: https://xamgirl.com/lottie-animations-step-by-step-in-xamarin-forms/
tags:
- xamarin forms
- ios
- android
- перевод
---


![Шаг за шагом добавляем Lottie анимации в Xamarin Forms приложение](https://xamgirl.com/wp-content/uploads/2017/06/edd7cb_2dec6254a38c41d189698c1dd6ef12c2-mv2-665x435.png)

Lottie это библиотека, созданная Airbnb и позволяет вам запускать анимации. Она работает с помощью json-файла, который предоставляет контент для отображения анимации. Спасибо [Martijn van Dijk](https://github.com/martijn00) что теперь эта библиотека доступна для Xamarin Forms. Для подробного ознакомления, посмотрите его репозиторий: [https://github.com/martijn00/LottieXamarin](https://github.com/martijn00/LottieXamarin). Конечно, я бы еще рекомендовал ознакомиться с его статьей: [Bring Stunning Animations to Your Apps with Lottie](https://blog.xamarin.com/bring-stunning-animations-to-your-apps-with-lottie/)

На некоторых форумах, я видел как люди были разочарованы тем, что у них Lottie не работает, поэтому в этой посте я постараюсь показать как заставить Lottie работать, шаг за шагом.

## Шаг 1. Установка

Установите [Lottie for Xamarin.Forms](https://www.nuget.org/packages/Com.Airbnb.Xamarin.Forms.Lottie/2.0.0.1-alpha1), во все ваши проекты.

![Установка пакета Lottie](https://xamgirl.com/wp-content/uploads/2017/06/Screen-Shot-2017-06-22-at-12.08.49-AM-768x515.png)

## Шаг 2. Инициализация

Инициализируйте Lottie в обоих проектах iOS и Android с помощью вызова `AnimationViewRenderer.Init()`

![Инициализация в Android проекте](https://xamgirl.com/wp-content/uploads/2017/06/Screen-Shot-2017-06-22-at-12.18.33-AM-768x459.png)

![Инициализация в iOS проекте](https://xamgirl.com/wp-content/uploads/2017/06/Screen-Shot-2017-06-22-at-12.22.07-AM-768x401.png)

## Шаг 3. Применение

Вы можете добавить **AnimationView** из кода, или если это XAML страница:

```xml
xmlns:forms="clr-namespace:Lottie.Forms;assembly=Lottie.Forms"
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:LottieSample"
             x:Class="LottieSample.LottieSamplePage"
             xmlns:forms="clr-namespace:Lottie.Forms;assembly=Lottie.Forms">
    <ContentPage.Content>
         <StackLayout VerticalOptions="FillAndExpand" HorizontalOptions="FillAndExpand">
              <forms:AnimationView
                x:Name="AnimationView"
                Animation="video_cam.json"
                AutoPlay="True" Loop="true"
                VerticalOptions="FillAndExpand"
                HorizontalOptions="FillAndExpand" />
      </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

## Шаг 4. Выбор анимации

Выберите анимацию, созданную в [Adobe After Effects](https://www.adobe.com/products/aftereffects.html) и экспортируйте ее в json-файл, используя расширение [Bodymovin](https://github.com/bodymovin/bodymovin).

## Шаг 5. Добавление Json-файла в проект

В iOS добавьте его в свой проект и убедитесь, что в **Build Action** указан **BundleResource**

![](https://xamgirl.com/wp-content/uploads/2017/06/Screen-Shot-2017-06-22-at-12.25.13-AM-768x434.png)

В Android поместите его в папку **Assets** и убедитесь, что в **Build Action** указан **AndroidAsset**

![](https://xamgirl.com/wp-content/uploads/2017/06/Screen-Shot-2017-06-22-at-12.26.52-AM-768x662.png)

И это все, что вам нужно, чтобы ваша анимация работала!

![Результат](https://i.imgflip.com/1razf6.gif)

[Исходный код](https://github.com/CrossGeeks/Xamarin.Samples/tree/master/Xamarin%20Forms/LottieSample)
