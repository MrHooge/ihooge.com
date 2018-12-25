---
title: 《JavaScript设计模式》学习总结(一)
date: 2017-04-17 22:40:32
tags: JavaScript 设计模式
---

# 面向对象编程

## 灵活的语言——JavaScript

**普通的函数：**
```javascript
    function checkName(){}
    function checkEmail(){}
```
>这样就无形中创建了很多全局变量

**函数另一种形式**
```javascript
    var checkName = function(){}
    var checkEmail = function(){}
```
>这样写只是将函数保存到变量中来实现函数的功能

**用对象收编变量**
```javascript
    var CheckObject = {
        checkName : function(){}
        checkEmail : function(){}
    }
```
>这样我们将所有的函数作为CheckObject对象的方法，这样我们就只有一个对象，而且使用它也很简单：CheckObject.checkName()。

**对象的另一种形式**
```javascript
    var CheckObject = function(){}
    CheckObject.checkName = function(){}
    CheckObject.checkEmail = function(){}
```
>使用的方式和前面的一样：CheckObject.checkName()；但是该对象不能复制或者说这个对象在用*new*关键字时，新创建的对象不能继承这些方法。

**真假对象**
```javascript
    var CheckObject = function(){
        return {
            checkName : function(){},
            checkEmail : function(){}
        }
    }

    var a = CheckObject();
    a.checkEmail(); // undefined
```
>当每次调用这个函数时都返回一个新的对象，这样执行过程中表面上是CheckObject对象，可实际是返回一个新的对象。

<!-- more -->

**类也可以**
```javascript
    var CheckObject = function(){
        this.checkName = function(){}
        this.checkEmail = function(){}
    }

    var a = new CheckObject();
    a.checkEmail(); // undefined
```
>像这样的对象就可以看成类啦，就可以用 *new* 关键字创建对象啦。

**一个检测类**
```javascript
    var CheckObject = function(){};
    CheckObject.prototype.CheckName = function(){}
    CheckObject.prototype.checkEmail = function(){}
```
or
```javascript
    var CheckObject = function(){};
    CheckObject.prototype = {
        CheckName : function(){},
        checkEmail : function(){}
    }

    //使用
    var a = new CheckObject();
    a.checkName();
    a.checkEmail();
```
>这样每次调用方法，就要写一次对象。这是可以避免的，那就是要在你声明的每一个方法末尾处将当前对象返回，在JavaScript中this指向的就是当前对象，所以你可以将它返回。

**方法还可以这样用**
```javascript
    var CheckObject = function(){};
    CheckObject.prototype = {
        CheckName : function(){
            return this;
            },
        checkEmail : function(){
            return this;
        }
    }
    //使用
    CheckObject.checkName().checkEmail();
```
or
```javascript
    var CheckObject = function(){};
    CheckObject.prototype = {
        CheckName : function(){
            return this;
            },
        checkEmail : function(){
            return this;
        }
    }
    //使用
    var a = new CheckObject();
    a.checkName().checkEmail();
```

**函数的祖先**
prototypr.js : 一款JavaScript框架，里面为我们方便的封装了很多方法，它最大的特点就是对源生对象（JavaScript语言为我们提供的对象类，如Function、Array、Object等等）的拓展。
```javascript
    添加一个方法：
    Function.prototype.checkEmail = function(){}
    //使用
    var a = function(){};
    a.checkEmail();
    or :
    var f = new Function();
    f.checkEmail();
```
>这样做会污染源生对象Function，所以别人创建的函数也会被你创建的函数所污染，造成不必要的开销，但是你可以抽象出一个统一添加方法的功能方法。

```javascript
    Function.prototype.addMethod = function (name, fn){
        this[name] = fn;
    }
    //使用
    var methods = function(){}; //or  var methods = new Function();
    methods.addMethod('checkName',function(){});
    methods.checkName();
```

**链式添加**
```javascript
    Function.prototype.addMethod = function (name, fn){
        this[name] = fn;
        return this;
    }
    //使用
    var methods = function(){}; //or  var methods = new Function();
    methods.addMethod('checkName',function(){
    }).addMethods('checkEmail',function(){
    });
    methods.checkName();
    methods.checkEmail();
    //链式使用
    var methods = function(){}; //or  var methods = new Function();
    methods.addMethod('checkName',function(){
        return this;
    }).addMethods('checkEmail',function(){
        return this;
    });
    methods.checkName().checkEmail();
```

**换一种方式使用**
```javascript
    Function.prototype.addMethod = function (name, fn){
        this.prototype[name] = fn;
        return this;
    }
    //使用
    var Methods = function(){}; //or  var methods = new Function();
    Methods.addMethod('checkName',function(){
    }).addMethods('checkEmail',function(){
    });
    //此时要通过new关键字来创建新对象
    var m = new Methods();
    m.checkName();
```

**总结**
“灵活性”是这门语言特有的气质，不同的人可以写出不同风格的代码，这是JavaScript给予我们的财富，不过我们要在团队开发中慎重挥霍，尽量保证团队开发代码风格的一致性，这也是团队代码易开发、可维护以及代码规范的必然要求。





