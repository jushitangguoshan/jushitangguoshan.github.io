---
layout: mysql8.x安装后
title: MYSQL8.X安装后 phpMyAdmin无法登陆解决办法
date: 2020-03-22 16:55:01
tags: [MySql, Linux, PhpMyAdmin]
categories: [MySql, PhpMyAdmin]
keywords: docker
description: "&emsp;&emsp;MySql-升级之路，适用于 CentOS 系统"
---
#### 1、因为某些原因安装了8.0以后phpMyAdmin始终无法登陆，比如报错（服务器请求的客户端未知的身份验证方法。）:

```mysql
SQLSTATE [HY000] [2054]
```

#### 2、原因：

​		在MySQL 8.0+中，默认身份验证插件已从'mysql_native_password'更改为'caching_sha2_password'，'root'@'localhost'管理帐户默认使用’caching_sha2_password’身份验证插件。。如果您希望root帐户使用之前的默认身份验证插件’mysql_native_password‘，PHP的PDO_MySQL和ext / mysqli扩展不支持caching_sha2_password。另外，当PHP 7.1.4之前的PHP版本和PHP 7.2之前的版本使用PHP MySQL扩展时，即使未使用caching_sha2_password，也无法与default_authentication_plugin = caching_sha2_password连接。要恢复MySQL 8.X之前的兼容性，您可以重新配置服务器以恢复到以前的默认身份验证插件mysql_native_pass，

#### 3、所以在my.ini找到

```shell
default_authentication_plugin=caching_sha2_password
```

#### 4、改为或者加入下面一行代码：

```shell
default_authentication_plugin=mysql_native_password 
```

#### 5、如果你用的最新版

MYSQL8.0.13和phpmyadmin4.8.3重启，发现不能登陆。是因为密码已经是sha2方式保存的，所以php以原来的方式验证肯定无法通过，所以要修改一下密码。如果不能请做如下操作：
```
use mysql； ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '你的密码'; FLUSH PRIVILEGES;      
```
