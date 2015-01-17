---
layout: post
title: 通过ucenter同步的关键js代码
category: 技术
tags: [PHP, ucenter]
keywords: ucenter,php,同步,关键,js,代码
---


> 最近在做ucenter同步登陆和退出的模块，要通过ucenter实现ucenter所管理的各个应用的同步登陆和退出。

参考：[ucenter接口函数](http://faq.comsenz.com/library/UCenter/interface/interface_user.htm)

##关于ucenter

###什么是ucenter
    UCenter 的中文意思就是”用户中心“。 UCenter 是 Comsenz 旗下各个产品之间信息直接传递的一个桥梁，通过 UCenter 可以无缝整合 Comsenz 系列产品，实现用户的一站式注册、登录、退出以及社区其他数据的交互。

###ucenter作用
    提供同步登录、退出、注册等相关接口，可以实现用户一个账号，在一处登录，全站通行。

##js同步代码
    由于同步登陆是通过js方式实现的，我所要做的事情就是将接口返回的js代码输出在html里，通过这样的过程就实现了同步。当用ajax方式来输出接口返回的js代码的时候，还会遇到兼容性的问题，比如在chrome浏览器里可以实现同步，而在ie浏览器里却不能同步。今天终于通过一段js解决了这个问题。其实js很简单。

```javascript
<javascriipt>
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
</javascriipt>
```

其实最关键的js代码就那么两行， 同步的：  ```document.writeln(data.info)```， 跳转的： ```document.writeln(jump_script)```，都是用的同一个js对象 document.writeln

参考：[document.writeln](https://developer.mozilla.org/zh-CN/docs/Web/API/document.writeln#Parameters)
