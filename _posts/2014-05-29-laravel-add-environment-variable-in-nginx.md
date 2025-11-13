---
layout: post
title: Добавляем переменную окружения Laravel в nginx
redirect_to: "https://ypakala.com"
date: 2014-05-29 21:20
tags:
- памятка
- nginx
- laravel
---

Открываем конфиг:

```bash
sudo nano /etc/nginx/sites-available/local.dev
```

Находим блок <mark> location ~ \.php$` </mark> и обновляем конфигурации:

```
location ~ \.php$ {
	fastcgi_split_path_info ^(.+\.php)(/.+)$;

	fastcgi_param LARAVEL_ENVIROMENT "dev";

	fastcgi_pass unix:/var/run/php5-fpm.sock;
	fastcgi_index index.php;
	include fastcgi_params;
}
```

Перезагружаем nginx:

```bash
sudo /etc/init.d/nginx restart
```

Далее, в файле <mark> bootstrap/start.php </mark> редактируем метод определения окружения:

```php
$env = $app->detectEnvironment(function(){
	return getenv("LARAVEL_ENVIROMENT");
});
```
