---
layout: post
title: php中关于self和static的区别
category: 技术
tags: [PHP]
keywords: php,self,static,区别
description: php中关于self和static的区别，使用上的不同在哪里
---

> 话说在php中使用self和static的区别是什么？这是每一个phper都会遇到的一个问题，千言万语的解释它们的区别也抵不上一小段代码，废话不说，上代码。

## 代码

### 基类 Animal

```php
<?php
namespace demo;

//基类
class Animal
{
    public function __construct()
    {
    }

    public function selfFactory()
    {
        return new self();  #这里使用的self
    }

    public function staticFactory()
    {
        return new static();  #这里使用的static
    }

}
?>
```

### 子类 Dog

```php
<?php
namespace demo;

//子类继承基类
class Dog extends Animal
{

}
?>
```

### 实例

```php
<?php
namespace demo;
require_once __DIR__ . '/Animal.php';
require_once __DIR__ . '/Dog.php';
$dog = new Dog(); #实例化子类
var_dump($dog->selfFactory()); #调用self方法
//输出 object(demo\Animal)[2] Animal基类对象
var_dump($dog->staticFactory()); #调用static方法
//输出 object(demo\Dog)[2] Dog子类对象
?>
```

### 结论

结论是什么？已经很明显了。当实例化一个子类的时候，self代表的是基类，static代表的是当前实例化的类。

## 参考

[后期静态绑定](http://php.net/manual/zh/language.oop5.late-static-bindings.php)


