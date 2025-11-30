---
layout: post
title: Laravel - как проверить, что закачиваемый файл изображение?
redirect_to: "https://ypakala.com"
date: 2014-06-07 11:53
tags:
- laravel
- php
---

Как проверить, что закачиваемый файл изображение? Пару быстрых  фрагментов ниже подробно опишут, как выполнить эту задачу.

Создаем представление `form.blade.php` со следующим содержанием:

```html
<!doctype html>
<html lang="en">
<head>
	<title>File Upload</title>
</head>
<body>
{% raw %}
@if (Session::has("message"))
	{{ Session::get("message") }}
@endif

<hr />

{{ Form::open(array('url' => '/', 'files' => true)) }}
{{ Form::file('image'); }}
{{ Form::submit('Upload File'); }}
{% endraw %}
</body>
</html>
```

Приведенный ниже код необходимо поместить в контроллер, или для быстроты просто бросить его в пути.

Вы можете использовать следующую логику с [Laravel валидатором](http://laravel.com/docs/validation) для проверки файла:

```php
Route::get('/', function()
{
	return View::make("form");
});

Route::post('/', function()
{
	$input = array('image' => Input::file('image'));
	
	$rules = array(
		'image' => 'image'
	);
	
	$validator = Validator::make($input, $rules);
	
	if ($validator->fails()) {
		return Redirect::to('/')->with('message', 'Error: The provided file was not an image');
	} else {
		return Redirect::to('/')->with('message', 'Success: File upload was successful');
	}
});
```
