---
title: docker安装mysql，redis
date: 2021-08-03 09:36:40
tags: 
  - mysql
  - redis
  - docker
categories:
 - 运维
---
> 以centos7为例
# docker的安装
```shell
#1.卸载旧版本
 yum remove docker \
>                   docker-client \
>                   docker-client-latest \
>                   docker-common \
>                   docker-latest \
>                   docker-latest-logrotate \
>                   docker-logrotate \
>                   docker-engine

#2.需要的安装包
yum install -y yum-utils

#3.设置镜像的仓库
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
#上述方法默认是从国外的，不推荐

#推荐使用国内的
yum-config-manager \
    --add-repo \
    https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
  
#更新软件包索引
yum makecache fast

#4.安装docker docker-ce 社区版 而ee是企业版
yum install docker-ce docker-ce-cli containerd.io # 这里我们使用社区版即可

#5.启动docker
systemctl start docker

#6.使用docker version 查看是否安装成功
docker version

#7.测试
docker run hello-world
```

# mysql8的安装
```shell
docker search mysql
docker pull mysql
docker images
cd /opt
mkdir mysql_docker
cd mysql_docker
echo $PWD
```

![image.png](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202211282232851.png)

> 运行
```
docker run --name mysqlserver -v $PWD/conf:/etc/mysql/conf.d -v $PWD/logs:/logs -v $PWD/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -d -i -p 3306:3306 mysql:latest
```

![image.png](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202211282232479.png)

![image.png](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202211282232582.png)
```shell
docker exec -it mysqlserver bash
```

![image.png](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202211282233033.png)
## 开启远程访问
```shell
use mysql;

select host,user from user;

ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';

flush privileges;
```

![image.png](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202211282233302.png)


# mysql 5.7
```
docker pull mysql:5.7
```
创建实例并启动
```
# --name指定容器名字 -v目录挂载 -p指定端口映射  -e设置mysql参数 -d后台运行
sudo docker run -p 3306:3306 --name mysql \
-v /mydata/mysql/log:/var/log/mysql \
-v /mydata/mysql/data:/var/lib/mysql \
-v /mydata/mysql/conf:/etc/mysql \
-e MYSQL_ROOT_PASSWORD=123456 \
-d mysql:5.7
####
-v 将对应文件挂载到主机
-e 初始化对应
-p 容器端口映射到主机的端口
```
MySQL 配置
```
vi /mydata/mysql/conf/my.cnf 创建&修改该文件
```
```
[client]
default-character-set=utf8
[mysql]
default-character-set=utf8
[mysqld]
init_connect='SET collation_connection = utf8_unicode_ci'
init_connect='SET NAMES utf8'
character-set-server=utf8
collation-server=utf8_unicode_ci
skip-character-set-client-handshake
skip-name-resolve
```

![image.png](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202211282233544.png)

![image.png](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202211282234220.png)
```
docker restart mysql
```


# redis
下载镜像文件
docker pull redis
docker pull redis:6.0
创建实例并启动
 ```
mkdir -p /mydata/redis/conf
touch /mydata/redis/conf/redis.conf
```

启动 同时 映射到对应文件夹,后面 \ 代表换行
```
docker run -p 6379:6379 --name redis \
-v /mydata/redis/data:/data \
-v /mydata/redis/conf/redis.conf:/etc/redis/redis.conf \
-d redis redis-server /etc/redis/redis.conf
```

> 6.0
```
docker run -p 6379:6379 --name redis6.0 \
-v /mydata/redis/data:/data \
-v /mydata/redis/conf/redis.conf:/etc/redis/redis.conf \
-d redis:6.0 redis-server /etc/redis/redis.conf
```

```
使用 redis 镜像执行 redis-cli 命令连接
docker exec -it redis redis-cli
```
持久化 默认 appendonly on 没有开启
```
vim /mydata/redis/conf/redis.conf
```
插入下面内容
```
appendonly yes
```


![image.png](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202211282234311.png)