---
title: photon pun2 设置中国区
date: 2022-05-09 00:33:19
tags: 
  - unity
  - pun2
categories:
 - 网络
---


# photon 网址
> https://dashboard.photonengine.com/zh-CN

先创建一个app，这里的appid等等会用到
# 中国区
> https://vibrantlink.com/

# 步骤
访问中国区官网
![在这里插入图片描述](https://img-blog.csdnimg.cn/b9f969e416a64e9cba78ddc29a1ff3e4.png)
填写信息，填写之前复制的appid，一般两个工作日
![在这里插入图片描述](https://img-blog.csdnimg.cn/2862745f18c14acab7f8e67e916e1ffb.png)
从unity商店中导入pun2
![在这里插入图片描述](https://img-blog.csdnimg.cn/4270a71d9e474ca7b277016cece51170.png)

# 修改信息
填写appid（邮件回复的id）
![在这里插入图片描述](https://img-blog.csdnimg.cn/2119ceeb2a5b4543a3b5ab276173e99b.png)
搜索LoadBalancingClient.cs
![在这里插入图片描述](https://img-blog.csdnimg.cn/5ba4e80917e543c5b05321a085cbdb56.png)

修改 NameServerHost， 由 ns.exitgames.io 改为 ns.photonengine.cn;
![在这里插入图片描述](https://img-blog.csdnimg.cn/e27dc1c9705d45d9a34acd2760142746.png)
 PhotonServerSettings 的 Fixed Region 设置为 cn;
![在这里插入图片描述](https://img-blog.csdnimg.cn/9242ecd02fd24b2a818ab96e4bfff7b5.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/43e2789663054234aafb4b814f22cdff.png)


# 测试
可以将配置文件的日志打开，载入自带的demo
![在这里插入图片描述](https://img-blog.csdnimg.cn/000c74a18cd846509213834b7a7672e4.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/3d791f6937474fc892adc2487cad7dca.png)

