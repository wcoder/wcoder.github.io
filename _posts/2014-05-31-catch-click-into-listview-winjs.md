---
layout: post
title: Как поймать событие нажатия кнопки внутри WinJs ListView?
redirect_to: "https://ypakala.com"
date: 2014-05-31 21:08
tags:
- winjs
- windows 8
- javascript
---

## Задача

Поймать события клика по картинке, находящейся внутри ListView, без вызова события `iteminvoked` самого списка.

## Решение

Рассмотрим на примере:

### Шаг 1

```html
<div id="itemsListTemplate" data-win-control="WinJS.Binding.Template">
	<div>
		<img src="#" data-win-bind="alt: title; src: picture" />
		<div>
			<h4 data-win-bind="innerText: title"></h4>
		</div>
		<div>
			<img id="likeId" src="images/like.png" data-win-bind="alt:title; onclick: clickHandler;" />
		</div>
	</div>
</div>

<div id="userListView"
		data-win-control="WinJS.UI.ListView"
		data-win-options="{
			selectionMode:'single',
			itemTemplate:select('#itemsListTemplate'),
			layout:{type:WinJS.UI.GridLayout}
		}">
</div>
```

### Шаг 2

Теперь создадим источник данных.

```js
var userArray = [
	{
		title: " James Brito",
		picture: "images/JamesBrito.png"
	},
	{
		title: "Alex",
		picture: "images/alex.png"
	},
	{
		title: "Aleph",
		picture: "images/Aleph.png"
	},
	{
		title: "Richard",
		picture: "images/Richard.png"
	}
];

// добавление события сlick
for (var i = 0; i < userArray.length; i++) {
	// http://msdn.microsoft.com/en-us/library/windows/apps/hh967819.aspx
	userArray[i].clickHandler = WinJS.Utilities.markSupportedForProcessing(function (e) {
	
		UserLike(e);
		
	});
}

var userList = new WinJS.Binding.List(userArray),
	userListView = document.getElementById("userListView");
	
if (userListView) {
	userListView.winControl.itemDataSource = userList.dataSource;
}

// пополняется при клике по изображению like.png
function UserLike(e) {
	document.getElementById("like").style.display = "none";
}
```

[Источник](http://www.mindfiresolutions.com/How-to-catch-the-click-event-of-the-button-inside-the-WinJs-ListView-2482.php)
