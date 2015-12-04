---
layout: post
title: Настройка собственных страниц ошибок в Nginx
date: 2014-07-20 02:08
tags:
- сниппет
- памятка
- настройка
- nginx
---

Конфигурация для блока `server` будет следующей:

```
error_page 404 /error/404.html;
error_page 500 501 502 503 504 /error/5xx.html;

location ^~ /error/ {
	internal;
	root /var/www/default;
}
```

При этом, необходимо наличие файлов: `404.html` и `5xx.html` в директории `/var/www/default/error/`

#### Ссылки на документацию:

* [location](http://nginx.org/en/docs/http/ngx_http_core_module.html#location)
* [error_page](http://nginx.org/en/docs/http/ngx_http_core_module.html#error_page)
* [root](http://nginx.org/en/docs/http/ngx_http_core_module.html#root)
