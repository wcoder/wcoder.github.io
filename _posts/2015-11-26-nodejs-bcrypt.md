---
layout: post
title: Приступая к работе с Node.js и bcrypt
date: 2015-11-26 18:28
tags: nodejs, bcrypt, security
---

Для тех из вас, кто ищет безопасный способ хранения паролей пользователей в вашем Node.js приложении, смотрите далее!

Представляем [bcrypt](https://www.npmjs.com/package/bcrypt-nodejs).

Этот npm пакет использует [UNIX библиотеку bcrypt](https://en.wikipedia.org/wiki/Bcrypt), написанную в 1999 году. Это позволяет хэшировать и шифровать конфиденциальные данные, такие как пароли пользователей, перед их сохранением в базу данных.

Давайте проверим на примере!

Во-первых, мы установим bcrypt и сохраним его в текущем проекте:

```
npm install bcrypt --save
```

Затем, внутри вашего приложения, создадим соль и воспользуемся функцией `hashSync` чтобы превратить обычный текст в зашифрованный хэш пароля.

``` js
// index.js
 
// подключаем bcrypt
var bcrypt = require('bcrypt');

// пароль пользователя
var passwordFromUser = "test_user_pass";
 
// создаем соль
var salt = bcrypt.genSaltSync(10);
 
// шифруем пароль
var passwordToSave = bcrypt.hashSync(passwordFromUser, salt)

// выводим результат
console.log(salt);
console.log(passwordFromUser);
console.log(passwordToSave);
```

Запускаем:

```
node index.js
```

И последнее, всякий раз, когда вам нужно вытащить пароль из базы данных и проверить его для пользователя (например, когда он пытается снова войти в систему) просто сделать что-то вроде этого:

``` js
// запрос пользователя из вашей базы данных - в данном примере используется MySQL
connection.query("SELECT * FROM users WHERE username = ?",
    [usernameEnteredByUser],
    function(err, rows) {
        if (err) {
            return done(err);
        }
 
        if (bcrypt.hashSync(passwordEnteredByUser, salt) === rows[0].password) {
          // да, это работает
        }
});
```

На этом все.

Источник: [Getting started with Node.js and bcrypt](http://codeplanet.io/getting-started-with-node-js-and-bcrypt/)
