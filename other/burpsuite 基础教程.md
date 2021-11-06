---
title: burpsuit基础教程
date: 2020-09-14 14:06
swiper: false # 将改文章放入轮播图中
swiperImg: '/medias/1.jpg' # 该文章在轮播图中的图片，可以是本地目录下图片也可以是http://xxx图片
img: 'http://cdn.mxrblog.cn/bing_2.jpg' # 该文章图片，可以是本地目录下图片也可以是http://xxx图片
categories: 工具
tags: burpsuit
top: false
---



# burpsuite 基础教程

1. 打开burpsuite

![image-20210627190201461](http://cdn.mxrblog.cn/image-20210627190201461.png)



2. 点击代理

![image-20210627190957219](http://cdn.mxrblog.cn/image-20210627190957219.png)



3. 关闭拦截器，这样可以让流量不被brupsite拦截，浏览器可以上网，intercept is off 即为关闭

![image-20210627191215458](http://cdn.mxrblog.cn/image-20210627191215458.png)



4. 点击option设置代理，并且在浏览器中设置代理与brupsuite保持一致

![image-20210627193613322](http://cdn.mxrblog.cn/image-20210627193613322.png)

5. 如果是使用的火狐浏览器，可能会因为浏览器本身要求https安全链接而让我们添加安全证书，此时我们需要添加安全证书

![1](http://cdn.mxrblog.cn/1.png)

- 从浏览器中打开127.0.0.1:8080

![2](http://cdn.mxrblog.cn/2.png)



![3](http://cdn.mxrblog.cn/3.png)



6. 任意点击网站内容后，进入brupsuite点击proxy下的HTTP history可以看见流量信息

![4](http://cdn.mxrblog.cn/4.png)



7. 修改流量信息

## 未完2待续

