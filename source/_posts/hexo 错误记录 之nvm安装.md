---
title: hexo 错误记录 之nvm安装
date: 2021-02-03 00:33:51
tags: 
  - hexo
  - nvm
categories:
  - 前端
---

hexo d 的时候报了这个错。查了几篇文章，发现是node 版本太高了,使用nvm更换为12.14.0可以正常上传
![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202211281645973.png)
```
TypeError [ERR_INVALID_ARG_TYPE]: The "mode" argument must be integer. Received an instance of Object
    at copyFile (fs.js:1972:10)
```
# nvm控制版本
通过将多个node 版本安装在指定路径，然后通过 nvm 命令切换时，就会切换我们环境变量中 node 命令指定的实际执行的软件路径。
windows系统下的nvm 安装

**下载**

链接：[https://github.com/coreybutler/nvm-windows/releases](https://github.com/coreybutler/nvm-windows/releases)

之后下载 nvm-setup.zip，无脑下一步

**检查是否安装成功**

打开CMD，输入nvm，安装成功则会如下图所示，它会显示出当前nvm版本以及nvm的命令：

![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202211281645930.png)
修改settings.txt

在你安装的目录下找到settings.txt文件，打开后加上 

node_mirror: https://npm.taobao.org/mirrors/node/ 

npm_mirror: https://npm.taobao.org/mirrors/npm/
![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202211281646934.png)
**基本命令**
```
nvm arch [32|64]： 显示node是运行在32位还是64位模式。指定32或64来覆盖默认体系结构。 
-nvm install <version> [arch]：该可以是node.js版本或最新稳定版本latest。（可选[arch]）指定安装32位或64位版本（默认为系统arch）。设置[arch]为all以安装32和64位版本。在命令后面添加–insecure，可以绕过远端下载服务器的SSL验证。
nvm list [available]：列出已经安装的node.js版本。可选的available，显示可下载版本的部分列表。这个命令可以简写为nvm ls [available]。
nvm on： 启用node.js版本管理。
nvm off： 禁用node.js版本管理(不卸载任何东西)
nvm proxy [url]： 设置用于下载的代理。留[url]空白，以查看当前的代理。设置[url]为none删除代理。
nvm node_mirror [url]：设置node镜像，默认为https://nodejs.org/dist/.。可以设置为淘宝的镜像https://npm.taobao.org/mirrors/node/
nvm npm_mirror [url]：设置npm镜像，默认为https://github.com/npm/npm/archive/。可以设置为淘宝的镜像https://npm.taobao.org/mirrors/npm/
nvm uninstall <version>： 卸载指定版本的nodejs。
nvm use [version] [arch]： 切换到使用指定的nodejs版本。可以指定32/64位[arch]。 
-nvm use <arch>：将继续使用所选版本，但根据提供的值切换到32/64位模式
nvm root [path]： 设置 nvm 存储node.js不同版本的目录 ,如果未设置，将使用当前目录。 
-nvm version： 显示当前运行的nvm版本，可以简写为nvm v
```