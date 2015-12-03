---
layout: post
title: 10 способов генерации случайных паролей из коммандной строки
date: 2014-12-24 04:23
tags:
- linux
- памятка
- команды
- перевод
- bash
---

### Способ 1

Этот метод использует SHA для получения хэша от даты в формате Unix, проходит через base64, а затем выводит первые 32 символа:

```
date +%s | sha256sum | base64 | head -c 32 ; echo
```

### Способ 2

Этот метод использует встроенную в /dev/urandom функцию и отфильтровывает только те символы, которые вы обычно используете в пароле:

```
< /dev/urandom tr -dc _A-Z-a-z-0-9 | head -c${1:-32};echo;
```

### Способ 3

Этот метод использует функцию рандома OpenSSL, которая может быть не установлен на вашей системе:

```
openssl rand -base64 32
```

### Способ 4

Как и метод ранее, но в обратном порядке:

```
tr -cd '[:alnum:]' < /dev/urandom | fold -w30 | head -n1
```

### Способ 5

Этот способ с применением фильтра:

```
strings /dev/urandom | grep -o '[[:alnum:]]' | head -n 30 | tr -d '\n'; echo
```

### Способ 6

Простой вариант с использованием `urandom`:

```
< /dev/urandom tr -dc _A-Z-a-z-0-9 | head -c6
```

### Способ 7

Метод с использованием команды `dd`:

```
dd if=/dev/urandom bs=1 count=32 2>/dev/null | base64 -w 0 | rev | cut -b 2- | rev
```

### Способ 8

Метод, позволяющий ввести пароль с одной стороны и получить другой на выходе:

```
</dev/urandom tr -dc '12345!@#$%qwertQWERTasdfgASDFGzxcvbZXCVB' | head -c8; echo ""
```

### Способ 9

Если вы собираетесь часто использовать генерацию паролей, вероятнее всего лучшим решением для вас, будет положить команду в функцию. В таком случае, как только вы выполните команду один раз, вы сможете получить сгенерированный пароль. Для этого добавьте в файл `~/.bashrc` следующее:

```
randpw(){
	< /dev/urandom tr -dc _A-Z-a-z-0-9 | head -c${1:-16};echo;
}
```

### Способ 10

Самый простой способ, работающий в Linux, Windows с Cygwin, Mac OS X:

```
date | md5sum
```
Конечно, можно сомневаться в его достаточной случайности, сравнивая с вариантами выше, но все же :)

### Источники

* [10 Ways to Generate a Random Password from the Command Line](http://www.howtogeek.com/howto/30184/10-ways-to-generate-a-random-password-from-the-command-line/)