---
layout: post
title: Модальное окно на чистом CSS
date: 2015-11-10 16:59
tags:
- css
---

## HTML

Создадим каркас для нашего модального окна:

```html
<a href="#open-modal">Open modal</a>

<div class="modal" id="open-modal">
    <div class="modal-container">
        <p>text text text</p>
        <a href="#modal-close">Close</a>
    </div>
</div>
```

## CSS

Далее немного стилизуем:

```css
p {
    margin-top: 0px;
}

.modal-container {
    position: fixed;
    background-color: #fff;
    border: 1px solid #000;
    width: 70%;
    max-width: 400px;
    left: 50%;
    padding: 20px;
    border-radius: 5px;
    
    -webkit-transform: translate(-50%, -200%);
    -ms-transform: translate(-50%, -200%);
    transform: translate(-50%, -200%);
}

.modal:before {
    content: "";
    position: fixed;
    display: none;
    background-color: rgba(0,0,0,.8);
    top: 0;
    left: 0;
    height: 100%;
    width: 100% 
}
```

Чтобы показать модальное окно мы воспользуемся псевдоклассом [:target](http://htmlbook.ru/css/target), появившемся в CSS3.

Добавим для этого следующих стилей:

```css
.modal:target:before {
    display: block;
}

.modal:target .modal-container {
    top: 20%;
    
    -webkit-transform: translate(-50%, 0);
    -ms-transform: translate(-50%, 0);
    transform: translate(-50%, 0);
}
```

Добавим анимации:

```css
.modal-container {

    ...
    
    -webkit-transition: -webkit-transform 200ms ease-out;
    transition: transform 200ms ease-out;
}
```

## Результат

[Смотреть результат](http://jsfiddle.net/evgeniypakalo/s38z79q5/embedded/result/)
