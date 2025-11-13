---
layout: post
title: Команды для консольного управления MySQL
redirect_to: "https://ypakala.com"
date: 2014-07-12 00:25
tags:
- памятка
- mysql
- базы данных
---

#### Создаем новую БД ([wiki](http://dev.mysql.com/doc/refman/5.5/en/create-database.html))

```sql
CREATE DATABASE  db_name CHARACTER SET utf8 COLLATE utf8_general_ci;
```

#### Добавляем пользователя к созданной БД ([wiki](http://dev.mysql.com/doc/refman/5.5/en/adding-users.html))

```sql
CREATE USER 'db_user'@'localhost' IDENTIFIED BY 'user_password';
GRANT ALL PRIVILEGES ON db_name.* TO 'db_user'@'localhost';
```

#### Импортирование БД

```bash
mysql -u [db_user] -p [db_name] < my_dump.sql
```

#### Экспортирование БД

```bash
mysqldump -u [db_user] -p [db_name] > my_dump.sql 
```
