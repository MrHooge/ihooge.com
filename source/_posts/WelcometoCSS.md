---
title: Welcome to CSS
date: 2017-04-20 15:35:30
tags: CSS
---

# Welcome to CSS!

CSS 是 Cascading Style Sheets. 的缩写
    Cascading：指的是CSS应用一种风格的方式
    Style Sheets 控制Web文档的外观和风格。

CSS和HTML
    HTML 排列页面结构.
    CSS 定义HTML元素的显示方式.

## Why Use CSS?
CSS允许您将特定样式应用于特定的HTML元素。
CSS的主要优点是它允许您将样式与内容分开。

只使用HTML，所有的样式和格式都在同一个地方，这在页面维护时变得相当困难。
>所有格式可以（并且应该）从HTML文档中删除并存储在单独的CSS文件中。
<!--more-->
## 内联CSS
使用内联样式是插入样式表的方法之一。使用内联样式，将独特的样式应用于单个元素。
为了使用内联样式，将样式属性添加到相关标签。
```html
<p style="color:white; background-color:gray;">
    This is an example of inline styling. 
</p>
```


## 嵌入式/内部CSS
内部样式在HTML页面的头部内的`<style>`元素中定义。
```html
<html>
   <head>
      <style>
      p {
         color:white;
         background-color:gray;
      }
      </style>
   </head>
   <body>
      <p>This is my first paragraph. </p>
      <p>This is my second paragraph. </p>
   </body>
</html>
```

## 外部CSS
使用此方法，所有样式规则都包含在单个文本文件中，并以.css扩展名保存。 
然后使用`<link>`标记在HTML中引用此CSS文件。 `<link>`元素位于头部。

## CSS语法
CSS由浏览器解释的样式规则组成，然后应用于文档中的相应元素。 样式规则有三个部分：选择器，属性和值。

## 类型选择器
最常见且易于理解的选择器是类型选择器。此选择器针对页面上的元素类型。
*CSS声明总是以分号结尾，声明组被大括号包围。*

## id和类选择器
id选择器允许您设置具有id属性的HTML元素，而不管它们在文档树中的位置。

## 后代选择器
这些选择器用于选择作为另一元素后代的元素。选择级别时，您可以根据需要选择尽可能多的级别。

## 注释
注释用于解释您的代码，并且可以在以后编辑源代码时帮助您。注释被浏览器忽略。
`/* Comment goes here */`

## 级联
网页的最终外观是不同造型规则的结果。 
形成级联的风格信息的三个主要来源是：
    由页面作者创建的样式表 
    浏览器的默认样式 
    用户指定的样式
## 继承
继承是属性流过页面的方式。除非另有规定，子元素通常将承担父元素的特征。


________________________________________________________________________

## font-family属性
font-family属性指定元素的字体。 字体系列有两种类型：
    字体系列：特定字体系列（如Times New Roman或Arial）
    通用家族：一组具有相似外观的字体系列（如Serif或Monospace）

font-family属性应该包含几个字体名称作为“回退”系统。 当以CSS样式指定网络字体时，添加多个字体名称，以避免意外的行为。 如果客户端计算机由于某种原因没有您选择的，那么它将尝试下一个。 指定通用字体系列是一个很好的做法，让浏览器在通用系列中选择类似的字体，如果没有其他字体可用。



