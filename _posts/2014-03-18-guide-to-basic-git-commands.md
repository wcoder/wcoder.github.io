---
layout: post
title: Памятка по основным командам Git
date: 2014-03-18 01:17
tags:
- памятка
- git
---

![Git logo](https://raw.githubusercontent.com/wcoder/blog/master/git/Git-Logo.png)

## Команды

`git status` — пересматриваем изменения

`git add . ` — добавляем изменения

`git commit -a -m 'message'` — подтверждение изменений в текущей ветке

`git pull` — скачать новые изменения

`git push username-project current-branch:remote-branch` — запись текущей ветки в удаленный репозиторий

`git reset --hard 'hash code'` — возвращаемся на версию по хеш коду

`git branch -a` — отобразить все ветки

`git branch -D local-branch` — удалить локальную ветку

`git checkout local-branch` — переключится на локальную ветку

`git merge local-branch` — наложить изменения из локальной ветки в текущую

`git checkout -b local-branch remotes/origin/master` — скачать ветку с удаленного репозитория и переключиться на нее

`git remote add username-project git@github.com:username/project.git` — добавление ссылки на удаленный репозиторий

`git remote update` — обновить информацию о удаленном репозитории

`git reset HEAD~10` — откатить форк на нужное количество коммитов

Cоздание ключа для работы с github:

```
cd ~/.ssh
ssh-keygen -t rsa -C "username@gmail.com" // генерация ключа
cat ~/.ssh/id_rsa.pub // вывод ключа
```

Теперь этот ключ надо прописать в публичные ключи в ветке на github.com

#### Первоначальные настройки

```
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
```
