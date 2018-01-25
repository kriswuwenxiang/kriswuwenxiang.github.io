---
layout:     post
title:      "Javascript事件处理程序方式"
subtitle:   "Javascript事件处理程序的5种方式（兼容写法）"
date:       2018-01-08
author:     "Wwx"
header-img: "images/post/2018-01-08/post-bg.jpg"
catalog: true
tags:
    - Javascript
---


## 引言

> **什么是事件？** 

JavaScript 使我们有能力创建动态页面，事件是可以被 JavaScript 侦测到的行为。网页中的每个元素都可以产生某些可以触发 JavaScript 函数的事件。比方说，我们可以在用户点击某按钮时产生一个 onClick 事件来触发某个函数。[JavaScript 事件参考手册](http://www.w3school.com.cn/jsref/jsref_events.asp)

**注意**： 事件通常与函数配合使用，当事件发生时函数才会执行。


> **什么是事件流？** 

**事件流** 描述的是从页面中接受事件的顺序。IE的事件流是 **事件冒泡流** ，而Netscape的事件流是 **事件捕获流** 。

 1. 事件冒泡，即事件最开始由最具体的元素(文档中嵌套层次最深的那个节点)接收，然后逐级向上转播至最不    具体的节点(文档)。
 2. 事件捕获，即事件最开始由不太具体的节点接收，而最具体的节点最后接收到事件。



### 一、HTML事件处理程序
即在HTML代码中添加事件处理程序，如下面代码：
{% highlight html %}
<html>
<body>
  <input type="button" value="按钮" onclick="clickEvent()">
  <script>
    function clickEvent() { alert("HTML事件处理程序"); }
  </script>
</body>
</html>
{% endhighlight %}
**缺点**： HTML和JavaScript代码紧密耦合，如果要更换事件处理程序，就必须改动HTML代码和JavaScript代码。

### 二、DOM0级事件处理程序
即在Javascript代码中对指定对象添加事件处理程序，如下面代码：
{% highlight html %}
<html>
<body>
  <input type="button" value="按钮">
  <script>
    var btn = document.querySelector("input");
    btn.onclick = function() { alert("DOM0级事件处理程序"); }
  </script>
</body>
</html>
{% endhighlight %}
**缺点**：

 - 不能给一个元素同时添加两个事件。
 - 不能控制元素的事件流（捕获或冒泡）。

### 三、DOM2级事件处理程序
即在Javascript代码中对指定对象添加事件处理程序，如下面代码：
{% highlight html %}
<html>
<body>
  <input type="button" value="按钮">
  <script>
    var btn = document.querySelector("input");
    btn.addEventListener("click", clickEvent, false);
    function clickEvent() { alert("DOM2级事件处理程序"); }
  </script>
</body>
</html>
{% endhighlight %}

DOM2级事件定义了两个方法：
用于处理添加和删除事件处理程序的操作：

 - addEventListener() —— 添加事件侦听器 
 - removeEventListener() —— 删除事件侦听器 

它们都接收三个参数：
 1. 监听事件类型（事件名） —— 使用 "click" ，不要使用 "on" 前缀。
 2. 事件触发时执行的函数。
 3. 表示事件流方式的布尔值：false事件句柄在冒泡阶段执行（默认）；true事件句柄在捕获阶段执行。

**注意**： DOM2级事件是 W3C DOM 规范中提供的注册事件监听器的方法。

**注意**： Internet Explorer 8 及更早IE版本不支持 addEventListener() 方法，Opera 7.0 及 Opera 更早版本也不支持。



### 四、IE事件处理程序

 - attachEvent() —— 添加事件 
 - detachEvent() —— 删除事件 

它们都接收两个参数：

 1. 事件名伴随on —— 使用 "onclick" ，这里不是事件，而是事件处理程序的名称，所以有 "on" 前缀。
 2. 事件触发时执行的函数。

**注意**： 这里没有和DOM2级事件处理程序中类似的第三个参数，是因为IE8及更早版本只支持冒泡事件流。

**DOM2级事件处理程序和IE事件处理程序 `不同点`**：

1. IE事件处理程序中attachEvent()的事件处理程序的作用域和DOM0与DOM2不同，她的作用域是在全局作用域中，不同于DOM0和DOM2中this指向元素，IE中的this指向window。
2. 可以使用attachEvent()来给同一个元素添加多个事件处理程序。但是与DOM2不同，事件触发的顺序不是添加的顺序而是添加顺序的相反顺序。



### 五、跨浏览器的事件处理程序

 - DOM中的事件对象

> (1)、type —— 事件类型 
>
> (2)、target —— 事件目标 
>
> (3)、stopPropagation() —— 阻止事件冒泡
>
> (4)、preventDefault() —— 阻止事件的默认行为

 - IE中的事件对象

> (1)、type —— 事件类型 
>
> (2)、srcElement —— 事件目标 
>
> (3)、cancelBubble=true —— 阻止事件冒泡
>
> (4)、returnValue=false —— 阻止事件的默认行为

为了处理不同浏览器对事件处理程序的不同支持，这里封装一个兼容方法，供有兴趣的同学参考：
{% highlight javascript %}
var eventUtil={
  addHandler:function(element,type,handler){// 添加句柄
    if(element.addEventListener){
      element.addEventListener(type,handler,false);
    }else if(element.attachEvent){
      element.attachEvent('on'+type,handler);
    }else{
      element['on'+type]=handler;
    }
  },
  removeHandler:function(element,type,handler){// 删除句柄
    if(element.removeEventListener){
      element.removeEventListener(type,handler,false);
    }else if(element.detachEvent){
      element.detachEvent('on'+type,handler);
    }else{
      element['on'+type]=null;
    }
  },
  getEvent:function(event){// 获取事件
    return event?event:window.event;
  },
  getType:function(event){//获取事件类型
    return event.type;
  },
  getElement:function(event){// 获取事件目标 
    return event.target || event.srcElement;
  },
  stopPropagation:function(event){// 阻止事件冒泡
    if(event.stopPropagation){
      event.stopPropagation();
    }else{
      event.cancelBubble=true;
    }
  },
  preventDefault:function(event){// 阻止事件的默认行为
    if(event.preventDefault){
      event.preventDefault();
    }else{
      event.returnValue=false;
    }
  }
}
{% endhighlight %}