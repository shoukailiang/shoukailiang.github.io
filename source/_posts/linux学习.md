---
title: linux学习
date: 2019-06-01 22:36:18
tags: 
  - ubuntu
  - deepin
categories:
 - linux
---
记录一些我曾经遇到过的问题--------------勿看
# ubuntu
### ubuntu 的软件安装
- 1 deb 包的安装方式
deb 是 debian 系 Linux 的包管理方式, ubuntu 是属于 debian 系的 Linux 发行版,所
以默认支持这种软件安装方式,当下载到一个 deb 格式的软件后,在终端输入这
个命令就能安装:
sudo dpkg -i *.deb

- 2编译安装方式
(小贴士:使用编译安装前,需要先建立编译环境,使用以下命令建立基本的编译环境:
sudo apt-get install build-essential)
在 linux 的世界,有很多软件只提供了源代码给你,需要你自己进行编译安装,一般开源
的软件都会使用 tar.gz 压缩档来进行发布,当然也有其他的形式。拿到源代码的压缩文档
把它解压到/tmp 目录下,进入/tmp/软件目录,然后执行以下三个命令:
1 ./configure
2 make
3 sudo make install
在第一步 ./configure 时可能会提示说有某某软件找不到,例如提示“ libgnome”这个开
发包找不到,那就把 libgnome 这个关键词 copy,然后打开新立得软件管理器,在里面
搜索 libgnome 这个关键词,就会找到 libgnome 相关的项目,把前面有个 ubuntu 符号
的 libgnome 包(注意:同样需要安装 dev 包,但可以不装 doc 包)全部安装,通过这个方
法把./configure 过程中缺失的开发包都全部装上就 OK 了,第一步能顺利通过,第二 ,三
步基本问题不大。
以上就是一般初学 ubuntu 的朋友必须掌握的编译安装的基本方法!

- 3。 apt-get 安装方法
ubuntu 世界有许多软件源,在系统安装篇已经介绍过如何添加源, apt-get 的基本软件
安装命令是:
sudo apt-get install 软件名

- 4。新立得软件包管理
打开:系统–系统管理–新立得软件包管理,这个工具其实跟 apt 一样,可以搜索,下
载,安装 ubuntu 源里的软件,具体安装方式很简单,看着界面应该会懂,就不详细介绍
了
> 在使用 dpkg -i 安装deb包后，会出现依赖关系而不能正常安装软件，这个时候先更新下源然后解决依赖关系后重装即可。
```
sudo apt-get update # 更新
sudo apt-get -f install # 解决依赖关系
sudo dpkg -i xxx.deb # 重新安装
```

#### 安装搜狗
http://blog.csdn.net/qq_21792169/article/details/53152700

#### 配置用户变量
- http://blog.csdn.net/qiao1245/article/details/44650929
- https://www.cnblogs.com/imayi/p/6082122.html
- https://blog.csdn.net/white_idiot/article/details/78253004
- https://blog.csdn.net/gaoanchen/article/details/77692451


#### 主题美化
https://zhuanlan.zhihu.com/p/36200924
#### 查看分区信息：

sudo fdisk -l
#### Ubuntu(Linux)开机自动挂载磁盘
https://jingyan.baidu.com/article/63acb44a27686961fcc17ec9.html

- sudo gedit /etc/fstab 
#### Ubuntu如何添加删除PPA
- https://blog.csdn.net/li_hai/article/details/8189290
- 删除一个PPA源

  - 1,到 源的 目 录:cd  /etc/apt/sources.list.d/

  - 2,可以看 到 关 于 源的 文件,删除即可 .
  - 
#### 显示网速
- https://github.com/fossfreedom/indicator-sysmonitor


### 错误：error while loading shared libraries: libcurl.so.4: cannot open shared object file: No such file or directory
- sudo apt-get install curl libcurl3


# centos7 
## 修改时区
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

查看时间 date

