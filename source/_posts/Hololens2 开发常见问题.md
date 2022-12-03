---
title: Hololens2 开发常见问题
date: 2022-05-09 09:59:26
tags: 
  - unity
  - hololens2
categories:
 - hololens2
---

# 常见错误
## 1 未能使用 通用身份验证.....
![image.png](https://img-blog.csdnimg.cn/img_convert/1aca0d2ca0fa80215d3040db01bf26e8.png)
### 解决办法 (大概率是usb连接未装)

![image.png](https://img-blog.csdnimg.cn/img_convert/e9fe54810bab322d1b7d6b758f122f2a.png)
## 2 缺少pdb
![image.png](https://img-blog.csdnimg.cn/img_convert/ce7f02749d5c640755fb0e0a28995f2b.png)
### 解决办法
..... 至今不知道
## 3 模型位置显示不正确的问题
- 1、Vuforia识别图上传时必须与实际打印的识别图的size一致。比如实际尺寸是10cm，那么上传时的width应该设置为0.1（Unity以米为单位）。
- 2、ARcamera 的 position 必须设置为（0，0，0）。
其他的关于模型不显示的问题，要么是显示了但不在识别码上，所以你看不到。要么就是你发布到Hololens的流程有问题，可能是某个选项忘记勾选了。
## 4 模型出现后，不消失
做此处的修改，此处为ImageTarget中自带的脚本
![image.png](https://img-blog.csdnimg.cn/img_convert/ec3735a097b9f2348c2712d936e328e2.png)
## 5 部署的时候出现:找不到 SDK“WindowsMobile, Version=10.0.18362.0”。	

![image.png](https://img-blog.csdnimg.cn/img_convert/5d027cc7d783086b44c19203a05ce533.png)

![image.png](https://img-blog.csdnimg.cn/img_convert/272c6141d1ddabc0f4cbc88929d9ad6f.png)
## 6 打包后只出现左眼的问题
在使用Hololens 2显示自己编写的Shader时，上传会出现只有左眼能看到，但是右眼看不到的问题。

![image.png](https://img-blog.csdnimg.cn/img_convert/063afb276030b5b9d7c94a225bdc1299.png)
## 7.openxr中hololens灰色

![image.png](https://img-blog.csdnimg.cn/img_convert/8d6bbba05d76f08d1ebe439a7a151ca8.png)

重新装一下openxr ,大概率直接没装或者一不小心被你删除了

![image.png](https://img-blog.csdnimg.cn/img_convert/f844e21513b0260e33f70f73c5042371.png)
## 8 激活 Windows 应用商店应用“Template3D_pzq3xp76mxafg!App”失败，错误为“拒绝访问”
hololens 没解锁，息屏了

## 9 读取本地文件失败
从我目前了解的情况看，貌似只支持在hololens2那几个文件夹中读取

![2adbf16298a6c3ba4d5ed56877b2f9b.png](https://img-blog.csdnimg.cn/img_convert/f0aca104e35036f06cea73cdcb124af0.png)
打勾权限

![cbe3ffdc37f021d5e5a53d29019fee3.png](https://img-blog.csdnimg.cn/img_convert/94247777b2b42f0ac6c4a458685ad6fe.png)

## json解析
使用simplejson
