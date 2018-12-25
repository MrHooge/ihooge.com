---
title: JavaScript 秘密花园
date: 2017-04-15 15:35:30
tags: JavaScript
---

# JavaScript 秘密花园

## 对象

*JavaScript* 中所有变量都可以当作对象使用，除了两个例外 *null* 和 *undefined*。

*数字的字面值不能当作对象使用* 若要使用也是可以的，例如：
```javascript
    2.toString(); // 出错：SyntaxError

    有很多变通方法可以让数字的字面值看起来像对象。

    2..toString(); // 第二个点号可以正常解析
    2 .toString(); // 注意点号前面的空格
    (2).toString(); // 2先被计算

```
<!--more-->
### 对象作为数据类型
*JavaScript* 的对象可以作为哈希表使用，主要用来保存命名的键与值的对应关系。
使用对象的字面语法`{}`可以创建一个简单对象。这个新创建的对象从 Object.prototype 继承，没有任何自定义属性。
```javascript
    var foo = {}; // 一个空对象
```

### 访问属性
有两种方式来访问对象的属性，点操作符或者中括号操作符.
```javascript
var foo = {name: 'kitten'}
foo.name; // kitten
foo['name']; // kitten
```
两种语法是等价的，但是中括号操作符在下面两种情况下依然有效
>动态设置属性
>属性名不是一个有效的变量名（译者注：比如属性名中包含空格，或者属性名是 JS 的关键词）

### 删除属性
删除属性的唯一方法是使用 `delete` 操作符；设置属性为 `undefined` 或者 `null` 并不能真正的删除属性， 而仅仅是移除了属性和值的关联。
```javascript
    var obj = {
        bar: 1,
        foo: 2,
        baz: 3
    };
    obj.bar = undefined;
    obj.foo = null;
    delete obj.baz;
```

### 属性名的语法
```javascript
    var test = {
        'case': 'I am a keyword so I must be notated as a string',
        delete: 'I am a keyword too so me' // 出错：SyntaxError
    };
```

对象的属性名可以使用字符串或者普通字符声明。但是由于 JavaScript 解析器的另一个错误设计， 上面的第二种声明方式在 ECMAScript 5 之前会抛出 SyntaxError 的错误。

这个错误的原因是 delete 是 JavaScript 语言的一个关键词；因此为了在更低版本的 JavaScript 引擎下也能正常运行， 必须使用字符串字面值声明方式。

## 原型
JavaScript 不包含传统的类继承模型，而是使用 prototype 原型模型。

虽然这经常被当作是 JavaScript 的缺点被提及，其实基于原型的继承模型比传统的类继承还要强大。 实现传统的类继承模型是很简单，但是实现 JavaScript 中的原型继承则要困难的多。
由于 JavaScript 是唯一一个被广泛使用的基于原型继承的语言，所以理解两种继承模式的差异是需要一定时间的。
>第一个不同之处在于 JavaScript 使用原型链的继承方式。
>第二个不同之处在于 JavaScript 使用对象冒充的继承方式。
参考链接：http://www.cnblogs.com/asqq/archive/2012/10/05/3194957.html

### 属性查找 
当查找一个对象的属性时，JavaScript 会向上遍历原型链， 直到找到给定名称的属性为止。也就是 Object.prototype - 但是仍然没有找到指定的属性，就会返回 undefined。

### 原型属性
当原型属性用来创建原型链时，可以把任何类型的值赋给它（prototype）。 然而将原子类型赋给 prototype 的操作将会被忽略。而将对象赋值给 prototype，将会动态的创建原型链。

### 性能  
如果一个属性在原型链的上端，则对于查找时间将带来不利影响。特别的，试图获取一个不存在的属性将会遍历整个原型链。

### 扩展内置类型的原型   
扩展 Object.prototype 或者其他内置类型的原型对象。这种技术被称之为 monkey patching 并且会破坏封装。虽然它被广泛的应用到一些 JavaScript 类库中比如 Prototype, 但是仍不认为为内置类型添加一些非标准的函数是个好主意。
扩展内置类型的唯一理由是为了和新的 JavaScript 保持一致，比如 Array.forEach。

## hasOwnProperty 函数
为了判断一个对象是否包含自定义属性而不是原型链上的属性， 我们需要使用继承自 Object.prototype 的 hasOwnProperty 方法。

hasOwnProperty 是 JavaScript 中唯一一个处理属性但是不查找原型链的函数。

### hasOwnProperty 作为属性

JavaScript不会被保护hasOwnProperty被非法占用，因此如果一个对象碰巧存在这个属性， 就需要使用外部的 hasOwnProperty 函数来获取正确的结果。

    当检查对象上某个属性是否存在时，hasOwnProperty 是唯一可用的方法。 同时在使用 for in loop 遍历对象时，推荐总是使用 hasOwnProperty 方法， 这将会避免原型对象扩展带来的干扰。

## for in 循环
和 in 操作符一样，for in 循环同样在查找对象属性时遍历原型链上的所有属性。
由于不可能改变 for in 自身的行为，因此有必要过滤出那些不希望出现在循环体中的属性， 这可以通过 Object.prototype 原型上的 hasOwnProperty 函数来完成。

### 使用 hasOwnProperty 过滤
一个广泛使用的类库 Prototype 就扩展了原生的 JavaScript 对象。 因此，当这个类库被包含在页面中时，不使用 hasOwnProperty 过滤的 for in 循环难免会出问题。
推荐使用 hasOwnProperty。不要对代码运行的环境做任何假设，不要假设原生对象是否已经被扩展了。

# 函数
## 函数声明与表达式
函数是JavaScript中的一等对象，这意味着可以把函数像其它值一样传递。 一个常见的用法是把匿名函数作为回调函数传递到异步函数中。

### 函数声明
方法会在执行前被 解析(hoisted)，因此它存在于当前上下文的任意一个地方， 即使在函数定义体的上面被调用也是对的。

## this 的工作原理
JavaScript 有一套完全不同于其它语言的对 this 的处理机制。 在五种不同的情况下 ，this 指向的各不相同。
### 全局范围内
当在全局范围内使用 this，它将会指向全局对象。

### 函数调用
这里 this 也会指向全局对象。

*ES5 注意: 在严格模式下（strict mode），不存在全局变量。 这种情况下 this 将会是 undefined。

###方法调用
方法调用中，this 指向当前对象。

### 调用构造函数
如果函数倾向于和 new 关键词一块使用，则我们称这个函数是 构造函数。 在函数内部，this 指向新创建的对象。

### 显式的设置 this 
当使用 Function.prototype 上的 call 或者 apply 方法时，函数内的 this 将会被 显式设置为函数调用的第一个参数。

尽管大部分的情况都说的过去，不过第一个规则（这里指的应该是第二个规则，也就是直接调用函数时，this 指向全局对象） 被认为是JavaScript语言另一个错误设计的地方，因为它从来就没有实际的用途。

###方法的赋值表达式
另一个看起来奇怪的地方是函数别名，也就是将一个方法赋值给一个变量。

##闭包和引用

闭包是 JavaScript一个非常重要的特性，这意味着当前作用域总是能够访问外部作用域中的变量。因为函数是JavaScript中唯一拥有自身作用域的结构，因此闭包的创建依赖于函数。
