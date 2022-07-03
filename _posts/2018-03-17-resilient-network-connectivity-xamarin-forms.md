---
layout: post
title: Отказоустойчивое подключение к сети в Xamarin.Forms & MAUI
date: 2018-03-17 18:25
original_url: https://xamarinhelp.com/resilient-network-connectivity-xamarin-forms/
tags:
- xamarin
- xamarin forms
- MAUI
- перевод
---

Мобильные сети относятся к наименее надежному типу сетей, и вашему приложению, скорее всего, придется общаться через нее, чтобы функционировать. Но, принимая во внимание потенциальность сбоев, мы должны обеспечить наше приложение возможностью приспосабливаться к плохому состоянию сети.

![Сеть](https://xamarinhelp.com/wp-content/uploads/2018/02/cell_network.png)

## HttpClient

В большинстве случаев вы будете подключаться к API через `HttpClient`, как здесь:

```csharp
public class MyApiClass
{
    public readonly HttpClient _client;

    public MyApiClass()
    {
        _client = new HttpClient();
        _client.DefaultRequestHeaders.Accept.Clear();
        // Предполагая, что вы подключаетесь к REST API, который принимает JSON
        _client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
    }

    public async Task GetAsync()
    {
        var result = await _client.GetAsync("https://someurl.com/product/1");
    }

    public async Task SendAsync()
    {
        var product = new Product { Name = "New Product" };

        var json = JsonConvert.SerializeObject(product);

        var result = await _client.SendAsync(new HttpRequestMessage()
        {
            RequestUri = new Uri("https://someurl.com/product"),
            Content = new StringContent(json)
        });
    }
}
```

> **Совет:** Не использовать оператор `using` для `HttpClient`, хранить только один экземпляр для всего приложения. Он был разработан, чтобы работать таким образом.

Есть множество мест, где мы можем потерпеть неудачу.

- `GetAsync`:
  - `ArgumentNullException` - запрос был пустым;
  - `HttpRequestException` - базовая сеть каким-то образом отказала;
  - `InvalidOperationException` - сообщение уже выло отправлено.

Поэтому вы хотели бы обернуть вызовы, чтобы перехватить эти ошибки и разобраться с ними по мере необходимости. Но, допустим ошибки нет, теперь нам нужно проверить возвращаемый код состояния:

```csharp
public async Task GetAsync()
{
    var result = await _client.GetAsync("https://someurl.com/product/1");

    if (result.IsSuccessStatusCode)
    {
        // Успех
    }
    else
    {
        // Возвращен код, отличный от 200
    }
}
```

Если код возвращается, хорошо, вы можете продолжать. Если это не так, то вы должны что-то с этим сделать. 400-е ошибки - это про то, что вы отправили неверный запрос к API, 500-е ошибки означают, что обратиться к API не удалось (ошибка на сервере).

## Проверка подключения

Возможно, ваша страница имеет много сетевых вызовов. Поэтому, возможно, вы захотите сделать проверку что сетевое подключение есть, перед тем как пытаться делать запрос. Вы можете сделать это с помощью плагина [Xam.Plugin.Connectivity](https://github.com/jamesmontemagno/ConnectivityPlugin). Проверить подключение можно так:

```csharp
CrossConnectivity.Current.IsConnected
```

Если значение `false` вам не нужно делать сетевые вызовы. Если значение `true`, то сеть может пропасть уже при выполнении следующей строки кода, в результате чего возникнет `HttpRequestException`.

## Повторное выполнение

Если у вас есть проблемы с подключением, вы, возможно, захотите повторить запрос, если это был всего лишь кратковременный сбой. Вы можете сделать это с помощью [Polly](https://github.com/App-vNext/Polly). Здесь снова тот же метод `GetAsync`, но на этот раз он будет повторяться до трех раз, с двух секундной паузой между запросами, если возникает `HttpRequestException`. Вы также можете добавить дополнительные исключения, при которых нужна повторная попытка.

```csharp
public async Task GetAsync()
{
    var result = await Policy
            .Handle<HttpRequestException>()
            .WaitAndRetry(
                retryCount: 3,
                sleepDurationProvider: (attempt) => TimeSpan.FromSeconds(2))
            .ExecuteAsync(async () => await _client.GetAsync("https://someurl.com/product/1"));

    if (result.IsSuccessStatusCode) { } // Успех
    else { } // Возвращен код, отличный от 200
}
```

## Выводы

Лучший способ избежать проблем с подключением - не делать сетевые вызовы. Если вашему приложению все же нужно ходить в сеть, можно уменьшить число возможных вызовов, реализовав кэширование.

Если вы используете `HttpClient` + [Refit](https://github.com/paulcbetts/refit) (для упрощения вызовов к API) убедитесь, что используете связку правильно.

Убедившись, что вы проверяете возможность подключения, проверьте наличие исключений и, возможно, повторите попытку. Затем проверьте коды состояния.

Теперь вы можете легко заставить приложение работать с ненадежной сетью.
