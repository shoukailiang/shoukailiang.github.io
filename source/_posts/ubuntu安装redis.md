---
title: ubuntu安装redis
date: 2019-07-08 14:22:34
tags: 
  - ubuntu
  - redis
categories:
 - linux
---
# 下载
```shell
wget http://download.redis.io/releases/redis-5.0.5.tar.gz
tar xzf redis-5.0.5.tar.gz
cd redis-5.0.5
```

# 编译
```
make
```
如果Command 'make' not found, but can be installed with:有这个错误提示,安装下列依赖
```

sudo apt-get install build-essential
```
make完后 edis-5.0.5目录下会出现编译后的redis服务程序redis-server,还有用于测试的客户端程序redis-cli,两个程序位于安装目录 src 目录下：


# 安装到指定目录去
```
sudo make install PREFIX=/usr/local/redis 
```
# 默认启动
下面启动redis服务.
```
cd bin
./redis-server
```
注意这种方式启动redis。并且是前台启动，也就是说启动后，命令界面就不干别的事情了。我们需要的是后台启动，就需要通过启动参数告诉redis使用指定配置文件。
# 修改配置文件
 先回到redis解压后的目录去,拷贝一份配置文件到安装目录
```
sudo cp redis.conf /usr/local/redis/
cd /user/local/redis
sudo vi redis.conf
```
修改第一处，bind 127.0.0.1(只限制了本地访问我们需要远程)改为
```
bind 0.0.0.0
```
修改第二处 daemonize no （前台启动方式）改为yes后台启动
```
daemonize yes
```
修改第三处 # requirepass foobared 密码（打开注释）
```
 requirepass 123456
```
还可以指定日志文件,默认为空
```
logfile /var/log/redis/redis-server.log
```
# 指定配置文件启动
```
cd bin
./redis-server ../redis-conf 
```
# 链接
```
 ./redis-cli -p 6379 -a 123456
```
之后就可以在命令行中插入数据了
# 常见错误
启动几分钟后，无法连接redis,并且有以下错误信息
```
MISCONF Redis is configured to save RDB snapshots, but is currently not able to persist on disk. Commands that may modify the data set are disabled. Please check Redis logs for details about the error．
```
在/etc/sysctl.conf 添加一项 ‘vm.overcommit_memory = 1’ ，然后重启（或者运行命令’sysctl vm.overcommit_memory=1’ ）使其生效