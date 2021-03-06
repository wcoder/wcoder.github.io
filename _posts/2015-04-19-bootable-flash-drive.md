---
layout: post
title: Делаем загрузочную флешку
date: 2015-04-19 22:43
tags:
- памятка
- программы
---

Записывать будем с помощью программы **WinSetupFromUSB**. Программа бесплатная, предназначена для создания загрузочной или мультизагрузочной флешки.

Скачать WinSetupFromUSB: [https://www.winsetupfromusb.com/downloads/](http://www.winsetupfromusb.com/downloads/)

### Установка

Программа не требует установки, все что нужно — распаковать архив и запустить нужную версию — 32-разрядную или же x64.

![распакованный архив](https://habrastorage.org/files/495/5d8/bae/4955d8bae60948c1bfef45530053cda0.png)

### Делаем загрузочную флешку

Несмотря на то, что создание загрузочной флешки — это не все, что можно делать с использованием данной утилиты (которая на самом деле включает в себя еще, как минимум 3 дополнительных инструмента для работы с USB накопителями), данная задача все-таки является основной.

В главном окне программы в верхнем поле выберите тот USB накопитель, на который будет производиться запись. Учтите, что все данные на нем будут удалены. Также отметьте галочкой пункт AutoFormat it with FBinst — это автоматически отформатирует флешку и приготовит ее к превращению в загрузочную, когда вы начнете.

![настройка записи](https://habrastorage.org/files/dc8/966/552/dc89665528e945c5a4a822bfffe5e164.png)

Следующий шаг — указать, что именно мы хотим добавить на флешку. Это может быть сразу несколько дистрибутивов, в результате чего мы получим мультизагрузочную флешку. Итак, отмечаем галочкой нужный пункт или несколько и указываем путь к нужным для работы WinSetupFromUSB файлам (для этого нажимаем кнопку с многоточием справа от поля):

* **Windows 2000/XP/2003 Setup** — используем для того, чтобы разместить дистрибутив одной из указанных операционных систем на флешке. В качестве пути требуется указать папку, в которой находятся папки I386/AMD64 (или только I386). То есть вам нужно либо смонтировать ISO образ с ОС в системе и указать путь к виртуальному приводу дисков, либо вставить диск с Windows и, соответственно, указать путь к нему. Еще один вариант — открыть образ ISO с помощью архиватора и извлечь все содержимое в отдельную папку: в этом случае в WinSetupFromUSB нужно будет указать путь к этой папке. Т.е. обычно, при создании загрузочной флешки Windows XP, нам просто требуется указать букву диска с дистрибутивом.
* **Windows Vista/7/8/Server 2008/2012** — для установки указанных операционных систем нужно указать путь к файлу образа ISO с нею. Вообще, в предыдущих версиях программы это выглядело иначе, но сейчас сделали проще.
* **UBCD4Win/WinBuilder/Windows FLPC/Bart PE** — также, как и в первом случае, потребуется путь к папке, в которой содержится I386, предназначено для различных загрузочных дисков на основе WinPE. Начинающему пользователю навряд ли потребуется.
* **LinuxISO/Other Grub4dos compatible ISO** — потребуется, если вы хотите добавить дистрибутив Ubuntu Linux (или другой Linux) или же какой-либо диск с утилитами для восстановления компьютера, проверки на вирусы и аналогичный, например: Kaspersky Rescue Disk, Hiren’s Boot CD, RBCD и другие. На большинстве из них используется именно Grub4dos.
* **SysLinux bootsector** — предназначен для добавления дистрибутивов Linux, в которых используется загрузчик syslinux. Скорее всего, не пригодится. Для использования требуется указать путь к папке, в которой находится папка SYSLINUX.

![процесс записи](https://habrastorage.org/files/206/309/562/206309562b354230a4291fe122224606.png)

После того, как все необходимые дистрибутивы были добавлены, просто нажимаем кнопку Go, утвердительно отвечаем на два предупреждения и начинаем ждать. Если вы делаете загрузочный USB накопитель, на котором присутствует Windows 7 или Windows 8, при копировании файла windows.wim может показаться, что WinSetupFromUSB завис. Это не так, наберитесь терпения и ожидайте.
