---
layout: post
title: Решаем возникающую ошибку «Synthetic SCSI Controller Error» при загрузке ISO образа
redirect_to: "https://ypakala.com"
date: 2015-07-15 01:40
tags:
- hyper-v
- виртуализация
- решения
---

Если при создании виртуальной машины в Hyper-V Manager, при добавлении ISO образа операционной системы у вас возникло следующее сообщение:

![Hyper-V: Synthetic SCSI Controller Error with boot ISO](https://raw.githubusercontent.com/wcoder/blog/master/hyper-v_iso/error.png)

### Решение

Во-первых, у меня есть вопрос. Почему это решение? Что в этом особенного, что заставляет его работать!? Это файловый поток или что-то другое?

Хорошо - решение. Вы готовы? Серьезно?

**Сделать копию ISO и смонтировать его в виртуальный DVD-привод снова.**

![facepalm](https://raw.githubusercontent.com/wcoder/blog/master/hyper-v_iso/cat-facepalm.gif)
