---
layout: post
title: Памятка по настройке LAMP в Ubuntu для локальной разработки
date: 2014-03-22 00:59
tags:
- lamp
- ubuntu
- apache
- сервер
- памятка
---

## Установка пакетов

```
sudo apt-get install apache2 php5 php5-mysql php5-xdebug php5-curl mysql-server phpmyadmin
```

## Конфигурация виртуального хоста

В данном примере, настройки производятся для домена `examples.loc`

```
sudo vim /etc/apache2/sites-available/examples
```

```
<VirtualHost *:80>
	ServerAdmin webmaster@localhost
	
	ServerName examples.loc
	DocumentRoot /home/user/www/examples.loc
	<Directory /home/user/www/examples.loc>
		#Options Indexes FollowSymLinks MultiViews
		AllowOverride All
		Order allow,deny
		allow from all
	</Directory>
	
	ErrorLog ${APACHE_LOG_DIR}/error-examples.log
	
	# Possible values include: debug, info, notice, warn, error, crit,
	# alert, emerg.
	LogLevel warn
	
	CustomLog ${APACHE_LOG_DIR}/access-examples.log combined
	
</VirtualHost>
```

### Включение хоста

```
sudo a2ensite examples
```

```
sudo service apache2 restart
```

```
sudo echo "127.0.0.1    examples.loc" >> /etc/hosts
```

## Конфигурация PHP

```
sudo vim /etc/php5/apache2/php.ini
```

```
display_errors = On
display_startup_errors = On
html_errors = On
date.timezone = "Europe/Minsk"
```

## Включение модулей Apache

```
sudo a2enmod rewrite
```

### Полезное

* [Как установить и настроить NGINX+PHP+XDebug на Ubuntu 12.04](http://webdevnotice.blogspot.ru/2012/08/nginxphpxdebug-ubuntu-1204.html)
* [XDebug отладка PHP скриптов в Ubuntu](http://ubuntu-favorite-os.blogspot.com/2012/12/xdebug.html)
