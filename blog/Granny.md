---
title: Hackthebox::Granny walkthrough
date: 2021/12/14
categories: 渗透测试
tags: [渗透测试,walkthrough]
index_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_21.jpg
banner_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_21.jpg
---



- ***这是一台hackthebox的简单的windows靶机***

#  信息收集

- 首先使用nmap进行端口扫描

![image-20211226003047452](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211226003047452.png)

- 使用nmap进行粗略的漏洞扫描，发现80端口是可以利用的

![image-20211226005753161](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211226005753161.png)

- 查看80端口在web上的内容

![image-20211226003317178](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211226003317178.png)

- 查看80端口服务 `Microsoft IIS httpd 6.0` 是否有什么漏洞

![image-20211226005854653](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211226005854653.png)

- 使用msf查看

![image-20211226010242981](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211226010242981.png)

- 使用第二个模块`windows/iis/iis_webdav_scstoragepathfromurl`进行并成功获得一个shell

![image-20211226010656885](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211226010656885.png)

# 提权

- 我们需要提权，返回msf中使用`post/multi/recon/local_exploit_suggester`查看是否存在漏洞利用

![image-20211226013533089](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211226013533089.png)

![image-20211226014221073](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211226014221073.png)

- 使用这个渗透模块`exploit/windows/local/ms14_070_tcpip_ioctl`
- 注意设置payload与第一个会话中的一致，否则会报错

![image-20211226015918161](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211226015918161.png)

- 当我们执行run后发现出现了错误

![image-20211226023721860](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211226023721860.png)

- 我们需要回到会话中然后迁移进程

![image-20211226023838691](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211226023838691.png)

- 再次挂起会话，使用刚才那个没有成功的模块，这次成功了

![image-20211226024116445](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211226024116445.png)

![image-20211226024224306](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211226024224306.png)

- 获得flag

![image-20211226025101345](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211226025101345.png)

