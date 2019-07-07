---
title: mysql-8.0的一些简单的配置
date: 2018-07-28 16:28:11
tags: 
  - 后端
  - mysql
categories:
  - 数据库   
---

# 下载
下载后，将zip解压后放到一个目录下，配置好环境变量
# 配置文件
下载好后，下载的根目录没有 my.ini 文件（或my-default.ini），没有my.ini文件，没关系可以自行创建。在安装根目录下添加 my.ini
```ini
[mysqld]
# 设置3306端口
port=3306
# 设置mysql的安装目录
basedir=C:\Program Files\mysql-8.0.12-winx64
# 设置mysql数据库的数据的存放目录
datadir=D:\mysql\data
# 允许最大连接数
max_connections=200
# 允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统
max_connect_errors=10
# 服务端使用的字符集默认为UTF8
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
# 默认使用“mysql_native_password”插件认证
default_authentication_plugin=mysql_native_password
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
[client]
# 设置mysql客户端连接服务端时默认使用的端口
port=3306
default-character-set=utf8
```
> 注意，里面的 basedir 是我本地的安装目录，datadir 是我数据库数据文件要存放的位置，各项配置需要根据自己的环境进行配置。
# 初始化数据库
在MySQL安装目录的 bin 目录下执行命令：
```
mysqld --initialize --console

```
执行完成后，会打印 root 用户的初始默认密码
```
C:\Program Files\mysql-8.0.12-winx64\bin
λ mysqld --initialize --console
2018-07-28T08:09:39.819831Z 0 [System] [MY-013169] [Server] C:\Program Files\mysql-8.0.12-winx64\bin\mysqld.exe (mysqld 8.0.12) initializing of server in progress as process 8624
2018-07-28T08:09:46.120948Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: thbVf;1w7(Zy
2018-07-28T08:09:48.278535Z 0 [System] [MY-013170] [Server] C:\Program Files\mysql-8.0.12-winx64\bin\mysqld.exe (mysqld 8.0.12) initializing of server has completed
```
> thbVf;1w7(Zy就是初始密码（不含首位空格）

如果删掉初始化的 datadir 目录，再执行一遍初始化命令，又会重新生成的
# 安装服务
在MySQL安装目录的 bin目录下执行命令（以管理员身份打开cmd命令行）
```
mysqld --install [服务名]

```
后面的服务名可以不写，默认的名字为 mysql。当然，如果你的电脑上需要安装多个MySQL服务，就可以用不同的名字区分了，比如 mysql5 和 mysql8。
> 安装完成之后，就可以通过命令net start mysql启动MySQL的服务了。
```
C:\Program Files\mysql-8.0.12-winx64\bin
λ mysqld --install
Service successfully installed.

C:\Program Files\mysql-8.0.12-winx64\bin
λ net start mysql
MySQL 服务正在启动 .
MySQL 服务已经启动成功。
```
# 更改密码
在MySQL安装目录的 bin 目录下执行命令：
mysql -u root -p

　　这时候会提示输入密码，刚刚的初始密码
进去后：修改密码如下代码
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '新密码';
```
结果
```
C:\Program Files\mysql-8.0.12-winx64\bin
λ mysql -u root -p
Enter password: ************
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.12

Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';
Query OK, 0 rows affected (0.08 sec)

mysql> exit
```