---
layout: post
title: Получаем версию UWP приложения из пакета приложения
date: 2016-01-23 20:00
original_url: http://grogansoft.com/blog/?p=1189
tags:
- c#
- uwp
- сниппет
- перевод
---

Это простой сниппет для получения номера версии вашего приложения из пакета. Вы можете отобразить текущий номер версии на вашем UI или использовать в логах отладки, можете использовать актуальную версию пакета как "единый источник правы" для версии вашего приложения.

Получить версию пакета можно из `Windows.ApplicationModel.Package.Current`. Свойство `Version` имеет четыре значения: основная версия, вспомогательная версия, номер ревизии и номер сборки. Вы узнаете их после того, как загрузите пакет вашего приложения в магазин, где он будет иметь номер версии из четырех частей, как `1.1.3.2`.

Код для приведения этих номеров к удобному, строковому представлению:

```csharp
public string AppVersion
{
	get
	{
		var package = Windows.ApplicationModel.Package.Current;
		var packageId = package.Id;
		var version = packageId.Version;
		return $"Version {version.Major}.{version.Minor}.{version.Revision}";
	}
}
```

В этом примере я использовал только три из четырех возможных компонентов версии, поскольку номер сборки не имеет значения для конечного пользователя. Visual Studio может автоматически, постепенно увеличивать номер сборки. Но, вы должны будете сами определить когда повышать номер основной или вспомогательной версии, согластно вашему стилю и потребностям.

Теперь вы должны волноваться только об обновлении версии вашего приложения, когда загрузите новый пакет в магазин. Корректная версия приложения всегда будет отображаться на UI вашего приложения.
