---
layout: post
title: Начало работы с Xamarin.Forms 2.0 и Windows 10 Preview
date: 2015-11-29 21:35
original_url: https://blog.xamarin.com/getting-started-with-xamarin-forms-windows-10-preview/
tags:
- Xamarin.Forms
- Xamarin
- Windows 10
- C#
- перевод
---

С выходом [Xamarin 4](https://blog.xamarin.com/introducing-xamarin-4/), вышли и Xamarin.Forms 2.0, включающий новые функиции и оптимизации, чтобы помочь вам создавать больше нативных, кросс-платформенных, мобильных приложений. В дополнение к `ListView` - стратегия кэширования, сужающие жесты и новые свойства, появившиеся в первой публичной превью версии Xamarin.Forms для Windows 10 (UWP) приложений. Хотя Xamarin.Forms уже поддерживает Windows Phone и Windows Store, Windows 10 представляет собой универсальный Windows-проект, который может предназначаться как для настольных, так и мобильных платформ, что позволит вам распрастранить ваше приложение на большее кол-во устройств.

![Промо статьи](https://blog.xamarin.com/wp-content/uploads/2015/11/promo-1024x597.png)

Начало
------

Прежде, чем вы начнете создавать универсальные приложения Windows 10 с Xamarin.Forms, есть несколько необходимых требований, которые должны быть установлены:

- Windows 10
- [Visual Studio 2015](https://www.visualstudio.com/vs-2015-product-editions)
- [Universal Windows Developer Tools](https://dev.windows.com/en-us/downloads/windows-10-sdk)

Как только вы все установили, пора перейти к готовому Xamarin.Forms проекту. Xamarin.Forms 2.0 не только добавляют предварительную поддержку для приложений Windows 10, но и множество дополнительных функций и оптимизаций для ваших мобильных приложений.

Для начала зайдем в менеджер Nuget пакетов вашей Visual Studio.

![Управление Nuget пакетами](https://blog.xamarin.com/wp-content/uploads/2015/11/Manage-NuGet-Packages-1024x592.png)

Выберите фильтр "Upgrade available" и обновите все ваши Nuget пакеты, для Xamarin.Forms.

![Обновление пакетов](https://blog.xamarin.com/wp-content/uploads/2015/11/UpgradeNuGets-1024x609.png)

> Visual Studio может предложить перезапуститься после этого шага.

Добавляем универсальное приложение Windows
------------------------------------------

После того как все ваши Nuget пакеты обновлены, настало время, чтобы добавить пустой универсальный проект Windows. Его можно найти в разделе `Add New Project -> Windows -> Universal -> Blank App`.

![Шаблон UWP проекта](https://blog.xamarin.com/wp-content/uploads/2015/11/UWP-Project-Template-1024x207.png)

Добавление Nuget пакетов & PCL/Shared Project ссылок
----------------------------

Когда вы создаете новый проект Xamarin.Forms, все необходимые ссылки и NuGet-пакеты  добавляются автоматически. Однако для существующих проектов Xamarin.Forms, которые хотят поддерживать UWP, формы должны быть добавлены вручную. Как? Во-первых, добавьте Xamarin.Forms. Щелкните правой кнопкой по вашему решению (Solution) и найдите пункт "Manage NuGet Packages…" и в фильтре выберите "Installed". Найдите Xamarin.Forms и выберите UWP проект для установки последней версии пакета.

![Добавление Xamarin.Forms в UWP проекту](https://blog.xamarin.com/wp-content/uploads/2015/11/Add-Forms-to-UWP-1024x594.png)

Теперь, добавьте ваш общий код, который содержит Модели, Представления и ViewModels. В приложении UWP щелкнуть правой кнопкой по "References" и выбрать "Add Reference". В пункте "Projects" вы найдете все проекты в решении. Найдите свой PCL или Shared проект, проверьте его и нажмите "OK".

![Управление зависимостями](https://blog.xamarin.com/wp-content/uploads/2015/11/Reference-Manager1-1024x511.png)

Обновление App.xaml.cs
----------------------

У всех приложений Windows есть `App.xaml` для "низкоуровневой" конфигурации приложения. Точно так же, как XAML страница в Xamarin.Forms, у этой страницы есть C# code-behind для обработки событий жизненного цикла. Откройте его, и вы найдете метод `OnLaunched`.

Найдите следующую строку кода:

``` csharp
rootFrame.NavigationFailed += OnNavigationFailed;
```

и добавьте вызов Xamarin.Forms метода `Init`:

``` csharp
Xamarin.Forms.Forms.Init (e);
```

Обновление MainPage.xaml
------------------------

Откройте `MainPage.xaml` приложения Windows 10 и удалите `Grid` по умолчанию, чтобы теперь страница была абсолютно пуста. Затем, добавьте новое пространство имен в XAML и измените `Page`, чтобы была Xamarin.Forms `Page`:

``` xml
<forms:WindowsPage xmlns:forms="using:Xamarin.Forms.Platform.UWP"
	...>
</forms:WindowsPage>
```

Обновление MainPage.xaml.cs
---------------------------

Обновите code-behind файл, чтобы инициализировать ваш Xamarin.Forms приложение:

``` csharp
public sealed partial class MainPage
{
	public MainPage()
	{
		InitializeComponent();
		LoadApplication (new YOUR_NAMESPACE.App());
	}
}
```

Замените `YOUR_NAMESPACE` пространством имен, которое используете вы, в котором находится класс `App.cs`.

Конфигурация приложения и деплой
--------------------------------

Иногда Visual Studio может не собирать ново-добавленный проект. Чтобы гарантировать, что проект будет собираться и деплоиться на устройство, щелкните правой кнопкой по решению и откройте менеджер конфигурации. Проверьте приложение UWP, флаги `build` и `deploy`, они должны быть активны!

![Менеджер конфигураций](https://blog.xamarin.com/wp-content/uploads/2015/11/Configuration-Manager1-1024x642.png)

Добавьте изображения и другие Nuget пакеты
-------------------------------------------

Последний шаг, просто добавить любое изображение или любой из Nuget пакетов или плагинов для Xamarin, которые вы можете иметь в качестве зависимостей. Убедитесь, что каждая из этих зависимостей имеей поддержку Windows 10 (UWP), перед использованием.

## Узнать больше, обратная связь

Чтобы узнать больше обо всех новых функциях в [Xamarin.Forms 2.0](https://developer.xamarin.com/releases/xamarin-forms/xamarin-forms-2.0/2.0.0/) обязательно прочтите примечания к выпуску. Полный исходный код примера (приложения MyWeather), вы можете посмотреть на [GitHub](https://github.com/jamesmontemagno/MyWeather.Forms). Поддержка Windows 10 (UWP) находится еще не в финальной стадии, поэтому есть возможность, что вы можете столкнуться с некоторыми шероховатостями. Помогите сделать Xamarin.Forms для UWP лучше, пожалуйста, не забудьте оставить отзыв, [сообщив о проблеме](https://bugzilla.xamarin.com/enter_bug.cgi?alias=&assigned_to=&attachurl=&blocked=&bug_file_loc=http%3A%2F%2F&bug_severity=normal&bug_status=NEW&cf_tags=&comment=&contenttypeentry=&contenttypemethod=autodetect&contenttypeselection=text%2Fplain&data=&deadline=&dependson=&description=&estimated_time=&form_name=enter_bug&maketemplate=Remember%20values%20as%20bookmarkable%20template&op_sys=Mac%20OS&product=Forms&rep_platform=PC&short_desc=&target_milestone=UWP&version=1.5.1), если таковые возникли.

----
