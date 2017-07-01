---
layout: post
title: Ваше приложение готово к iOS9?
date: 2015-11-07 18:25
original_url: https://blog.xamarin.com/is-your-app-ready-for-ios-9/
tags:
- перевод
- xamarin
- Xamarin.Forms
- ios
---

iOS 9 вышла около месяца назад, и испытывает самый быстрый темп внедрения чем когда-либо для iOS.

Когда пользователи обновляются, они ожидают приложения, с которыми они взаимодействуют чтобы использовать новейшие возможности. В iOS9 появилось много [новых функций](http://https/developer.xamarin.com/guides/ios/platform_features/introduction_to_ios9/), которыми вы можете воспользоваться, но в этом после мы опишем наиболее популярные из них.

## Совместимость с iOS9

Даже если вы не планируете добавлять возможности iOS9 в ваши приложения, вы должны [пересобрать их с помощью последней версии Xamarin](https://developer.xamarin.com/guides/ios/platform_features/ios9/). Если у вас уже есть приложения в магазине, которые предназначены для iOS8 или более ранних версий системы, и еще не пересобраны с помощью XCode 7 и Xamarin.iOS 9 то, это первое что вы должны сделать чтобы поддерживать iOS9.

## Многозадачность в iPad

iOS9 предлагает новые способности для [многозадачности](http://developer.xamarin.com/guides/ios/platform_features/introduction_to_ios9/multitasking/) на iPad с введением *Slide Over*, *Split View* (только для iPad Air 2, iPad Mini 4 и iPad Pro) и *Picture in Picture*

![iOS9 многозадачность](https://blog.xamarin.com/wp-content/uploads/2015/08/io-9-split-view-multitasking-300x216.jpg)

Чтобы поддержать многозадачность в iPad, вы должны сделать следующее:

1. Билд для iOS9 (или выше).
2. Использовать storyboard (или `.xib`) файл для
3. Использовать storyboard с [Auto Layout](https/developer.xamarin.com/guides/ios/user_interface/designer/designer_auto_layout/) и [Size Classes](https://developer.xamarin.com/guides/ios/platform_features/introduction_to_unified_storyboards/#Size_Classes) для построения ваших интерфейсов.
4. Поддержите все четыре ориентации устройства: Portrait, Upside-down Portrait, Landscape Left, и Landscape Right.

Это — один из многих шагов, которые Apple сделала для того, чтобы разработчики использовали Storyboards с Auto Layout вместо создания представления в code-behind файле. Конечно, если ваше приложение должно занять весь экран, потому что это — игра, приложение камеры и т.д., есть способ отключить многозадачность в вашем `Info.plist`.

## App Transport Security

[App Transport Security](http://developer.xamarin.com/guides/ios/platform_features/introduction_to_ios9/ats/) (ATS)  обеспечивает безопасное соединение между интернет-ресурсов и приложением, либо напрямую через ваше приложение или библиотеку, которая это потребляет. В приложениях, собранных для iOS9 или выше, все соединения, использующие `NSUrlConnection`, `CFUrl`, или `NSUrlSession` подвергнутся требованиям безопасности ATS и завершатся с исключением, если они не соответствуют требованиям.

Если вы спешите поскорее залить свои iOS9 приложения в App Store, и не можете быстро обеспечить безопасность всех соединений, вы можете отказаться от ATS добавив `NSExcpetionDomans` к вашему `Info.plist`. Это будет выглядеть так:

```xml
<key>NSAppTransportSecurity</key>
<dict>
	<key>NSExceptionDomains</key>
	<dict>
		<key>www.the-domain-name.com</key>
		<dict>
			<key>NSExceptionMinimumTLSVersion</key>
			<string>TLSv1.0</string>
			<key>NSExceptionRequiresForwardSecrecy</key>
			<false/>
			<key>NSExceptionAllowsInsecureHTTPLoads</key>
			<true/>
			<key>NSIncludesSubdomains</key>
			<true/>
		</dict>
	</dict>
</dict>
```

Для получения дополнительной информации посетите [Xamarin Developer Center](https://developer.xamarin.com/guides/ios/platform_features/introduction_to_ios9/ats/#Opting-Out_of_ATS).

## 3D Touch

Новинка в iPhone 6S и 6S Plus. [3D Touch](https://developer.xamarin.com/guides/ios/platform_features/introduction_to_ios9/3dtouch/) добавляет три различные, чувствительные к давлению, жеста в ваши iOS приложения. Добавление [Pressure Sensitivity](https://developer.xamarin.com/guides/ios/platform_features/introduction_to_ios9/3dtouch/#Pressure_Sensitivity), [Peek and Pop](https://developer.xamarin.com/guides/ios/platform_features/introduction_to_ios9/3dtouch/#Peek_and_Pop) и/или [Quick Actions](https://developer.xamarin.com/guides/ios/platform_features/introduction_to_ios9/3dtouch/#Quick_Actions) к вашему приложению позволит пользователям взаимодействовать с приложением как никогда прежде. Поддержка 3D Touch не обязательна но, пользователи с новейшими аппаратными средствами будут ожидать 3D Touch в своих приложениях, таким образом, вы должны будете рассмотреть добавление его к вашему Xamarin приложению, чтобы улучшить опыт взаимодействия ваших пользователей с приложением и гарантировать, что ваше приложение выделится среди конкурентов.

## App Search

Новое API [App Search](https://developer.xamarin.com/guides/ios/platform_features/introduction_to_ios9/search/) предоставляет несколько различных способов, индексации информации которая находится в пределах вашего приложения. Вы можете индексировать информацию конфеденциально для пользователя, позволяя им искать через spotlight в вашем приложении. Также вы можете индексировать содержимое вашего приложения публично, используя общий API поиска Apple, который позволит вашему приложению быть в результатах поиска пользователей, которые его еще не установили. Например, если ваше приложение для 7-минутных тренировок и пользователь ищет в spotlight (или просит Siri), тогда ваше приложение может показаться как предложение перед ним или ею.

Какой контент вы должны индексировать? Apple предоставляет следующие предложения по этому вопросу:

- любой просмотренный контент, созданный или курируемый пользователем в вашем приложении.
- точки навигации и фичи в приложении.
- такие вещи как новые сообщения, контент или другие типы элементов, отображаемых приложением, которые были недавно загружены на устройство.

Чем проще для пользователей найти то, что они ищут, тем более вероятно, что они воспользуются вашим приложением, так что думайте о том, как App Search мог бы воспользоваться вашим приложением.

## Ресурсы

Чтобы сделать это как можно проще для вас, чтобы принести поддержку iOS9 в ваши приложения сегодня, ознакомьтесь с нашими [бесплатными лекциями в Xamarin University](https://university.xamarin.com/lightninglectures/updating-your-xamarinios-apps-to-ios9) по обновлению приложений для iOS9. Для полного, детального списка новых функций доступных в iOS9, взгляните на [Introduction to iOS 9](http://developer.xamarin.com/guides/ios/platform_features/introduction_to_ios9/). Обучаетесь на практике? Мы так же имеем много [примеров](http://developer.xamarin.com/samples/ios/iOS9/) для вас, чтобы проверить, увидеть как реализовать те или иные крутые фичи iOS9. Добавить поддержку iOS9 в ваши Xamarin.Forms приложения также легко! Craig Dunn [написал в блоге](http://conceptdev.blogspot.com.by/2015/09/ios-9-ify-your-xamarinforms-app_29.html) подробно, как это сделать в Xamarin.Forms приложении.
