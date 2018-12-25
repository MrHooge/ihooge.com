---
title: Ubuntu 14.04下配置 LAMP+phpMyAdmin PHP开发环境
date: 2016-06-07 11:20:58
tags: LAMP+phpMyAdmin
---

# Ubuntu 14.04下配置 LAMP+phpMyAdmin PHP开发环境

先安装 Apache Web服务器，终端：sudo apt-get install apache2  apache2-doc，然后测试是否安装成功。浏览器地址栏输入：http://localhost，回车，出现下图所示内容则代表成功！

<!-- more -->

接下来安装PHP5和Apache的php模块，终端：`sudo apt-get install php5 libapache2-mod-php5` 。重启Apache服务使php模块生效，终端：`sudo service apache2 restart`。测试php5是否安装成功，先编辑一个测试文件，终端：`sudo vim /var/www/html/phpinfo.php`，输入如下内容：

    ```
    <?php

    phpinfo();

    ?>
    ```



保存，然后再在浏览器地址栏输入：`http://localhost/phpinfo.php`！





安装MySql数据库，终端：`sudo apt-get install mysql-server mysql-client`，安装过程会提示设置密码，设置一个且记住！




再安装mysql的界面管理phpmyadmin，终端：`sudo apt-get install phpmyadmin`，安装过程会有提示选择web服务器，选中apache，回车，还会提示设置密码！phpmyadmin安装完后，并不在apache默认路径下，需要建立一个连接，终端：`sudo ln -s /usr/share/phpmyadmin /var/www/html`，重启apache服务器，浏览器打开：`http://localhost/phpmyadmin`，出现下图所示登录页面则表示安装成功！



输入刚才设置的密码，点击右下角'执行'就可登录！

登录后提示，mcrypt错误！终端执行：`sudo php5enmod mcrypt`，

然后重启apache，再次登录phpmyadmin正常！