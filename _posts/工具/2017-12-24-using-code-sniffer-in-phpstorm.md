---
layout: post
title: 在phpstorm上安装代码检查工具
category: 工具
tags: [PHP, phpstorm]
keywords: phpstorm,安装,工具
description: 在phpstorm上安装代码检查工具,基于psr2规范的检查
---

> 我们写代码要遵循一定的规范, 但是我们有时候会犯错，所以需要有一个检查机制。当我们犯错的时候检查机制会发挥作用。下面我介绍一下如何在phpstrom上设置代码检查。

## 代码规范

php有很多规范，我遵循的是PSR规范。PSR是PHP通用性框架小组[FIG](http://www.php-fig.org/)（PHP Framework Interop Group） 制定的PHP规范，是PHP开发的事实标准。关于PSR的中文文档，可以参考[https://github.com/PizzaLiu/PHP-FIG](https://github.com/PizzaLiu/PHP-FIG)

## 让phpstrom支持基于PSR2的代码检查的步骤

> 环境： windows操作系统 phpstrom版本10.0.2

### 安装phpcs

#### 使用[composer](http://www.phpcomposer.com/)全局安装

```

composer global require "squizlabs/php_codesniffer=*"

```

> 注：windows系统,会在```C:\Users\{user name}\AppData\Roaming\Composer\vendor\bin```下生成一个phpcs.bat文件,这个是phpstorm后续设置需要用到的文件

### phpstorm设置


- 步骤1：打开phpstorm点击 File->Settings

    ![image](/public/img/2015-12-24/phpstorm/1.png)

---

- 步骤2：接着点击Languages & Frameworks->PHP->Code Sniffer点击Configuration右侧的按钮，

    ![image](/public/img/2015-12-24/phpstorm/2.png)  

---

- 步骤3：选择PHP Code Sniffer (phpcs) path:的路径，就是刚才composer之后生成的那个phpcs.bat的路径。

    ![image](/public/img/2015-12-24/phpstorm/3.png)  

---

- 步骤4：选择之后点击Validate验证成功

    ![image](/public/img/2015-12-24/phpstorm/4.png)  

---

- 步骤5：节点点击Editor->Inspections展开点击右侧的PHP

    ![image](/public/img/2015-12-24/phpstorm/5.png)

---

- 步骤6：勾选PHP Code Sniffer Validation 选择右侧的PSR2

    ![image](/public/img/2015-12-24/phpstorm/6.png)

---

- 步骤7：点击验证成功 大功告成！！

    ![image](/public/img/2015-12-24/phpstorm/7.png)

---

- 看看效果吧，当写的代码不符合PSR2规范的时候该行代码下会有波浪线,点击波浪线可以查看提示信息

     ![image](/public/img/2015-12-24/phpstorm/8.png)

 ---

 > 以上是phpstorm配置代码检查工具的通用步骤，我是基于windows的环境用的phpstorm10.0.2的版本,不同的版本可能设置上会有差异。在linux/mac环境下的步骤是一样的，区别就在步骤3中选择phpcs文件的路径不同，还有就是windows下是用的phpcs.bat文件，linux/mac下是phpcs文件


