---
layout: post
title: Как удалить все фотографии со страницы Вконтакте?
date: 2015-03-05 05:11
tags:
- javascript
- vk
---

 Если у вас возникла необходимость удаления всех фотографий с вашей страницы, выполните эти простые шаги:

- Переходим на страницу вашего профиля
- Кликаем на фотку профиля
- Нажимаем `F12` - открываем панель разработчика (если вы используете Google Chrome)
- Выбираем вкладку `Console/Консоль`
- Вставляем туда следующий код:

```js
setInterval('a=0;b=1;while(a!=b){Photoview.deletePhoto();a=cur.pvIndex;Photoview.show(false,cur.pvIndex+1,null);b=cur.pvIndex;}',3000);
```

- Нажимаем `Enter`
- Profit!

> Иногда во Вконтакте срабатывает защита на удаление фотографий в этом случае подождите примерно 1-2 часа и продолжайте удаление.