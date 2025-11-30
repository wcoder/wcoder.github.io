---
layout: post
title: Ёлка на чистом CSS
redirect_to: "https://ypakala.com"
date: 2015-01-03 02:32
tags:
- css
---

### HTML

Начнем с каркаса:

```html
<div class="tree">
    <div class="tree-layer"></div>
    <div class="tree-layer"></div>
    <div class="tree-layer"></div>
    <div class="tree-log"></div>
</div>
```

### CSS

Стилизуем первый треугольник:

```css
.tree {
    width: 120px;
}

.tree-layer,
.tree-log {
    margin: 0 auto;
}

.tree-layer {
    width: 0;
    height: 0;
    border-left: 40px solid transparent;
    border-right: 40px solid transparent;
    border-bottom: 60px solid green;
}
```

Далее по аналогии:

```css
.tree-layer:nth-child(2) {
    margin-top: -30px;
    border-left-width: 50px;
    border-right-width: 50px;
    border-bottom-width: 80px;
}

.tree-layer:nth-child(3) {
    margin-top: -50px;
    border-left-width: 60px;
    border-right-width: 60px;
    border-bottom-width: 100px;
}
```

Остался лишь ствол дерева, сделаем:

```css
.tree-log {
    width: 20px;
    height: 30px;
    background-color: saddlebrown;
}
```

Готово!

### Результат

<iframe width="100%" height="300" src="//jsfiddle.net/evgeniypakalo/3ece02pv/embedded/result/" allowfullscreen="allowfullscreen" frameborder="0"> </iframe>
