---
layout: post
title: Выполнение JavaScript в WebView Xamarin Forms
date: 2017-07-02 00:50
original_url: https://xamarinhelp.com/xamarin-forms-webview-executing-javascript/
tags:
- Xamarin.Forms
- javascript
- C#
- перевод
---


В существующем элементе WebView имеется функция для выполнения JavaScript кода после загрузки страницы, однако она не позволяет получить результат выполнения. В этой статье мы реализуем эту возможность через связываемое свойство (BindableProperty).

## Расширяем WebView

Создадим наследника класса WebView и добавим к нему функцию `EvaluateJavascript`, которая будет принимать строку для выполнения и возвращать строку с результатом.

```csharp
public class WebViewer : WebView
{
    public static BindableProperty EvaluateJavascriptProperty =
    BindableProperty.Create(nameof(EvaluateJavascript), typeof(Func<string, Task<string>>), typeof(WebViewer), null, BindingMode.OneWayToSource);

    public Func<string, Task<string>> EvaluateJavascript
    {
        get { return (Func<string, Task<string>>)GetValue(EvaluateJavascriptProperty); }
        set { SetValue(EvaluateJavascriptProperty, value); }
    }
}
```

## Рендереры

Теперь нам нужно добавить кастомный рендерер для каждой платформы, чтобы реализовать новую функцию.

### Android

```csharp
[assembly: ExportRenderer(typeof(WebViewer), typeof(WebViewRender))]
namespace Mobile.Droid
{
    public class WebViewRender : WebViewRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.WebView> e)
        {
            base.OnElementChanged(e);

            var webView = e.NewElement as WebViewer;
            if (webView != null)
                webView.EvaluateJavascript = async (js) =>
                {
                    var reset = new ManualResetEvent(false);
                    var response = string.Empty;
                    Device.BeginInvokeOnMainThread(() =>
                    {
                        Control?.EvaluateJavascript(js, new JavascriptCallback((r) => { response = r; reset.Set(); }));
                    });
                    await Task.Run(() => { reset.WaitOne(); });
                    return response;
                };
        }
    }

    internal class JavascriptCallback : Java.Lang.Object, IValueCallback
    {
        public JavascriptCallback(Action<string> callback)
        {
            _callback = callback;
        }

        private Action<string> _callback;
        public void OnReceiveValue(Java.Lang.Object value)
        {
            _callback?.Invoke(Convert.ToString(value));
        }
    }
}
```

### iOS

```csharp
[assembly: ExportRenderer(typeof(WebViewer), typeof(WebViewRender))]
namespace Mobile.iOS
{
    public class WebViewRender : WebViewRenderer
    {
        protected override void OnElementChanged(VisualElementChangedEventArgs e)
        {
            base.OnElementChanged(e);

            var webView = e.NewElement as WebViewer;
            if (webView != null)
                webView.EvaluateJavascript = (js) =>
                {
                    return Task.FromResult(this.EvaluateJavascript(js));
                };
        }
    }
}
```

### UWP

```csharp
[assembly: ExportRenderer(typeof(WebViewer), typeof(WebViewRender))]
namespace Mobile.UWP.CustomRenderers
{
    public class WebViewRender : WebViewRenderer
    {
        protected async override void OnElementChanged(ElementChangedEventArgs<WebView> e)
        {
            base.OnElementChanged(e);
            var webView = e.NewElement as WebViewer;
            if (webView != null)
                webView.EvaluateJavascript = async (js) =>
                {
                    return await Control.InvokeScriptAsync("eval", new[] { js });
                };
        }
    }
}
```

## Вызов из модели представления

Первым делом, нам нужно создать свойство в нашей модели представления (ViewModel), как показано ниже.

```csharp
private Func<string, Task<string>> _evaluateJavascript;
public Func<string, Task<string>> EvaluateJavascript
{
    get { return _evaluateJavascript; }
    set { _evaluateJavascript = value; }
}
```

Далее, нужно связать свойство из модели представления с элементом управления (контролом).

```xml
<control:WebViewer Source="{Binding WebViewSource}"
                   EvaluateJavascript="{Binding EvaluateJavascript}, Mode=OneWayToSource}" />
```

Теперь мы можем вызвать нашу функцию из модели представления и получить результат.

```csharp
var result = await EvaluateJavascript("document.getElementById('test');");
```

## Предупреждения

Есть большая ловушка, с которой вы можете столкнуться, выполнение JavaScript, которое придется принять к сведению.

### Android

В Android версии 4.1 и ниже вы можете легко использовать этот JavaScrip и возвращать результат.

```js
document.getElementById('myElement').value;
```

Но, в версии 4.2 изменен JavsScript движок, поэтому когда вы запускаете указанный выше код, он сначала возвращает правильный результат, а потом устанавливает весь объект из DOM в результат выполнения этого сценария. Теперь, если вы снова выполните код выше, ваш скрипт не сможет найти элемент, потому что он больше не существует, т.к. весь ваш DOM - это прошлый результат.

Чтобы обойти эту проблему, вы должны присвоить результат вашего скрипта переменной. Вам не нужно возвращать значение самой переменной, достаточно просто установить ей значение, и оно будет возвращено, не затрагивая DOM существующей страницы.

```js
var x = document.getElementById('myElement').value;
```
