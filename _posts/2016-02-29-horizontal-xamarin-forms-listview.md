---
layout: post
title: Горизонтальный ListView в Xamarin.Forms
date: 2016-02-29 23:50
original_url: http://liamtait.com/horizontal-xamarin-forms-listview/
tags:
- Xamarin.Forms
- ios
- android
- windows phone
- перевод
---

И так, вы хотите привязать некоторые данные, но хотите чтобы они были расположены горизонтально.

<video autoplay loop><source id="webmSource" src="https://zippy.gfycat.com/DefiantOffensiveDodo.webm" type="video/webm"><source id="mp4Source" src="https://zippy.gfycat.com/DefiantOffensiveDodo.mp4" type="video/mp4">
    Sorry, your browser doesn't support HTML5 video.
</video>

Простой способ добиться этого - это использование существующего в `ListView` свойства `Rotation` чтобы повернуть ListView по горизонтали или повернуть ваш контент в обратном направлении по вертикали.

В моем случае я хотел, чтобы мой список был во `Frame`, хотя, это может быть любой `View`, который может содержать элемент.

`RelativeLayout` позволяет задать высоту для `ListView` это будет значение `Width` и наоборот.

Делается это так:

```
RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=1}"
```

и

```
RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent,Property=Height,Factor=1}"
```

Однако, когда происходит вращение, точка разворота меняет расположение элемента, поэтому мы также должны:

Установить якорь для нашей позиции:

```
AnchorX="0"
AnchorY="0"
```

и, ограничить элемент:

```
RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=1}"
RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent,Property=Height,Factor=1}"
```

Я использовал

```
HorizontalOptions="Center"
VerticalOptions="StartAndExpand"
```

но, это на ваш вкус. Чтобы список выглядел лучше.

Теперь самая важная часть, поворот `ListView`:

```
Rotation="270"
```

Теперь у нас есть список cбоку. Мы хотим, чтобы наш контент располагался правой стороной кверху (включая скроллинг).

В нашем `ViewCell` мы устанавливаем `ContentView` имеющее свойство:

```
Rotation="90"
```

Это таким же образом будет работать и для любого другого `View`, поэтому может быть заменено на `Grid`, `StackLayout` и др.

`RowHeight` указывает ширину ячеек вашего `ListView`.

Полный исходный код:

```xml
<Frame
	x:Name="Attachments"
	IsClippedToBounds="true"
	IsVisible="false"
	Padding="0"
	BackgroundColor="White"
	HasShadow="false"
	Grid.Row="5"
	Grid.RowSpan="1"
	Grid.Column="1"
	Grid.ColumnSpan="3">
	<RelativeLayout>
		<ListView
			x:Name="AttachedList"
			SeparatorVisibility="None"
			AnchorX="0"
			AnchorY="0"
			RowHeight="120"
			RelativeLayout.XConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=0}"
			RelativeLayout.YConstraint="{ConstraintExpression Type=RelativeToParent, Property=Height,Factor=1}"
			RelativeLayout.HeightConstraint="{ConstraintExpression Type=RelativeToParent, Property=Width,Factor=1}"
			RelativeLayout.WidthConstraint="{ConstraintExpression Type=RelativeToParent,Property=Height,Factor=1}"
			Rotation="270"
			HorizontalOptions="Center"
			VerticalOptions="StartAndExpand">
			<ListView.ItemTemplate>
				<DataTemplate>
					<ViewCell>
						<ContentView
							Padding="5,15,5,5"
							Rotation="90"
							HorizontalOptions="CenterAndExpand"
							VerticalOptions="CenterAndExpand">
							<Image
								HorizontalOptions="CenterAndExpand"
								VerticalOptions="CenterAndExpand"
								Source="http://placehold.it/250x350?text=PlaceholderPic"
								Aspect="AspectFill" />
						</ContentView>
					</ViewCell>
				</DataTemplate>
			</ListView.ItemTemplate>
		</ListView>
	</RelativeLayout>
</Frame>
```
