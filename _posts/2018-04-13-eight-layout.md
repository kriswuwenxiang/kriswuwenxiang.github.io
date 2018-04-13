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

所谓的三栏布局，通俗来讲就是两边固定，中间自适应。三栏布局对于PC端网站十分常见，通常左右两边导航固定宽度，中间的主体内容随浏览器宽度不同来自适应。



## 5种解决方案

### 一、浮动解决方案
#### 1、流体布局
左右模块各自向左右浮动，并设置中间模块的`margin`值使中间模块宽度自适应。如下面代码：
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
 - 左右模块布局在主体内容前面，页面解析时，左右模块先加载，主体内容无法最先加载。
 - 模块高度必须设定，无法自动适应。

#### 2、BFC布局
BFC 即 Block Formatting Contexts (块级格式化上下文)，它属于上述定位方案的普通流。具有 BFC 特性的元素可以看作是隔离了的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且 BFC 具有普通容器所没有的一些特性。如下面代码：
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
 - 左右模块布局在主体内容前面，页面解析时，左右模块先加载，主体内容无法最先加载。
 - 模块高度必须设定，无法自动适应。

#### 3、圣杯布局
利用的是浮动元素 margin 负值的应用，感兴趣的同学可以上网搜搜原理。主体内容可以优先加载，样式定义就稍微复杂。
如下面代码：
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
      float: left;
      width: 300px;
      margin-left: -100%;
      position: relative;
      left: -300px;
      background-color: red;
    }
    .layout .center{
      float: left;
      width: 100%;
      background-color: yellow;
    }
    .layout .right {
      float: left;
      width: 300px;
      margin-left: -300px;
      position: relative;
      right: -300px;
      background-color: blue;
    }
    .layout div{
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
 - 模块高度必须设定，无法自动适应。
 - 浏览器窗口小的情况下容易出现排版错乱。

#### 4、动态计算布局
calc() = calc(四则运算)，用于动态计算长度值。如下面代码：
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
 - 左右模块布局在主体内容前面，页面解析时，左右模块先加载，主体内容无法最先加载。
 - 模块高度必须设定，无法自动适应。


### 二、绝对定位解决方案
绝对定位布局优点，很快捷，设置很方便，而且也不容易出问题，你可以很快的就能想出这种布局方式。简单实用，并且主要内容可以优先加载。如下面代码：
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
 - 模块高度必须设定，无法自动适应。


### 三、Flex布局解决方案
felxbox布局是css3里新出的一个，它就是为了解决上述两种方式的不足出现的，是比较完美的一个。不需要指定高度，三个模块高度自动同步。目前移动端的布局也都是用flexbox 。如下面代码：
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
 - felxbox的缺点就是不能兼容IE8及以下浏览器。


### 四、Table布局解决方案
表格布局在历史上遭到很多人的摒弃，说表格布局麻烦，操作比较繁琐，其实这是一种误解，在很多场景中，表格布局还是很适用的，比如这个三栏布局，用表格布局就轻易写出来了。还有表格布局的兼容性很好，在flex布局不兼容的时候，可以尝试表格布局。如下面代码：
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
 - 当其中一个单元格高度超出的时候，两侧的单元格也是会跟着一起变高的，而有时候这种效果不是我们想要的。


### 五、Grid布局解决方案
CSS Grid(网格) 布局使我们能够比以往任何时候都可以更灵活构建和控制自定义网格。 Grid(网格) 布局使我们能够将网页分成具有简单属性的行和列。如下面代码：
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
 - 当其中一个单元格高度超出的时候，两侧的单元格也是会跟着一起变高的，而有时候这种效果不是我们想要的。


## 总结