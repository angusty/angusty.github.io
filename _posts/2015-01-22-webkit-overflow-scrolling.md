---
layout: post
title: 解决手机访问页面滚动卡的问题
category: 技术
tags: [CSS]
keywords: css,让你页面快速滚动的css,手机滚动卡
description: 只需要添加一句css就可以让你的网页快速滚动,解决你手机滚动卡的问题
---

> 最近发现当用手机浏览器访问自己的博客的时候会遇到卡顿的问题，特别是当点击一篇稍微长点的博文，比如[设计模式详解及PHP实现](/2015/01/11/design-patterns-of-php.html)，会明显的感觉滑动不流畅，而且会在滑动到某个地方的时候卡住，无法往下滑动。这个问题困扰了我几天，开始以为是js的影响，尝试去掉一些可能影响的js，问题仍然得不到解决。直到今天。。。终于搞定了

### 解决手机滑动卡的问题
如何解决的呢，其实很简单，只加了一句[css](https://github.com/angusty/angusty.github.io/blob/master/public/css/base.css#L8)

```css
-webkit-overflow-scrolling: touch;
```
是不是很神奇啊:smile:
这是由于你在对html中的某个模块使用了overflow scroll属性，这个属性会使你在手机上滑动这个模块的时候产生卡顿。这行代码只对手机端浏览器有效，它启用了硬件加速，所以让滑动变得流畅，不过会消耗更多的内存。
