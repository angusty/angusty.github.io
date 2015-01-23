---
layout: post
title: 通过ucenter同步的关键js代码
category: 技术
tags: [JavaScript, ucenter]
keywords: ucenter,php,同步,关键,js,代码
---


> 最近在做ucenter同步登陆和退出的模块，要通过ucenter实现ucenter所管理的各个应用的同步登陆和退出。

参考：[ucenter接口函数](http://faq.comsenz.com/library/UCenter/interface/interface_user.htm)

## 关于ucenter

### 什么是ucenter
UCenter 的中文意思就是”用户中心“。 UCenter 是 Comsenz 旗下各个产品之间信息直接传递的一个桥梁，通过 UCenter 可以无缝整合 Comsenz 系列产品，实现用户的一站式注册、登录、退出以及社区其他数据的交互。

### ucenter作用
提供同步登录、退出、注册等相关接口，可以实现用户一个账号，在一处登录，全站通行。

## js同步代码
由于同步登陆是通过js方式实现的，我所要做的事情就是将接口返回的js代码输出在html里，通过这样的过程就实现了同步。当用ajax方式来输出接口返回的js代码的时候，还会遇到兼容性的问题，比如在chrome浏览器里可以实现同步，而在ie浏览器里却不能同步。今天终于通过一段js解决了这个问题。其实js很简单。

### 版本1
```javascript
<script>
$.ajax({
  url: url,
  dataType:'json',
  type : 'post',
  async: true,
  success:function(data) {
    var state = data['status'];
    if (state === 'y') {
    //data.info 同步的js代码
    document.writeln(data.info);
    //data.url 同步成功后的跳转地址
    var jump_script = "<script>window.location.href='" + data.url + "'<\/script>";
    document.writeln(jump_script);
    } else {
      //提示登录失败
    }
  }
});
</script>
```

其实最关键的js代码就那么两行， 同步的：  ```document.writeln(data.info)```， 跳转的： ```document.writeln(jump_script)```，都是用的同一个js对象 document.writeln

### 版本2
使用版本1之后遇到了一个问题，会有一段时间页面是一片空白，什么东西都没有，造成的原因恰恰就是因为在执行document.writeln(var)时，var的内容会将当前页面的所有内容覆盖，所以在执行跳转之前，页面会是空白，这样显示很不友好，而且没有给用户提示，于是有了版本2的修改

```javascript
<script>
$.ajax({
  url: url,
  dataType:'json',
  type : 'post',
  async: true,
  success:function(data) {
    var state = data['status'];
    if (state === 'y') {
    //将data.info 同步的js代码 追加到head末尾
    $('head').append(data.info);
    //由于data.info是一段同步的js代码，这段代码要执行需要时间，
    由于无法监测到同步代码的执行时间，延时是为了对这段时间
    setTimeout(function() {
      window.location.href= data.url;
    } , 1000);
    } else {
       //提示登录失败
    }
  }
});
</script>
```

这段代码通过jquery的append把同步的js代码追加在head末尾，不会影响当前页面的展示，在没有登录前可以给用户友好的提示


参考：[document.writeln](https://developer.mozilla.org/zh-CN/docs/Web/API/document.writeln#Parameters)
