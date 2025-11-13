---
layout: post
title: Включаем gzip-сжатие с использование .htaccess
redirect_to: "https://ypakala.com"
date: 2015-12-16 21:50
original_url: http://www.xpertdeveloper.com/2012/04/htaccess-gzip-compression/
tags:
- apache
- .htaccess
- перевод
---

Gzip&nbsp;&mdash; наилучшая практика чтобы сжать контент, тем самым сохранив пользовательский трафик. В&nbsp;этой статье я&nbsp;покажу как сжимать статический контент с&nbsp;использованием Apache и&nbsp;файла .htaccess.

Позвольте начать с&nbsp;небольшого введения. Мы&nbsp;можем сжимать содержимое двумя различными способами: **Gzip** и&nbsp;**deflate**.

## Введение

Метод Gzip использовался в&nbsp;ранних версиях Apache (до&nbsp;версии 1.3). Таким образом, на&nbsp;данный момент у&nbsp;вас должен быть Apache версии выше 1.3, если нет&nbsp;&mdash; обновите до&nbsp;последней версии.

Чтобы воспользоваться преимуществом этого средства сжатия, у&nbsp;вас должен быть установлен Apache с&nbsp;включенным модулем `mod_deflate`. Чтобы включить этот модуль вы&nbsp;просто должны разкомментировать эту строку в `http.conf`&nbsp;&mdash; конфигурационный файл Apache.

После включения этого модуля ваш сервер будет готов обеспечивать сжатие данных. Но, сервер будет делать это только тогда, когда он&nbsp;получит соответствующие заголовки от&nbsp;клиента. Таким образом, теперь, чтобы включить это вы&nbsp;должны поместить код ниже в&nbsp;файл `.htaccess` вашего сайта, чтобы сообщить серверу что должно быть сжато.

## код .htaccess

```
<IfModule mod_deflate.c>
  # force deflate for mangled headers
  # developer.yahoo.com/blogs/ydn/posts/2010/12/pushing-beyond-gzipping/
  <IfModule mod_setenvif.c>
    <IfModule mod_headers.c>
      SetEnvIfNoCase ^(Accept-EncodXng|X-cept-Encoding|X{15}|~{15}|-{15})$ ^((gzip|deflate)\s*,?\s*)+|[X~-]{4,13}$ HAVE_Accept-Encoding
      RequestHeader append Accept-Encoding "gzip,deflate" env=HAVE_Accept-Encoding
    </IfModule>
  </IfModule>

  # HTML, TXT, CSS, JavaScript, JSON, XML, HTC:
  <IfModule filter_module>
    FilterDeclare   COMPRESS
    FilterProvider  COMPRESS  DEFLATE resp=Content-Type $text/html
    FilterProvider  COMPRESS  DEFLATE resp=Content-Type $text/css
    FilterProvider  COMPRESS  DEFLATE resp=Content-Type $text/plain
    FilterProvider  COMPRESS  DEFLATE resp=Content-Type $text/xml
    FilterProvider  COMPRESS  DEFLATE resp=Content-Type $text/x-component
    FilterProvider  COMPRESS  DEFLATE resp=Content-Type $application/javascript
    FilterProvider  COMPRESS  DEFLATE resp=Content-Type $application/json
    FilterProvider  COMPRESS  DEFLATE resp=Content-Type $application/xml
    FilterProvider  COMPRESS  DEFLATE resp=Content-Type $application/xhtml+xml
    FilterProvider  COMPRESS  DEFLATE resp=Content-Type $application/rss+xml
    FilterProvider  COMPRESS  DEFLATE resp=Content-Type $application/atom+xml
    FilterProvider  COMPRESS  DEFLATE resp=Content-Type $application/vnd.ms-fontobject
    FilterProvider  COMPRESS  DEFLATE resp=Content-Type $image/svg+xml
    FilterProvider  COMPRESS  DEFLATE resp=Content-Type $application/x-font-ttf
    FilterProvider  COMPRESS  DEFLATE resp=Content-Type $font/opentype
    FilterChain     COMPRESS
    FilterProtocol  COMPRESS  DEFLATE change=yes;byteranges=no
  </IfModule>

  <IfModule !mod_filter.c>
    # Legacy versions of Apache
    AddOutputFilterByType DEFLATE text/html text/plain text/css application/json
    AddOutputFilterByType DEFLATE application/javascript
    AddOutputFilterByType DEFLATE text/xml application/xml text/x-component
    AddOutputFilterByType DEFLATE application/xhtml+xml application/rss+xml
    AddOutputFilterByType DEFLATE application/atom+xml
    AddOutputFilterByType DEFLATE image/svg+xml application/vnd.ms-fontobject
    AddOutputFilterByType DEFLATE application/x-font-ttf font/opentype
  </IfModule>
</IfModule>
```

После размещения кода выше в&nbsp;файл `.htaccess` обратите внимание на&nbsp;заголовок ответа вашего сайта. Вы&nbsp;увидите один дополнительный заголовок `Accept-Encoding`. Это означает что запрашивающий клиент получит сжатый контент.

```
Accept-Encoding:gzip,deflate,sdch
```

## Результат

Посмотрите на&nbsp;изображения ниже, чтобы увидеть какое сжатие мы&nbsp;получили. Так что в&nbsp;конечном итоге это улучшает производительность вашего сайта.

![Результат](https://cloud.githubusercontent.com/assets/766193/11850541/f6bc8f16-a43e-11e5-928e-73c0f2fc4b9c.jpg)

На&nbsp;изображении выше вы&nbsp;можете увидеть, что фактический размер страницы составляет 707Кб, но&nbsp;после сжатия этот размер составляет 401Кб.

На&nbsp;этом все.
