
---
title: node.js下邮箱验证
date: 2016-08-16 13:41:58
tags: NodeJs
---

最近用node.js做注册邮箱验证时,用到urlBase64位字符串:


实现思路:  
1、数据库表中添加一个激活的状态字段0或1 (0为未激活，1未激活)，在添加一个验证码字段validateCode，最后在添加一个注册时间字段addTime  
2、程序中，注册页面添加注册信息，随机生成注册验证码添加到数据库中，越复杂越好，然后对验证码进行加密，把用户id或者UserName和加过密的验证码作为参数发送到邮箱中(有的时候只把验证码发送到邮箱，然后找到有没有匹配的，然后修改状态，我感觉这样不太好，防止有相同的验证码出现，所以最好在加一个参数)  
3、发送到邮箱里以后，用户可以点击进行确认，这里有时间限制，比如48小时之内未能通过注册，则失效，只能重新注册，激活链接只能使用一次，一次后也将失效  
4、处理页面中，首先验证链接是否过期，将注册的时间与当前的时间作比较，如果超过时间则提示验证码过期，重新注册或者重新发送验证码，然后再判断链接是否用过，只能使用一次，这个只要判断数据库中的验证码是否为空即可，验证都通过以后，根据id或userName从数据库中取回验证码与链接中的验证码作比较，通过了，修改状态为1，即启用，然后将注册码清空，转到登陆或者首页，否则提示验证失败！  
5 ,为了防注册机注入，我们要判断邮箱的唯一性，要不然他们会伪造一个邮箱激活  
``` javascript 
    //首先利用node.js中的crypto将用户id进行加密(需要将加密后的值存入数据库)  
    var sha=crypto.createHash('sha512');//可以用console.log(crypto.getHashes());查看支持哪些加密方式  
    var password=sha.update(userid).digest("base64");//将userid加密  
   //由于加密后的值含有特殊字符,不能直接有做为url在网络中传输,故需要将其转换(这里需要我用的是:urlsafe-base64 npm)  
   //编码后的值可以放入url中,在服务器用邮件发出去  
    var randomURLSafeBase64 = URLSafeBase64.encode(new Buffer(password,'base64'));//需要先安装:npm urlsafe-base64  
   //当用户点击链接时,将取到的base64字符串解码,并与数据库中的值比对  
    var decodeName= URLSafeBase64.decode(randomURLSafeBase64);//编码  
    console.log("decodeName:"+decodeName.toString("base64"));//解码  
```

<!-- more-->
已上链接：http://blog.csdn.net/pz0605/article/details/47611201

下面是用 *nodemailer* 做的一个测试
Example
```javascript
var nodemailer = require('nodemailer');

// create reusable transporter object using the default SMTP transport
var transporter = nodemailer.createTransport('smtps://user%40gmail.com:pass@smtp.gmail.com');

// setup e-mail data with unicode symbols
var mailOptions = {
    from: '"Fred Foo 👥" <foo@blurdybloop.com>', // sender address
    to: 'bar@blurdybloop.com, baz@blurdybloop.com', // list of receivers
    subject: 'Hello ✔', // Subject line
    text: 'Hello world 🐴', // plaintext body
    html: '<b>Hello world 🐴</b>' // html body
};

// send mail with defined transport object
transporter.sendMail(mailOptions, function(error, info){
    if(error){
        return console.log(error);
    }
    console.log('Message sent: ' + info.response);
});

```

##install
```
npm install nodemailer
```
To send e-mails you need a transporter object
```
var transporter = nodemailer.createTransport(transport[, defaults])
```

##Examples
```
var smtpConfig = {
    host: 'smtp.gmail.com',
    port: 465,
    secure: true, // use SSL
    auth: {
        user: 'user@gmail.com',
        pass: 'pass'
    }
};

var poolConfig = {
    pool: true,
    host: 'smtp.gmail.com',
    port: 465,
    secure: true, // use SSL
    auth: {
        user: 'user@gmail.com',
        pass: 'pass'
    }
};

var directConfig = {
    name: 'hostname' // must be the same that can be reverse resolved by DNS for your IP
};
```

具体参考链接：https://github.com/nodemailer/nodemailer

个人测试代码
```javascript
var nodemailer  = require("nodemailer");
var user = 'xxxx@163.com'
  , pass = 'your_email_passwd' //
  ;
var smtpTransport = nodemailer.createTransport( {
      service: "163"
    , auth: {
        user: user,
        pass: pass
    }
  });

smtpTransport.sendMail({
    from    : 'Elliott<' + user + '>'
  , to      : '<yunfei6558@qq.com>'
  , subject : 'Node.JS通过SMTP协议从QQ邮箱发送邮件'
  , html    : '这是一封测试邮件 <br> '
}, function(err, res) {
    console.log(err, res);
});
```

个人测试成功