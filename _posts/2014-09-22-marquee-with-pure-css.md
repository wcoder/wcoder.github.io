---
layout: post
title: Бегущая строка на чистом CSS
date: 2014-09-22 08:10
tags:
- css
- анимация
---

А ведь раньше ее можно было задать на чистом html (тегом  `<marquee>`) :)

Теперь так:

### HTML

```html
<h1 class="marquee"><span>This is a marquee. Text, text, text...</span></h1>
```

### CSS

```css
@-webkit-keyframes scroll {
    0% {
        -webkit-transform: translate(0, 0);
        transform: translate(0, 0);
    }
    100% {
        -webkit-transform: translate(-100%, 0);
        transform: translate(-100%, 0)
    }
}

@-moz-keyframes scroll {
    0% {
        -moz-transform: translate(0, 0);
        transform: translate(0, 0);
    }
    100% {
        -moz-transform: translate(-100%, 0);
        transform: translate(-100%, 0)
    }
}

@keyframes scroll {
    0% {
        transform: translate(0, 0);
    }
    100% {
        transform: translate(-100%, 0)
    }
}

.marquee {
    display: block;
    width: 100%;
    white-space: nowrap;
    overflow: hidden;
}

.marquee span {
    display: inline-block;
    padding-left: 100%;
    -webkit-animation: scroll 5s infinite linear;
    -moz-animation: scroll 5s infinite linear;
    animation: scroll 5s infinite linear;
}
```

### Демо

<iframe width="100%" height="300" src="//jsfiddle.net/evgeniypakalo/6hcwkzvw/embedded/result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>



