---
layout: post
title: Отключаем выделение выбранного элемента ListView с использованием эффектов в Xamarin Forms
date: 2018-02-02 23:05
original_url: https://iwritecodesometimes.net/2018/02/01/xamawin-override-select-state-using-effects/
tags:
- android
- ios
- xamarin forms
- сниппет
- перевод
---

ListView в Xamarin.Forms - это эффектный способ отображения повторяющихся элементов одинакового формата. Типичное применение включает какое-то взаимодействие с пользователем, например выбрать один элемент, а затем перейти на новый экран.

В этой статье мы рассмотрим возможность отключения выделения для элементов списка. Хотя это просто в UWP, с использованием `SelectionMode`, Xamarin.Forms не предоставляет аналогичную функциональность из коробки (пока!), из-за различия способов, которым каждая платформа обрабатывает списки. К счастью, благодаря нашему хорошему другу PlatformEffect, реализовать эту функциональности - просто!

Начнем с эффекта в iOS проекте:

```csharp
[assembly: ResolutionGroupName("MyEffects")]
[assembly: ExportEffect(typeof(ListViewHighlightEffect), nameof(ListViewHighlightEffect))]
namespace Effects.iOS.Effects
{
    public class ListViewHighlightEffect : PlatformEffect
    {
        protected override void OnAttached()
        {
            var listView = (UIKit.UITableView)Control;

            listView.AllowsSelection = false; // !!!
        }

        protected override void OnDetached()
        {

        }
    }
}
```

Когда вы обратите внимание на Android, вы заметите, что эффект реализуется аналогично:

```csharp
[assembly: ResolutionGroupName("MyEffects")]
[assembly: ExportEffect(typeof(ListViewHighlightEffect), nameof(ListViewHighlightEffect))]
namespace Effects.Droid.Effects
{
    public class ListViewHighlightEffect : PlatformEffect
    {
        protected override void OnAttached()
        {
            var listView = (Android.Widget.ListView)Control;

            listView.ChoiceMode = ChoiceMode.None; // !!!
            listView.SetSelector(Android.Resource.Color.Transparent); // !!!
        }

        protected override void OnDetached()
        {

        }
    }
}
```
Вдобавок, мы можем сделать выделение прозрачным, [переопределив](https://gist.github.com/wcoder/0f2dc9cfe230764cc9caa7b5234a3fab#file-styles-xml-L22-L26) значения цветов в styles.xml

Теперь, мы просто применим эффект для нашего списка, вот так:

```csharp
MyListView.Effects.Add(Effect.Resolve("MyEffects.ListViewHighlightEffect"));
```

Результатом является отсутствие выделения элементов списка при выборе или прокрутке. Проверьте как это выглядит до и после на iOS:

<table><tr><td>
<img src="https://iwritecodesometimes.files.wordpress.com/2018/01/jan-31-2018-14-33-18.gif?w=346&h=629&zoom=2" />
</td><td>
<img src="https://iwritecodesometimes.files.wordpress.com/2018/01/jan-31-2018-14-34-12.gif?w=346&h=629&zoom=2" />
</td></tr></table>

