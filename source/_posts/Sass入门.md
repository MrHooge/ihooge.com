---
title: Sass 入门
date: 2017-03-08 22:20:38
tags: Sass
---

# Sass 入门

## 什么是 CSS 预处理器？
    CSS 预处理器定义了一种新的语言，其基本思想是，用一种专门的编程语言，为 CSS
     增加了一些编程的特性，将CSS作为目标生成文件，然后开发者就只要使用这种语言
     进行编码工作。

通俗的说，“CSS预处理器用一种专门的编程语言，进行Web页面样式设计，然后再编译成正常的 CSS 文件，以供项目使用。CSS预处理器为CSS增加一些编程的特性，无需考虑浏览器的兼容性问题”，例如你可以在 CSS 中使用变量、简单的逻辑程序、函数（如右侧代码编辑器中就使用了变量$color）等等在编程语言中的一些基本特性，可以让你的CSS更加简洁、适应性更强、可读性更佳，更易于代码的维护等诸多好处。

```css
$color: red;

.test {
    color: $color;
}
```

## 什么是 Sass？
Sass 官网上是这样描述 Sass 的：

>Sass是一门高于CSS的元语言，它能用来清晰地、结构化地描述文件样式，有着比普通CSS更加强大的功能。Sass能够提供更简洁、更优雅的语法，同时提供多种功能来创建可维护和管理的样式表。

## Sass 和 SCSS 有什么区别？
Sass 和 SCSS 其实是同一种东西，我们平时都称之为 Sass，两者之间不同之处有以下两点：

>1.文件扩展名不同，Sass 是以“.sass”后缀为扩展名，而 SCSS 是以“.scss”后缀为扩展名
>2.语法书写方式不同，Sass 是以严格的缩进式语法规则来书写，不带大括号({})和分号(;)，而 SCSS 的语法书写和我们的 CSS 语法书写方式非常类似。
<!-- more -->

**Sass 语法**
```css
$font-stack: Helvetica, sans-serif  //定义变量
$primary-color: #333 //定义变量

body
  font: 100% $font-stack
  color: $primary-color
```

**SCSS 语法**
```css
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

**编译出来的 CSS**
```css
body {
  font: 100% Helvetica, sans-serif;
  color: #333;
}
```

##  Sass的基本特性-基础
**声明变量**
定义变量的语法：
在有些编程语言中（如，JavaScript）声明变量都是使用关键词“var”开头，但是在 Sass 不使用这个关键词，而是使用大家都喜欢的美元符号“$”开头。
>$width:300px;
>"$":声明符，"width":变量，"300px":变量值。

**普通变量与默认变量***
**默认变量**
sass 的默认变量仅需要在值后面加上 !default 即可。
```
$baseLineHeight:1.5 !default;
body{
    line-height: $baseLineHeight; 
}
编译后的css代码：
body{
    line-height:1.5;
}
```

sass 的默认变量一般是用来设置默认值，然后根据需求来覆盖的，覆盖的方式也很简单，只需要在默认变量之前重新声明下变量即可。
```
$baseLineHeight: 2;
$baseLineHeight: 1.5 !default;
body{
    line-height: $baseLineHeight; 
}
编译后的css代码：
body{
    line-height:2;
}
```

可以看出现在编译后的 line-height 为 2，而不是我们默认的 1.5。默认变量的价值在进行组件化开发的时候会非常有用。
**变量的调用**
在 Sass 中声明了变量之后，就可以在需要的地方调用变量。调用变量的方法也非常的简单。

比如在定义了变量
```
$brand-primary : darken(#428bca, 6.5%) !default; // #337ab7
$btn-primary-color: #fff !default;
$btn-primary-bg : $brand-primary !default;
$btn-primary-border : darken($btn-primary-bg, 5%) !default;
```
>*注：*darken()  Decrease the lightness of a color in the HSL color space by an absolute amount. 将HSL颜色空间中的颜色的亮度减少绝对量。
>[darken](http://lesscss.org/functions/#color-operations-darken)

在按钮 button 中调用，可以按下面的方式调用
```
.btn-primary {
   background-color: $btn-primary-bg;
   color: $btn-primary-color;
   border: 1px solid $btn-primary-border;
}
```
编译出来的CSS:
```
.btn-primary {
  background-color: #337ab7;
  color: #fff;
  border: 1px solid #2e6da4;
}
```

**局部变量和全局变量**
```//SCSS
$color: orange !default;//定义全局变量(在选择器、函数、混合宏...的外面定义的变量为全局变量)
.block {
  color: $color;//调用全局变量
}
em {
  $color: red;//定义局部变量
  a {
    color: $color;//调用局部变量
  }
}
span {
  color: $color;//调用全局变量
}
```

css 的结果：
```
//CSS
.block {
  color: orange;
}
em a {
  color: red;
}
span {
  color: orange;
}
```
上面的示例演示可以得知，在元素内部定义的变量不会影响其他元素。如此可以简单的理解成，全局变量就是定义在元素外面的变量，如下代码：
>$color:orange !default;

$color 就是一个全局变量，而定义在元素内部的变量，比如 $color:red; 是一个局部变量。
除此之外，Sass 现在还提供一个 !global 参数。!global 和 !default 对于定义变量都是很有帮助的。

**嵌套-选择器嵌套**
Sass 中还提供了选择器嵌套功能，但这也并不意味着你在 Sass 中的嵌套是无节制的，因为你嵌套的层级越深，编译出来的 CSS 代码的选择器层级将越深，这往往是大家不愿意看到的一点。这个特性现在正被众多开发者滥用。

选择器嵌套为样式表的作者提供了一个通过局部选择器相互嵌套实现全局选择的方法，Sass 的嵌套分为三种：

>1.选择器嵌套
>2.属性嵌套
>3.伪类嵌套

1、选择器嵌套
假设我们有一段这样的结构：
```
<header>
<nav>
    <a href=“##”>Home</a>
    <a href=“##”>About</a>
    <a href=“##”>Blog</a>
</nav>
<header>
```
想选中 header 中的 a 标签，在写 CSS 会这样写：
```
nav a {
  color:red;
}

header nav a {
  color:green;
}
```
那么在 Sass 中，就可以使用选择器的嵌套来实现：
```
nav {
  a {
    color: red;

    header & {
      color:green;
    }
  }  
}
```

**嵌套-属性嵌套**
Sass 中还提供属性嵌套，CSS 有一些属性前缀相同，只是后缀不一样，比如：border-top/border-right，与这个类似的还有 margin、padding、font 等属性。假设你的样式中用到了：
```
.box {
    border-top: 1px solid red;
    border-bottom: 1px solid green;
}
```
在 Sass 中我们可以这样写：
```
.box {
  border: {
   top: 1px solid red;
   bottom: 1px solid green;
  }
}
```

**嵌套-伪类嵌套**
其实伪类嵌套和属性嵌套非常类似，只不过他需要借助`&`符号一起配合使用。我们就拿经典的“clearfix”为例吧：
```
.clearfix{
&:before,
&:after {
    content:"";
    display: table;
  }
&:after {
    clear:both;
    overflow: hidden;
  }
}
```
编译出来的 CSS：
```
clearfix:before, .clearfix:after {
  content: "";
  display: table;
}
.clearfix:after {
  clear: both;
  overflow: hidden;
}
```

>避免选择器嵌套：
选择器嵌套最大的问题是将使最终的代码难以阅读。开发者需要花费巨大精力计算不同缩进级别下的选择器具体的表现效果。
选择器越具体则声明语句越冗长，而且对最近选择器的引用(&)也越频繁。在某些时候，出现混淆选择器路径和探索下一级选择器的错误率很高，这非常不值得。
为了防止此类情况，我们应该尽可能避免选择器嵌套。然而，显然只有少数情况适应这一措施。

**注释**
注释对于一名程序员来说，是极其重要，良好的注释能帮助自己或者别人阅读源码。在 Sass 中注释有两种方式，我暂且将其命名为：

1、类似 CSS 的注释方式，使用 ”/* ”开头，结属使用 ”*/ ”
2、类似 JavaScript 的注释方式，使用“//”

## Sass运算
加法

加法运算是 Sass 中运算中的一种，在变量或属性中都可以做加法运算。如：
```
.box {
  width: 20px + 8in;
}
```
编译出来的 CSS:
```
.box {
  width: 788px;
}
```
但对于携带不同类型的单位时，在 Sass 中计算会报错，如下例所示：
```
.box {
  width: 20px + 1em;
}
```
编译的时候，编译器会报错：“Incompatible units: 'em' and ‘px'.”
>减法 、乘法、除法 同理； 但是数值得单位类型 要相同
