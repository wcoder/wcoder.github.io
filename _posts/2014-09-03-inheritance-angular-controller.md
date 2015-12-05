---
layout: post
title: Наследование в контроллере Angular
date: 2014-09-03 02:30
tags:
- javascript
- angularjs
- сниппет
---

```html
{% raw %}<div ng-controller="BMWController">
    My name is {{ name }} and I am a {{ type }}
    <button ng-click="clickme()">Click Me</button> 
</div>{% endraw %}
```

```js
function CarController($scope) {

    $scope.name = 'Car';
    $scope.type = 'Car';
    
    $scope.clickme = function() {
        alert('This is parent controller "CarController" calling');
    }
}
  
function BMWController($scope, $injector) {

    $injector.invoke(CarController, this, {$scope: $scope});
    
    $scope.name = 'BMW';  
}

```

























