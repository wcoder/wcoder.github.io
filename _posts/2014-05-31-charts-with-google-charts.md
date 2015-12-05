---
layout: post
title: Диаграммы с помощью Google Charts
date: 2014-05-31 19:46
tags:
- javascript
- api
---

Google диаграммы обеспечивают идеальный способ визуализации данных на вашем веб-сайте. 

Предоставляет большое количество готовых к использованию типов диаграмм, [примеры в галлерее](https://google-developers.appspot.com/chart/interactive/docs/gallery).

### Пример круговой диаграммы

```html
<!doctype html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Google Charts</title>
	
	<!-- Загрузка AJAX API -->
	<script type="text/javascript" src="https://www.google.com/jsapi"></script>
	<script type="text/javascript">
	
		// Загрузить API визуализации.
		google.load('visualization', '1.0', {'packages': ['corechart'] });
		
		// Установка функции обратного вызова для запуска отрисовки,
		// по окончанию загрузки API визуализации.
		google.setOnLoadCallback(drawChart);
		
		// Функция, которая создает круговую диаграмму,
		// заполняет таблицу данными и отрисовывает их.
		function drawChart() {
		
			// Создание таблицы данных.
			var data = new google.visualization.DataTable();
			data.addColumn('string', 'Topping');
			data.addColumn('number', 'Slices');
			data.addRows([
				['Mushrooms', 3],
				['Onions', 10],
				['Olives', 1],
				['Zucchini', 1],
				['Pepperoni', 2]
			]);
			
			// Установка опций для диаграммы.
			var options = {
				'title': 'How Much Pizza I Ate Last Night',
				'width': 500,
				'height': 500,
				'is3D': true
			};
			
			// Инициирование и отрисовка диаграммы, передавая некоторые параметры.
			var chart = new google.visualization.PieChart(document.getElementById('chart_div'));
			chart.draw(data, options);
		}
	</script>
</head>
<body>

	<!-- div, который будет содержать круговую диаграмму -->
	<div id="chart_div"></div>
	
</body>
</html>
```

### Ссылки

* [Быстрый старт](https://google-developers.appspot.com/chart/interactive/docs/quick_start)
* [PNG графики](https://google-developers.appspot.com/chart/interactive/docs/printing)
