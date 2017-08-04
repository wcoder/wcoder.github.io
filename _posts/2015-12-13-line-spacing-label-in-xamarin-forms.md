---
layout: post
title: Межстрочный интервал для Label в Xamarin.Forms
date: 2015-12-13 14:30
original_url: http://thetechstudio.ghost.io/line-spacing-label-in-xamarin-forms/
tags:
- c#
- xamarin
- xamarin forms
- ios
- android
- перевод
---

Одним из ограничений контрола `Label` в Xamarin.Forms является отсутствие свойства для межстрочного интервала. Поэтому в этом посте мы разберем, как это сделать.

## Контрол для форм

Контрол для форм создается очень просто, для этого понадобиться лишь добавить одно свойство:

``` csharp
public class LineSpacingLabel : Label
{
    public double LineSpacing { get; set; }
}
```

Я назвал этот контрол для простоты `LineSpacingLabel`.

Настоящая магия происходит в рендерерах для iOS и Android.

## iOS рендерер

Код рендерера:

```csharp
var lineSpacingLabel = (LineSpacingLabel)this.Element;
var paragraphStyle = new NSMutableParagraphStyle()
{
    LineSpacing = (nfloat)lineSpacingLabel.LineSpacing
};
var string = new NSMutableAttributedString(lineSpacingLabel.Text);
var style = UIStringAttributeKey.ParagraphStyle;
var range = new NSRange(0, string.Length);

attrString.AddAttribute(style, paragraphStyle, range);

this.Control.AttributedText = string;
```

Нам нужно свойство `Text`, поэтому добавим этот код в метод `OnElementPropertyChanged`.

## Android рендерер

Код рендерера:

```csharp
protected LineSpacingLabel LineSpacingLabel { get; private set; }

protected override void OnElementChanged(ElementChangedEventArgs<Label> e)
{
    base.OnElementChanged(e);

    if (e.OldElement == null)
    {
        this.LineSpacingLabel = (LineSpacingLabel)this.Element;
    }

    var lineSpacing = this.LineSpacingLabel.LineSpacing;

    this.Control.SetLineSpacing(1f, (float)lineSpacing);

    this.UpdateLayout();
}
```

Здесь нам не нужно полагаться на наличие свойства `Text`, поэтому мы просто добавим код в метод рендерера `OnElementChanged`.

## Дополнительная литература

Пару подробных статей о важности получения правильного междустрочного интервала:

- [Line spacing - Butterick's Practical Typography](http://practicaltypography.com/line-spacing.html)
- [Line Spacing For Text - Fonts.com](http://www.fonts.com/content/learning/fontology/level-2/text-typography/line-spacing-for-text)

