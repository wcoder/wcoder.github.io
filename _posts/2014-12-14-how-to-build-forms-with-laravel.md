---
layout: post
title: Как создавать формы в Laravel
date: 2014-12-14 03:17
published: false
tags:
- памятка
- laravel
- php
- сниппет
- перевод
---

На самом деле, Larvel сожержит кучу удобных методов для простого создания форм. Мы расмотрим эти методы.

Давайте создадим форму, просто создаем новый файл `app/views/form.blade.php` и пишем туда следующее:

```html
<!--app/views/form.blade.php-->
<!doctype html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Laravel</title>
</head>
<body>
	{{ Form::open(array('url'=>'form-submit')) }}
	
	{{ Form::close() }}
</body>
</html>
```

Результат отображения будет слудующим:

```html
<!--app/views/form.blade.php-->
<!doctype html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Laravel</title>
</head>
<body>
	<form method="POST" action="http://localhost/<laravel dir>/public/form-submit" accept-charset="UTF-8">
		<input name="_token" type="hidden" value="h7xNdTaJXwLz5v0lkBolVPelpxldoiDR5gcKWkku">  
	</form>
</body>
</html>
```

Возможно, вам не нравятся значения формы по умолчанию, поэтому давайте посмотрим как их можно изменить:

```
{{ Form::open(array(
	'url'=>'form-submit',
	<!--POST or GET or DELETE-->
	'method'=>'POST',
	'accept-charset'=>'UTF-8',
	<!--IF form contain file upload input-->
	'files'=>true
)) }}
```

А теперь расмотрим подробнее поля формы.

### Текстовое поле

```
{{ Form::label('username','Username',array('id'=>'','class'=>'')) }}
{{ Form::text('username','clivern',array('id'=>'','class'=>'')) }}
```

### Поле ввода текста

```
{{ Form::label('biog','Biog.',array('id'=>'','class'=>'')) }}
{{ Form::textarea('biog','biog here',array('id'=>'','class'=>'')) }}
```

### Поле ввода пароля

```
{{ Form::label('password','Password',array('id'=>'','class'=>'')) }}
{{ Form::password('password','',array('id'=>'','class'=>'')) }}
```

### Поле ввода email'а

```
{{ Form::label('email','Email',array('id'=>'','class'=>'')) }}
{{ Form::email('email','hello@clivern.com',array('id'=>'','class'=>'')) }}
```

### Список

```
{{ Form::label('status','Status',array('id'=>'','class'=>'')) }}
{{ Form::select('status',array('enabled'=>'Enabled','disabled'=>'Disabled'),'enabled') }}
```

### Переключатель

```
{{ Form::label('status','Status',array('id'=>'','class'=>'')) }}
{{ Form::radio('status','enabled',true) }} Enabled
{{ Form::radio('status','disabled') }} Disabled
```

### Чекбокс

```
{{ Form::label('status','Status',array('id'=>'','class'=>'')) }}
{{ Form::checkbox('status','1',true) }} Enabled
```

### Скрытое поле

```
{{ Form::hidden('record_to_update','1') }}
```

### Кнопки

```
<!-- submit buttons -->
{{ Form::submit('Save') }}

<!-- reset buttons -->
{{ Form::reset('Reset') }}

<!-- normal buttons -->
{{ Form::button('Normal') }}
```

Оригинал: [How To Build Forms With Laravel](http://wz2.ru/lfp8)
