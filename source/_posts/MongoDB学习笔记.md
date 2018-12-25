---
title: MongoDB
date: 2017-07-12 18:20:58
tags: MongoDB mongoose Node.js
---

# MongoDB
## 什么是MongoDB？
它是介于关系型数据库和非关系型数据库之间的一种NoSQL数据库，用C++编写，是一款集敏捷性、可伸缩性、扩展性于一身的高性能的面向文档的通用数据库。

## 为什么要用MongoDB？
    它具有以下几个特征：
    a）、灵活的文档数据模型
        可以非常容易的存储不同结构的的数据，并且还能动态的修改这些数据的源结构模式
    b）、可伸缩可扩展性
        从单个服务器到数千个节点，MongoDB可以很轻松的进行水平扩展，部署多个数据中心
    c）、二级索引
        包括在完全一致的任何字段上的索引、地理空间、文本搜索以及TTL索引，都能进行快速、细粒度的访问到数据
    d）、丰富的查询语言
        MongoDB的查询语言提供了多样化的字段级别的操作符、数据类型以及即时更新。几乎提供了所有编程语言的驱动来更直观的使用它
    e）、健壮的操作工具
        MongoDB的管理服务和运维管理工具可以使你很轻松的部署，监控、备份和规划它

## 安装MongoDB

更详细的说明可以查看[《MongoDB的下载、安装与部署》](http://www.cnblogs.com/wangjieguang/p/mongodbone.html?utm_source=tuicool&utm_medium=referral)
## 可视化工具
Robomongo是一款不错的可视化工具，Windows、MacOS、Linux平台都有，界面也简洁。虽然有收费，普通操作免费版足够用。
[官网地址](https://robomongo.org/)

<!-- more -->
## 引入与连接

### 引入模块

在需要使用的js文件中引入模块。

`var mongoose = require('mongoose');`
### 连接数据库

`var db = mongoose.connect('mongodb://localhost/mongodb');`

URL以`mongodb://` + `[用户名:密码@]` +`数据库地址[:端口]` + `数据库名`。（默认端口27017）
需要对连接状况进行判断，可以用以下代码：

```javascript
db.connection.on("error", function (error) {  
    console.log("数据库连接失败：" + error);
});

db.connection.on("open", function () {  
    console.log("数据库连接成功"); 
})

db.connection.on('disconnected', function () {    
    console.log('数据库连接断开');  
})
```


## 基本概念

最常接触到的有三个概念Schema、Model、Entity。按自己理解，
Schema是定义数据库的结构。类似创建表时的数据定义，但比创建数据库可以做更多的定义，只是没办法通过Schema对数据库进行更改。
Model是将Schema定义的结构赋予表名。但可用此名对数据库进行增删查改。
Entity是将Model与具体的数据绑定，可以对具体数据自身进行操作，例如保存数据。

### Schema

Schema用来定义数据库文档结构，数据库有什么字段、字段是什么类型、默认值、主键之类的信息。除了定义结构外，还能定义文档的实例方法，静态模型方法，复合索引，中间件等。详情查看mongoose官方文档。
在引入Mongoose模块`var mongoose = require('mongoose')`的js文件中进行操作。
```javascript
var blogSchema = new mongoose.Schema({
    title:  String,
    comments: [{ body: String, date: Date }],
    date: { type: Date, default: Date.now },
    hidden: Boolean,
    meta: {
        votes: Number,
        favs:  Number
  }
})
```
这样即定义了一个名为blogSchema的Schema。

如需再添加数据，用add方法。

`blogSchema.add( { author: String, body: String} );`
资料中介绍，Shema不仅定义了文档的结构和属性，还可以定义文档的插件、实例方法、静态方法、复合索引文档生命周期钩子，具体还需查看官方文档。

### Schema.Type

Schema.Type是Mongoose内部定义的数据类型。基本类型有：String、Number、Date、Boolean、Array、Buffer、Mixed、ObjectId。

### Mixed
混合数据类型，可以直接定义{}来使用，以下两种形式等价。
```javascript
new Schema({mixed: {Schema.Types.Mixed} });
new Schema({mixed: {} });
```
### ObjectId

储存在数据库中的每个数据都会有默认的主键_id，默认存储的是ObjectId。
ObjectId是一个12字节的BSON类型字符串。按照字节顺序依次代表：

    4字节：UNIX时间戳
    3字节：表示运行MongoDB的机器
    2字节：表示生成此_id的进程
    3字节：由一个随机数开始的计数器生成的值

### Model

`var blogModel = mongoose.model('Blog', blogSchema);`

将名为blogSchema的Schema与Blog名字绑定，即是存入数据库的名字，但存入数据库中的名字是Blogs，会自动添加一个s。
这里将Model命名为blogModel，需要对Blog表操作的话，都需要使用变量名blogModel。

## Entity

可以绑定具体数据对Model实例化。
```javascript
var blogEntity = new blogModel({
    title:  "Mongoose",
    author: "L",
    body:   "Documents are instances of out model. Creating them and saving to the database is easy",
    comments: [{ body: "It's very cool! Thanks a lot!", date: "2014.07.28" }],
    hidden: false,
    meta: {
        votes: 100,
        favs:  99
    }
})
```

这里将名为blogModel的Model实例化。之后我们可以用blogEntity名对数据进行保存并执行回调。
```javascript
blogEntity.save(function(err, docs){
    if(err) console.log(err);
    console.log('保存成功：' + docs);
})
```

在平常使用SQL语句操作数据库时，取得数据后先组织成SQL语句，然后放入执行语句中执行。这里理解也是类似，取得数据先进行实例化，这一步类似于组织成SQL语句，然后再做具体操作例如上面的Save操作。但由于Node.js是异步操作，所以返回的数据利用回调函数来进行操作。

知道了以上概念后就可以对数据进行操作了，下面将列出一些常用的资料，并附上相应的例子。

## 增查改删(CRUD)

所有的参数都是以JSON对象形式传入。

### 增(C)

>  [Model.create(doc(s), [callback])](http://mongoosejs.com/docs/api.html#model_Model.create)
```javascript
var doc = ({
    title:  "Mongoose",
    author: "L",
    body:   "Documents are instances of out model. Creating them and saving to the database is easy",
    comments: [{ body: "It's very cool! Thanks a lot!", date: "2014.07.28" }],
    hidden: false,
    meta: {
        votes: 100,
        favs:  99
    }
};

blogModel.create(doc, function(err, docs){
    if(err) console.log(err);
    console.log('保存成功：' + docs);
});
```
> Model#save([options], [options.safe], [options.validateBeforeSave], [fn])
```javascript
var blogEntity = new blogModel({
    title:  "Mongoose",
    author: "L",
    body:   "Documents are instances of out model. Creating them and saving to the database is easy",
    comments: [{ body: "It's very cool! Thanks a lot!", date: "2014.07.28" }],
    hidden: false,
    meta: {
        votes: 100,
        favs:  99
    }
});

blogEntity.save(function(err, docs){
    if(err) console.log(err);
    console.log('保存成功：' + docs);
});
```
> Model.insertMany(doc(s), [options], [callback])

多条数据插入，将多条数据一次性插入，相对于循环使用create保存会更加快。
```javascript
blogModel.insertMany([
    {title: "mongoose1", author: "L"}, 
    {title: "mongoose2", author: "L"}
    ], function(err, docs){
        if(err) console.log(err);
        console.log('保存成功：' + docs);
});
```

## 查(R)

> Model.find(conditions, [projection], [options], [callback])

conditions：查询条件；projection：控制返回的字段；options：控制选项；callback：回调函数。
```javascript
blogModel.find({title: "Mongoose", meta.votes: 100}, {title: 1, author: 1, body: 1}, function(err, docs){
    if(err) console.log(err);
    console.log('查询结果：' + docs);
})
```

查询“title”标题为“Mongoose”，并且“meta”中“votes”字段值为“100”的记录，返回仅返回“title”、“author”、“body”三个字段的数据。

> Model.findOne([conditions], [projection], [options], [callback])

conditions：查询条件；projection：控制返回的字段；options：控制选项；callback：回调函数。
只返回第一个查询记录。

> Model.findById(id, [projection], [options], [callback])

id：指定_id的值；projection：控制返回的字段；options：控制选项；callback：回调函数。

### 改(U)

> Model.update(conditions, doc, [options], [callback])

conditions：查询条件；doc：需要修改的数据，不能修改主键（_id）；options：控制选项；callback：回调函数，返回的是受影响的行数。

    options有以下选项：
    　　safe (boolean)： 默认为true。安全模式。
    　　upsert (boolean)： 默认为false。如果不存在则创建新记录。
    　　multi (boolean)： 默认为false。是否更新多个查询记录。
    　　runValidators： 如果值为true，执行Validation验证。
    　　setDefaultsOnInsert： 如果upsert选项为true，在新建时插入文档定义的默认值。
    　　strict (boolean)： 以strict模式进行更新。
    　　overwrite (boolean)： 默认为false。禁用update-only模式，允许覆盖记录。
```javascript
blogModel.update({title: "Mongoose"}, {author: "L"}, {multi: true}, function(err, docs){
    if(err) console.log(err);
    console.log('更改成功：' + docs);
})
```

以上代码先查询“title”为“Mongoose”的数据，然后将它的“author”修改为“L”，“multi”为true允许更新多条查询记录。

> Model.updateMany(conditions, doc, [options], [callback])

一次更新多条

> Model.updateOne(conditions, doc, [options], [callback])

一次更新一条

> Model.findByIdAndUpdate(id, [update], [options], [callback])

id：指定_id的值；update：需要修改的数据；options控制选项；callback回调函数。

    options有以下选项：
    　　new： bool - 默认为false。返回修改后的数据。
    　　upsert： bool - 默认为false。如果不存在则创建记录。
    　　runValidators： 如果值为true，执行Validation验证。
    　　setDefaultsOnInsert： 如果upsert选项为true，在新建时插入文档定义的默认值。
    　　sort： 如果有多个查询条件，按顺序进行查询更新。
    　　select： 设置数据的返回。

> Model.findOneAndUpdate([conditions], [update], [options], [callback])

conditions：查询条件；update：需要修改的数据；options控制选项；callback回调函数。

    options有以下选项：
    　　new： bool - 默认为false。返回修改后的数据。
    　　upsert： bool - 默认为false。如果不存在则创建记录。
    　　fields： {Object|String} - 选择字段。类似.select(fields).findOneAndUpdate()。
    　　maxTimeMS： 查询用时上限。
    　　sort： 如果有多个查询条件，按顺序进行查询更新。
    　　runValidators： 如果值为true，执行Validation验证。
    　　setDefaultsOnInsert： 如果upsert选项为true，在新建时插入文档定义的默认值。
    　　passRawResult： 如果为真，将原始结果作为回调函数第三个参数。

### 删(D)

> Model.remove(conditions, [callback])
```javascrit
blogModel.remove({author: "L"}, function(err, docs){
    if(err) console.log(err);
    console.log('删除成功：' + docs);
})
```

删除“author”值为“L”的记录。

> Model.findByIdAndRemove(id, [options], [callback])

id：指定_id的值；update：需要修改的数据；options控制选项；callback回调函数。

    options有以下选项：
    　　sort： 如果有多个查询条件，按顺序进行查询更新。
    　　select： 设置数据的返回。

> Model.findOneAndRemove(conditions, [options], [callback])

conditions：查询条件；update：需要修改的数据；options控制选项；callback回调函数。

    options有以下选项：
    　　sort： 如果有多个查询条件，按顺序进行查询更新。
    　　maxTimeMS： 查询用时上限。
    　　select： 设置数据的返回。

### 复杂条件查询

在之前的查询说明中仅演示了确定值的查询，如果遇到更加复杂的情况就需要使用其他一些方法。
详细的文档可以在这儿查找 mongodb查询符。

> Query#exec([operation], [callback])

执行查询，回调函数。

使用find()、$where之类查询返回的是Mongoose自己封装的Query对象，使用find()可以在函数最后接上回调来获取查询到的数据。

使用链式语句时，可以在之后接.exec()执行查询，并指定回调函数。

```javascript
blogModel.find({title: "Mongoose", meta.votes: 100}, {title: 1, author: 1, body: 1}).exec(function(err, docs){
    if(err) console.log(err);
    console.log('查询结果：' + docs);
})
```

配合各种查询符可以方便的实现复杂的查询。

比如我需要查询“title”中以“Mongoose”开头，并且“meta”中“votes”的值小余100。并且按“meta”中“votes”的值升序排序。
```javascript
blogModel.and([
    { title: { $regex: "Mongoose.+","$options":"i"}},
    { meta.votes: { $lt: 100}}
).sort({ meta.votes: 1}
).exec(function(err, docs){
    if(err) console.log(err);
    console.log('查询结果：' + docs);
});
```
### 比较查询运算符

$equals 等于 ／ $gt 大于 ／ $gte 大于等于 ／ $lt 小余 ／ $lte 小余等于 ／ $ne 不等于 ／ $in 在数组中 ／ $nin 不在数组中

`blogModel.find({meta.votes: {$lt: 100}});`

查询“meta”中的“votes”字段值小余100的数据。

`blogModel.find({title: {$in: ['Mongoose', 'Mongodb', 'Nodejs']}});`

查询“title”为“Mongoose”或“Mongodb”或“Nodejs”其中之一的数据。

### 逻辑查询运算符

$or 或 ／ $and 与 ／ $nor 非
```javascript
blogModel.find({ $and: [
    {meta.votes: {$gte: 50}}, 
    {meta.votes: {$lt: 100}}
]});
```
查询“meta”中的“votes”字段值大于等于50到小余100的数据。
```javascript
blogModel.find({ $nor: [
    {meta.votes: 50}, 
    {meta.votes: 100}
]});
```
查询“meta”中的“votes”字段值不等于50和不等于100的数据。

以上例子也可以写成这样形式，比较清晰，其他类同
```javascript
blogModel.and([
    {meta.votes: {$gte: 50}}, 
    {meta.votes: {$lt: 100}}
]);

blogModel.nor([
    {meta.votes: 50}, 
    {meta.votes: 100}
]);
```
### 元素查询运算符

$exists　查询的字段值是否存在
```javascript
blogModel.find({ title: {$exists: true}});
blogModel.where('title').exists(true)；
```
查询存在“title”字段的数据。

### 评估查询运算符

> $mod　与数据进行取模运算筛选
```javascript
blogModel.find({ meta.votes: {$mod: [4, 0]}});
blogModel.where('meta.votes').$mod(4, 0);
```
查找“meta”中的“votes”字段值与4取模后，值为0的数据。

> $regex　使用正则表达式查询数据

`blogModel.find({ title: { $regex: "Mongoose.+","$options":"i"}});`

搜索以“Mongoose”开头的“title”字段，“options”中的“i”代表不区分大小写。

 $options参数与其余用法可以查看mongodb文档中 $regex 一节。

> $where　支持js表达式查询
```javascript
blogModel.find({ $where: 'this.comments.length === 10 || this.name.length === 5' });
blogModel.$where(function() { return this.comments.length === 10 || this.name.length === 5; });
```
### 数组查询运算符

> Query#all([path], val)　　查询数组的本身及超集

`blogModel.find( tags: ['nodejs', 'mongoose']);`

查询“tags”的字段值同时包含有['nodejs', 'mongoose']的数据。只要值中包含此数组即返回数据，若是只包含数组中的一个则不返回此数据。

> Query#elemMatch(path, criteria)　　查询数组的交集

`blogModel.find( $elemMatch: { tags: 'mongoose', author: 'L'});`

查询“tags”为“mongoose”或是“author”为“L”的数据。

> Query#size([path], val)　　查询指定大小的数组
```javascript
blogModel.find( tags: { $size: 2});
blogModel.where('tags').size(2);
```

查询“tags”数组中包含两个元素的数据。

### 其他常用的运算符

> Query#limit(val)　　限制查询返回的数量

`blogModel.find( tags: 'mongoose').limit(5);`

查询“tags”为“mongoose”的数据，只返回前5个查询结果。

> Query#skip(val)　　跳过前N个查询结果

`blogModel.find( tags: 'mongoose').skip(10).limit(5);`

查询“tags”为“mongoose”的数据，跳过前10个查询结果，返回从第11个开始的五个查询结果。
做分页时常用到这两个，但数据量过大时就会有性能问题。

> Query#sort(arg)　　对结果按某个指定字段进行排序

1、asc为升序，-1、desc为降序。可以对一个字段进行排序，也可以是多个。

`blogModel.find( tags: 'mongoose').skip(10).limit(5).sort("{ meta.votes: 1}");`

查询“tags”为“mongoose”的数据，跳过前10个查询结果，返回从第11个开始的五个查询结果。之后按“votes”进行升序排序。

> Query#count([criteria], [callback])　　计数

`blogModel.count({ title: 'mongoose'}, function(err, docs){});`

统计“title”为“mongoose”数据的数量

> Query#select(arg)　　选择指定字段

在查询中可以选择指定的查询字段，或者排除指定的字段。+为包含，-为排除。

`blogModel.select('title body');`

只包含“title”、“body”字段。

`blogModel.select('-title -body');`
排除“title”、“body”字段。
