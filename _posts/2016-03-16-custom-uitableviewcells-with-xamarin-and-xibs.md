---
layout: posts
title: Создание кастомных ячеек для UITableView с помощью Xamarin и XIB
date: 2016-03-16 23:40
tags:
- ios
- xamarin
---

Иногда, когда мы создаем iOS приложение с помощью Xamarin, мы отказываемся от использования сторибордов (storyboard) и просто создаем пользовательский интерфейс в C# коде. Это прекрасно работает, но когда используем **UITableView**, бывают случаи, когда мы хотим сделать дизайн **UITableViewCell** с помощью дизайнера. К счастью, это достаточно легко сделать с помощью одного *.xib* файла в Xamarin Studio.

###  Создание проекта

Для этого примера, мы начнем с создания пустого проекта.

> File -> New Solution -> iOS -> iPhone -> Empty Project

### Создание таблицы

Далее, добавим новый класс для нашего **UITableViewController**, назовем *MyTableViewController.cs*. В *AppDelegate.cs*, установим RootViewController равным нашему новому классу. Загрузим наши данные в **MyTableViewController**, установив свойство `TableView.Source`.

```csharp
public override void ViewDidLoad ()
{
	base.ViewDidLoad ();

	var conferences = ConferenceRepository.GetConferences ();
	TableView.Source = new MyTableViewSource (conferences);
}
```

### Создание ячейки

К этому моменту, мы еще не использовали дизайнер для нашего UI. На самом деле, у нас еще нет ни одного  *.storyboard* или *.xib* файла в проекте. Для того, чтобы сделать дизайн ячеек нашей таблицы, мы создадим одну сейчас. Мы добавим новый файл, выбрав шаблон *Table View Cell* и назовем его *MyCustomCell.xib*.

> Add -> New File -> iOS -> Table View Cell

![Создание xib файла](https://dl.dropboxusercontent.com/u/30506652/blog/articles/ios_tableview_cell_xib/Untitled.jpg)

Далее, открываем создавшийся *.xib* файл, перетаскиваем элементы управления из панели элементов. Укажем контролам имена, для последующего с ними взаимодействия.

![Дизайн ячейки в Xamarin Studio](https://dl.dropboxusercontent.com/u/30506652/blog/articles/ios_tableview_cell_xib/Untitled2.jpg)

После этого дизайнер сгенерирует нам оутлеты (outlet) для доступа к UI элементам, они будут располагаться в файле *MyCustomCell.designer.cs*

```csharp
using MonoTouch.Foundation;
using System.CodeDom.Compiler;

namespace XibTableCellDesign
{
	[Register ("MyCustomCell")]
	partial class MyCustomCell
	{
		[Outlet]
		MonoTouch.UIKit.UILabel ConferenceDescription { get; set; }

		[Outlet]
		MonoTouch.UIKit.UILabel ConferenceName { get; set; }

		[Outlet]
		MonoTouch.UIKit.UILabel ConferenceStart { get; set; }

		void ReleaseDesignerOutlets ()
		{
			if (ConferenceDescription != null) {
				ConferenceDescription.Dispose ();
				ConferenceDescription = null;
			}

			if (ConferenceName != null) {
				ConferenceName.Dispose ();
				ConferenceName = null;
			}

			if (ConferenceStart != null) {
				ConferenceStart.Dispose ();
				ConferenceStart = null;
			}
		}
	}
}
```

### Добавляем модель

Обратите внимание, что оутлеты - приватные. Поэтому у нас нет доступа к элементам интерфейса из метода `GetCell` нашего **UITableViewSource**, как это обычно делается - `label.Text = value`. Мы можем обойти эту проблему,  добавив публичное свойство в нашей кастомной ячейке, чтобы установить модель данных.

```csharp
public partial class MyCustomCell : UITableViewCell
{
	public Conference Model { get; set; }
}
```

Затем, в методе `LayoutSubviews` нашей ячейки, мы можем установить свойства для элементов ячейки из свойств модели данных.

```csharp
public override void LayoutSubviews ()
{
	base.LayoutSubviews ();

	this.ConferenceName.Text = Model.Name;
	this.ConferenceStart.Text = Model.StartDate.ToShortDateString ();
	this.ConferenceDescription.Text = Model.Description;
}
```

### Использование ячейки

Последний шаг - создать и использовать ячейку в нашей таблице:

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
	var conference = _conferences [indexPath.Row];
	var cell = (MyCustomCell)tableView.DequeueReusableCell (MyCustomCell.Key);
	if (cell == null) {
		cell = MyCustomCell.Create ();
	}
	cell.Model = conference;

	return cell;
}
```

![результат](http://arteksoftware.com/content/images/2014/Aug/CustomTableViewCells-1.png)

Пример приложения можно найти в [репозитории на GitHub](https://github.com/RobGibbens/XibTableCellDesign).

Источник: [Custom UITableViewCells with Xamarin and XIBs](http://arteksoftware.com/custom-uitableviewcells-with-xamarin-and-xibs/)
