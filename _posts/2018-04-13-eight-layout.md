---
layout:     post
title:      "三栏布局的5种解决方案"
subtitle:   "网站常见的8种布局方式"
date:       2018-04-13
author:     "Wwx"
header-img: "images/post/2018-04-13/post-bg.jpg"
catalog: true
tags:
    - Html
    - Css
---


## 引言

> **什么是三栏布局？** 

所谓的三栏布局，通俗来讲就是两边固定，中间自适应。三栏布局对于PC端网站十分常见，通常左右两模块固定宽度，中间的主体内容随浏览器宽度不同来自适应。



## 常见定位方案

#### 1、普通流 (normal flow)

在普通流中，元素按照其在 HTML 中的先后位置至上而下布局，在这个过程中，行内元素水平排列，直到当行被占满然后换行，块级元素则会被渲染为完整的一个新行，除非另外指定，否则所有元素默认都是普通流定位，也可以说，普通流中元素的位置由该元素在 HTML 文档中的位置决定。

#### 2、浮动 (float)

在浮动布局中，元素首先按照普通流的位置出现，然后根据浮动的方向尽可能的向左边或右边偏移，其效果与印刷排版中的文本环绕相似。

#### 3、绝对定位 (absolute positioning)

在绝对定位布局中，元素会整体脱离普通流，因此绝对定位元素不会对其兄弟元素造成影响，而元素具体的位置由绝对定位的坐标决定。



## 5种布局解决方案

### 一、浮动解决方案
#### 1、流体布局
左右模块各自向左右浮动，并设置中间模块的 `margin` 值使中间模块宽度自适应。如下面代码：
{% highlight html %}
<!DOCTYPE html>
<html lang="zh">
<head>
  <style>
    html *{
      padding: 0;
      margin: 0;
    }
    .layout .left{
      float:left;
      width:300px;
      background: red;
    }
    .layout .center{
      margin-left: 300px;
      margin-right: 300px;
      background: yellow;
    }
    .layout .right{
      float:right;
      width:300px;
      background: blue;
    }
    .layout div{
      min-height: 100px;
    }
  </style>
</head>
<body>
  <section class="layout">
    <div class="left"></div>
    <div class="right"></div>
    <div class="center">
      <p>浮动解决方案</p>
      <p>你好，这只是一个测试...</p>
      <p>你好，这只是一个测试...</p>
      <p>你好，这只是一个测试...</p>
    </div>
  </section>
</body>
</html>
{% endhighlight %}
**缺点**：
 - 左右模块加载顺序在中间主体内容前面。
 - 模块高度必须设定，三个模块高度无法自动适应。

#### 2、BFC布局
BFC 即 `Block Formatting Contexts` (块级格式化上下文)，它属于普通流。具有 BFC 特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素。BFC 可以阻止元素被浮动元素覆盖。如下面代码：
{% highlight html %}
<!DOCTYPE html>
<html lang="zh">
<head>
  <style>
    html *{
      padding: 0;
      margin: 0;
    }
    .layout .left{
      float:left;
      width:300px;
      background: red;
    }
    .layout .center{
      overflow: hidden;
      background: yellow;
    }
    .layout .right{
      float:right;
      width:300px;
      background: blue;
    }
    .layout div{
      min-height: 100px;
    }
  </style>
</head>
<body>
  <section class="layout">
    <div class="left"></div>
    <div class="right"></div>
    <div class="center">
      <p>浮动解决方案</p>
      <p>你好，这只是一个测试...</p>
      <p>你好，这只是一个测试...</p>
      <p>你好，这只是一个测试...</p>
    </div>
  </section>
</body>
</html>
{% endhighlight %}
**缺点**：
 - 左右模块加载顺序在中间主体内容前面。
 - 模块高度必须设定，三个模块高度无法自动适应。

#### 3、margin 负值布局
利用浮动元素的 `margin 负值`，中间主体内容可以布局到左右模块前面，优先加载。如下面代码：
{% highlight html %}
<!DOCTYPE html>
<html lang="zh">
<head>
  <style>
    html *{
      padding: 0;
      margin: 0;
    }
    .layout {
      margin-left: 300px;
      margin-right: 300px;
    }
    .layout .left {
      width: 300px;
      margin-left: -100%;
      position: relative;
      left: -300px;
      background-color: red;
    }
    .layout .center{
      width: 100%;
      background-color: yellow;
    }
    .layout .right {
      width: 300px;
      margin-left: -300px;
      position: relative;
      right: -300px;
      background-color: blue;
    }
    .layout div{
      float: left;
      min-height: 100px;
    }
  </style>
</head>
<body>
  <section class="layout">
    <div class="center">
      <p>浮动解决方案</p>
      <p>你好，这只是一个测试...</p>
      <p>你好，这只是一个测试...</p>
      <p>你好，这只是一个测试...</p>
    </div>
    <div class="left"></div>
    <div class="right"></div>
  </section>
</body>
</html>
{% endhighlight %}
**缺点**：
 - Css样式定义稍微复杂。
 - 模块高度必须设定，三个模块高度无法自动适应。
 - 浏览器窗口小的情况下可能会出现排版错乱。

#### 4、动态计算布局
利用 `calc` (四则运算)，对中间主体内容宽度进行动态计算。如下面代码：
{% highlight html %}
<!DOCTYPE html>
<html lang="zh">
<head>
  <style>
    html *{
      padding: 0;
      margin: 0;
    }
    .layout .left{
      width: 300px;
      background: red;
    }
    .layout .center{
      width: calc( 100% - 600px );
      background: yellow;
    }
    .layout .right{
      width: 300px;
      background: blue;
    }
    .layout div{
      float: left;
      min-height: 100px;
    }
  </style>
</head>
<body>
  <section class="layout">
    <div class="left"></div>
    <div class="center">
      <p>浮动解决方案</p>
      <p>你好，这只是一个测试...</p>
      <p>你好，这只是一个测试...</p>
      <p>你好，这只是一个测试...</p>
    </div>
    <div class="right"></div>
  </section>
</body>
</html>
{% endhighlight %}
**缺点**：
 - 左右模块加载顺序在中间主体内容前面。
 - 模块高度必须设定，三个模块高度无法自动适应。
 - 不能兼容IE8及以下浏览器。


### 二、绝对定位解决方案
绝对定位布局快捷、简单实用，中间主体内容可以布局到左右模块前面，优先加载。如下面代码：
{% highlight html %}
<!DOCTYPE html>
<html lang="zh">
<head>
  <style>
    html *{
      padding: 0;
      margin: 0;
    }
    .layout .left{
      left:0;
      width: 300px;
      background: red;
    }
    .layout .center{
      left: 300px;
      right: 300px;
      background: yellow;
    }
    .layout .right{
      right:0;
      width: 300px;
      background: blue;
    }
    .layout div{
      position: absolute;
      min-height: 100px;
    }
  </style>
</head>
<body>
  <section class="layout">
    <div class="center">
      <p>绝对定位解决方案</p>
      <p>你好，这只是一个测试...</p>
      <p>你好，这只是一个测试...</p>
      <p>你好，这只是一个测试...</p>
    </div>
    <div class="left"></div>
    <div class="right"></div>
  </section>
</body>
</html>
{% endhighlight %}
**缺点**：
 - 模块高度必须设定，三个模块高度无法自动适应。


### 三、Flex布局解决方案
Flex 是 `Flexible Box` 的缩写，意为"弹性布局"，用来为盒状模型提供最大的灵活性。Flex 布局可以`简便`、`完整`、`响应式`地实现各种页面布局，是目前最常用的布局方式。如下面代码：
{% highlight html %}
<!DOCTYPE html>
<html lang="zh">
<head>
  <style>
    html *{
      padding: 0;
      margin: 0;
    }
    .layout{
      display: flex;
    }
    .layout .left{
      width: 300px;
      background: red;
    }
    .layout .center{
      flex: 1;
      background: yellow;
    }
    .layout .right{
      width: 300px;
      background: blue;
    }
  </style>
</head>
<body>
  <section class="layout">
    <div class="left"></div>
    <div class="center">
      <p>Flex布局解决方案</p>
      <p>你好，这只是一个测试...</p>
      <p>你好，这只是一个测试...</p>
      <p>你好，这只是一个测试...</p>
    </div>
    <div class="right"></div>
  </section>
</body>
</html>
{% endhighlight %}
**缺点**：
 - 不能兼容IE9及以下浏览器。


### 四、Table布局解决方案
表格布局让标签元素以表格单元格的形式呈现，类似于 `table标签`。单元格有一些比较特别的属性，例如元素的垂直居中对齐，关联伸缩等，还是有不少潜在的使用价值的。IE8+ 以及其他现代浏览器都是支持的，在 flex 布局不兼容的时候，可以尝试表格布局。如下面代码：
{% highlight html %}
<!DOCTYPE html>
<html lang="zh">
<head>
  <style>
    html *{
      padding: 0;
      margin: 0;
    }
    .layout{
      width: 100%;
      display: table;
    }
    .layout .left{
      width: 300px;
      background: red;
    }
    .layout .center{
      background: yellow;
    }
    .layout .right{
      width: 300px;
      background: blue;
    }
    .layout div{
      display: table-cell;
    }
  </style>
</head>
<body>
  <section class="layout">
    <div class="left"></div>
    <div class="center">
      <p>Table布局解决方案</p>
      <p>你好，这只是一个测试...</p>
      <p>你好，这只是一个测试...</p>
      <p>你好，这只是一个测试...</p>
    </div>
    <div class="right"></div>
  </section>
</body>
</html>
{% endhighlight %}
**缺点**：
 - table 布局的三栏（单元格）高度都是一致的。


### 五、Grid布局解决方案
Grid 布局即网格布局，使我们能够比以往任何时候都可以更灵活构建和控制自定义网格，将网页分成具有简单属性的行和列。如下面代码：
{% highlight html %}
<!DOCTYPE html>
<html lang="zh">
<head>
  <style>
    html *{
      padding: 0;
      margin: 0;
    }
    .layout{
      width:100%;
      display: grid;
      grid-template-columns: 300px auto 300px;
    }
    .layout .left{
      background: red;
    }
    .layout .center{
      background: yellow;
    }
    .layout .right{
      background: blue;
    }
  </style>
</head>
<body>
  <section class="layout">
    <div class="left"></div>
    <div class="center">
      <p>Grid布局解决方案</p>
      <p>你好，这只是一个测试...</p>
      <p>你好，这只是一个测试...</p>
      <p>你好，这只是一个测试...</p>
    </div>
    <div class="right"></div>
  </section>
</body>
</html>
{% endhighlight %}
**缺点**：
 - 网格布局是一新的布局方式，规范还不够成熟，不少浏览器还不能很好的支持。


## 总结

Web 布局的四个阶段：

 - 1、`table` 表格布局，通过 table 标签布局。

 - 2、`float` 浮动及 `position` 定位布局。

 - 3、`flex` 弹性盒模型布局，是目前最为成熟和强大的布局方案。

 - 4、`grid` 栅格布局，一种基于二维网格的布局系统，具有强大的内容尺寸和定位能力，旨在完全改变我们设计基于网格的用户界面的方式，弥补网页开发在二维布局能力上的缺陷。未来的趋势。