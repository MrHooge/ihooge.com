---
title: SASS 学习指南
date: 2017-03-07 11:31:33
tags: SASS CSS
---

# SASS 学习指南

 安装方法可以参考网上的：
 [Sass 安装](http://www.w3cplus.com/sassguide/install.html)
 [SASS学习指南-阮一峰](http://www.ruanyifeng.com/blog/2012/06/sass.html)
 [Sass GitHub](https://github.com/sass/sass)
# SASS 文档

Sass (Syntactically Awesome StyleSheets) 是对 CSS 的扩展，让 CSS 语言更强大、优雅。 它允许你使用变量、嵌套规则、 mixins、导入等众多功能， 并且完全兼容 CSS 语法。 Sass 有助于保持大型样式表结构良好， 同时也让你能够快速开始小型项目， 特别是在搭配 Compass 样式库一同使用时。

### 特色
>1.完全兼容 CSS3
>2.在 CSS 语言基础上添加了扩展功能，比如变量、嵌套 (nesting)、混合 (mixin)
>3.对颜色和其它值进行操作的{Sass::Script::Functions 函数}
>4.函数库控制指令之类的高级功能
>5.良好的格式，可对输出格式进行定制
>6.支持 Firebug

### 语法
Sass 有两种语法。 第一种被称为 SCSS (Sassy CSS)，是一个 CSS3 语法的扩充版本，这份参考资料使用的就是此语法。 也就是说，所有符合 CSS3 语法的样式表也都是具有相同语法意义的 SCSS 文件。
第二种比较老的语法成为缩排语法（或者就称为 "Sass"）， 提供了一种更简洁的 CSS 书写方式。 它不使用花括号，而是通过缩排的方式来表达选择符的嵌套层级，I 而且也不使用分号，而是用换行符来分隔属性。

> *注：* [SCSS与SASS的区别](http://www.aseoe.com/show-11-589-1.html)

任一语法都可以导入另一种语法撰写的文件中。 只要使用 sass-convert 命令行工具，就可以将一种语法转换为另一种语法：
```javascript
# 将 Sass 转换为 SCSS
$ sass-convert style.sass style.scss

# 将 SCSS 转换为 Sass
$ sass-convert style.scss style.sass
```

 [Sass快速入门](http://www.sass.hk/guide/)
 [Sass中文文档](http://www.sass.hk/docs/)

 [sass语法](http://www.w3cplus.com/sassguide/syntax.html)

 [Compass用法指南](http://www.ruanyifeng.com/blog/2012/11/compass.html)
 一个让你的CSS开发效率会上一个台阶的利器！

## 慕课网 上面的一些关于 SASS 的课程
[慕课网Sass课程](http://www.imooc.com/search/course?words=sass)
 