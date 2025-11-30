---
layout: post
title: DRY о знании
redirect_to: "https://ypakala.com"
date: 2015-11-07 19:41
original_url: http://verraes.net/2014/08/dry-is-about-knowledge/
tags:
- перевод
- ddd
- dry
---

Дублирование кода не проблема.

Взгляните на эти два класса:

``` php
<?php // пример 1
final class Basket
{
	private $products;

	public function addProduct($product)
	{
		if (3 == count($this->products)) {
			throw new Exception("Максимально разрешено 3 продукта");
		}
		$this->products[] = $product;
	}
}

final class Shipment
{
	private $products;

	public function addProduct($product)
	{
		if (3 == count($this->products)) {
			throw new Exception("Максимально разрешено 3 продукта");
		}
		$this->products[] = $product;
	}
}
```

Вы сказали бы, что это дублирование кода? Это нарушает DRY принцип ([Don’t Repeat Yourself](https://ru.wikipedia.org/wiki/Don%E2%80%99t_repeat_yourself))?

Если да, то решение избавится от него, могло бы быть что-то вроде этого:

``` php
<?php // пример 2
abstract class ProductContainer
{
	protected $products;

	public function addProduct($product)
	{
		if (3 == count($this->products)) {
			throw new Exception("Максимально разрешено 3 продукта");
		}
		$this->products[] = $product;
	}
}

final class Basket extends ProductContainer {}
final class Shipment extends ProductContainer {}
```

Код идентичен, но почему? Как хорошие DDD ([Domain-driven design](https://ru.wikipedia.org/wiki/%D0%9F%D1%80%D0%BE%D0%B1%D0%BB%D0%B5%D0%BC%D0%BD%D0%BE-%D0%BE%D1%80%D0%B8%D0%B5%D0%BD%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B5_%D0%BF%D1%80%D0%BE%D0%B5%D0%BA%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5)) практики, мы должны были бы свериться с бизнесом. Продуктом мог быть легковоспламеняющийся химикат, и для безопасности, отгрузка не разрешается если имеем более трех из них. Как следствие, мы не позволяем клиентам заказывать более трех одновременно.

В другом сценарии сходство кода также может быть простым совпадением. Может быть поставка продукции ограничена и мы хотим, дать нашим клиентам равные возможности для ее покупки.

![найдите отличия](http://verraes.net/img/posts/2014-08-02-dry-is-about-knowledge/find-the-differences-small.jpg)

## Причины для изменения

В любом случае, это не имеет значения. Проблема в том, что правила могут измениться самостоятельно. Предприятие может понять, что они все еще могут продать больше, чем три продукта за один раз и разделить продукты на несколько поставок. В примере 2, мы застреваем с высокой связонностью обоих типов доменных объектов к тому же бизнес-правилу, и косвенно друг с другом. Изменение правил для `Basket` изменяет правило для `Shipment`, но потенциально имеет опасные последствия в реальном мире. Пример, конечно, откровенно простой, чтобы осуществить рефакторинг, но устаревший код обычно намного сложнее.

Вы могли бы попытаться решить проблему путем абстрагирования предела:

``` php
<?php // пример 3
abstract class ProductContainer
{
	protected $products;

	public function addProduct($product)
	{
		if ($this->getProductLimit() == count($this->products)) {
			throw new Exception(sprintf("Максимально разрешено %d продукта", $this->getProductLimit()));
		}
		$this->products[] = $product;
	}

	abstract protected function getProductLimit();
}
```

Это работает в определенной степени, но правила могут измениться самым неожиданным образом. Пример 3 предполагает, что предел всегда легко определить с помощью одного вызова метода. Новые правила могли бы принять во внимание клиента, их историю с нашей компанией, определенными юридическими условиями, продвижениями, и т.д. Новые правила могут динамично влиять на число продуктов, которые Вы можете купить способами, которые не могут быть смоделированы, используя наш абстрактный метод `addProduct()`.

## Знание

Бизнес-правило в моем примере не "Максимально разрешено 3 продукта". Есть фактически два бизнес-правила: "Корзине не позволяют иметь больше чем три продукта", и "Отгрузке не позволяют иметь больше чем три продукта". Два правила, независимо от того, насколько похожи, должны иметь два отдельных представления в модели.

"Don’t Repeat Yourself" никогда не был о коде. Это о знании. Это о сцеплении. Если две части кода будут представлять то же самое знание, то они будут всегда изменяться вместе. Необходимость изменения их обоих опасна: вы могли бы забыть одного из них. С другой стороны, если два одинаковых фрагмента кода представляют разные знания, они будут меняться независимо. Недублирование их представляет риск, потому что, изменяя знание для одного объекта, мог бы случайно изменить его для другого объекта.

При рассмотрении причин изменения, очень мощным является эвристическое моделирование.

## Далее

Последующие посты:

- [Why DRY?](http://blog.ploeh.dk/2014/08/07/why-dry/) by Mark Seemann ([@ploeh](https://twitter.com/ploeh))
- [Aim for DRY, but be willing to fall short](http://www.devjoy.com/2014/09/aim-for-dry-but-be-willing-to-fall-short/) by Richard Dalton ([@richardadalton](https://twitter.com/richardadalton))

Некоторые посты о DDD:

- [Domain-Driven Design is linguistic](http://verraes.net/2014/01/domain-driven-design-is-linguistic/)
- [Why Domain-Driven Design Matters](http://verraes.net/2014/05/why-domain-driven-design-matters/)
