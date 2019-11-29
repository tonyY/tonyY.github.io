---
layout: post
category: web
title: jQuery 禁用表单提交按钮，防止用户请求重复提交
tagline: by 沉默王二
tags: 
  - jQuery
---

当页面上有表单的时候，为了防止用户等不及服务器端响应重复点击提交按钮向服务器端发送重复请求，我们通常需要在请求提交之前将提交按钮禁用。

<!--more-->


先来看一下页面。

![](http://www.itwanger.com/assets/images/2019/11/web-jquery-form-disable-submit-1.png)

当用户点击提交申请这个按钮时，我们需要将其禁用。

```js
$("button.btn-submit", $form).attr("disabled","true");
```

一般请求结束会遇到两种情况，一种是 success，一种是 error，如果要在这两个函数中去掉按钮的禁用状态，稍显麻烦。更偷懒的做法是，在禁用按钮后设置一个定时，比如说 3 秒，定时结束后自动取消禁用状态。

```js
var _submit = $("button.btn-submit", $form).attr("disabled","true");
setTimeout(function(){
	_submit.removeAttr('disabled');
},3000);
```

好了，大功告成，页面级别上的有效措施已经完成。这会在一定程度上降低请求重复提交的可能性。服务器端也需要做好对应的处理。



















