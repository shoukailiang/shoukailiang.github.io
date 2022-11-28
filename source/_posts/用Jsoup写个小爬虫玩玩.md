---
title: 用Jsoup写个小爬虫玩玩
date: 2021-02-11 00:33:01
tags: 
  - jsonp
  - 爬虫
categories:
  - 爬虫
---

# 初始化
> 获取请求返回的页面信息，筛选出我们想要的数据就可以了
初始化项目
![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202211281709577.png)
导入依赖
![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202211281709494.png)
# 代码编写
## 编写一个封装对象的实体类
```java
package com.shoukailiang;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor
@NoArgsConstructor
public class Content {

    private String img;
    private String title;
    private String price;
}

```
## 爬取数据
爬取京东页面，可以看出京东keyword可以查询数据
![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202211281710354.png)
![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202211281710096.png)
先找到id为J_goodsList的div，然后遍历下面的每个li
![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202211281710378.png)
```java
Element element = document.getElementById("J_goodsList");
System.out.println(element);
// 获取所有的li元素
Elements elements = element.getElementsByTag("li");
```
![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202211281711644.png)
```java
ArrayList<Content> goodList = new ArrayList<>();

// 获取元素中的内容
for (Element el : elements) {
    // 如果通过src获取不到图片的话，有可能所有图片都是延迟加载的！
    // 我们attr的属性可能需要通过data-lazy-img去获取
    String img = el.getElementsByTag("img").eq(0).attr("src");
    String price = el.getElementsByClass("p-price").eq(0).text();
    String title = el.getElementsByClass("p-name").eq(0).text();

    Content content = new Content();
    content.setImg(img);
    content.setTitle(title);
    content.setPrice(price);
    goodList.add(content);
}
```
# 完整代码
```java
package com.shoukailiang;

import org.jsoup.Jsoup;
import org.jsoup.nodes.Document;
import org.jsoup.nodes.Element;
import org.jsoup.select.Elements;
import java.io.IOException;
import java.net.URL;
import java.util.ArrayList;
import java.util.List;

public class HtmlParseUtil {

    public List<Content> parseJD(String keywords) throws IOException {
        // 获得请求 https://search.jd.com/Search?keyword=java
        String url = "https://search.jd.com/Search?keyword=" + keywords;
        // 解析网页 Jsoup返回Document就是浏览器的Document对象
        Document document = Jsoup.parse(new URL(url), 30000);
        // 所有在js中可以使用的方法，这里都可以用
        Element element = document.getElementById("J_goodsList");
        System.out.println(element);
        // 获取所有的li元素
        Elements elements = element.getElementsByTag("li");

        ArrayList<Content> goodList = new ArrayList<>();

        // 获取元素中的内容
        for (Element el : elements) {
            // 如果通过src获取不到图片的话，有可能所有图片都是延迟加载的！
            // 我们attr的属性可能需要通过data-lazy-img去获取
            String img = el.getElementsByTag("img").eq(0).attr("src");
            String price = el.getElementsByClass("p-price").eq(0).text();
            String title = el.getElementsByClass("p-name").eq(0).text();

            Content content = new Content();
            content.setImg(img);
            content.setTitle(title);
            content.setPrice(price);
            goodList.add(content);
        }
        return goodList;
    }

    public static void main(String[] args) throws IOException {
        new HtmlParseUtil().parseJD("显示器").forEach(System.out::println);
    }
}

```
![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202211281711538.png)