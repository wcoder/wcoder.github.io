---
layout: post
title: Настройка часового пояса в Laravel 4
redirect_to: "https://ypakala.com"
date: 2014-04-06 19:45
tags:
- laravel
- php
---

Для настройки часового пояса в вашем Laravel приложении, в конфигурационном файле <mark>config/app.php</mark> указано следующее:

``` php
/*
|--------------------------------------------------------------------------
| Application Timezone
|--------------------------------------------------------------------------
|
| Here you may specify the default timezone for your application, which
| will be used by the PHP date and date-time functions. We have gone
| ahead and set this to a sensible default for you out of the box.
|
*/

'timezone' => 'UTC',
```

`UTC` можно заменить на любой из [поддерживаемых в PHP часовых поясов](http://php.net/manual/en/timezones.php).

Например для Беларуси будет:

```php
'timezone' => 'Europe/Minsk',
```
