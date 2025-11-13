---
layout: post
title: Сканирование QR-кодов, штрих-кодов в приложениях Xamarin.Forms
redirect_to: "https://ypakala.com"
date: 2014-11-06 06:16
original_url: http://blog.thomaslebrun.net/2014/09/xamarin-scanning-qrcode-in-a-xamarin-forms-application
tags:
- c#
- .net
- xamarin
- ios
- android
- windows phone
- перевод
---

Реализация сканирования QR-кодов, штрих-кодов в приложениях Xamarin.Forms является действительно простой задачей, благодаря  проекту ZXing, который был портирован на Xamarin (и для всех устройств: iOS, Android и Windows Phone).

![ZXing.Net.Mobile](https://components.xamarin.com/resources/icons/component-984/icon_114x114.png)

[Компонент Xamarin](https://components.xamarin.com/view/zxing.net.mobile) / [Проект на GitHub](https://github.com/Redth/ZXing.Net.Mobile)

Для реализации функционала, сначала необходимо добавить ссылку на компонент ZXing в каждом из проектов. Затем, как всегда, мы должны найти способ общения проекта форм (что содержит логику) и различных проектов UI (проект конкретной платформы). Чтобы решить эту проблему, мы используем DependencyService для регистрации интерфейса и созданных, конкретных реализаций.

Интерфейс:

```csharp
public interface IQrCodeScanningService
{
	Task<string> ScanAsync();
}
```

А вот реализация для проекта Windows phone:

```csharp
public class QrCodeScanningService : IQrCodeScanningService
{
    public async Task<string> ScanAsync()
    {
        var scanner = new ZXing.Mobile.MobileBarcodeScanner(App.RootFrame.Dispatcher);
        var scanResults = await scanner.Scan();

        return scanResults.Text;
    }
}
```

Реализация для Android проекта (конечно, таким же образом можно создать реализацию и для iOS):

```csharp
public class QrCodeScanningService : IQrCodeScanningService
{
    public async Task<string> ScanAsync()
    {
        var scanner = new ZXing.Mobile.MobileBarcodeScanner(Application.Context);
        var scanResults = await scanner.Scan();

        return scanResults.Text;
    }
}
```

Как вы можете видеть, каждая реализация является особенной (конструктор MobileBarcodeScanner принимает разные параметры, в зависимости от платформы).

Для каждой реализации, не забудьте добавить атрибут зависимостей так DependencyService может зарегистрировать интерфейс и реализацию:

```csharp
// Windows Phone
[assembly: Dependency (typeof(QrCodeScanningService))]
namespace QrCodeScanningWithXamarin.WinPhone.Helpers

// Android
[assembly: Dependency(typeof(QrCodeScanningService))]
namespace QrCodeScanningWithXamarin.Droid.Helpers
```

И теперь, в проекте Forms (Shared или Portable), мы можем использовать DependencyService, чтобы получить экземпляр класса, который реализует наш интерфейс:

```csharp
var scanButton = new Button
{
    Text = "Launch scan"
};
scanButton.Clicked += async (sender, args) =>
{
    var url = await DependencyService.Get<IQrCodeScanningService>().ScanAsync();

    Device.OpenUri(new Uri(url));
};
```

Запустите приложение и нажмите на кнопку, чтобы отобразить окно позволяющее пользователю сканировать QRcode или штрих-код. После того, как сканер появится на экране, нажмите на него и после автоматической фокусировки, выполнится сканирование и результат возвратится так, что вы сможете использовать его (просмотреть текст, открыть URL, и т.д.)
