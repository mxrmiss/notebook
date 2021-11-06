---
title: Sense
date: 2021-10-19
swiper: false # 将改文章放入轮播图中
swiperImg: '/medias/1.jpg' # 该文章在轮播图中的图片，可以是本地目录下图片也可以是http://xxx图片
img: 'http://cdn.mxrblog.cn/ct_23.jpg' # 该文章图片，可以是本地目录下图片也可以是http://xxx图片
categories: walkthrough
tags: [渗透测试, 靶机]
top: false
---



# Sense

- **Sense是hackthebox上的一台简单的靶机**



- nmap查看端口号以及漏洞
- nmap未给出能够有效利用的漏洞

![image-20211019234038347](http://cdn.mxrblog.cn/image-20211019234038347.png)

- 分别用浏览器打开http://10.10.10.60:80 和 https://10.10.10.60:443
- 都只显示这一个页面，使用gobuster分别爆破目录

![image-20211020151931184](http://cdn.mxrblog.cn/image-20211020151931184.png)

- 使用gobuster扫描，在https://10.10.10.60下发现了一些东西

![image-20211020012847413](http://cdn.mxrblog.cn/image-20211020012847413.png)

- 再使用gobuster扫描https://10.10.10.60
- 注意使用的字典，如果字典不好可能无法达到应有的目的

```
gobuster -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt dir -u https://10.10.10.60/ -x php,txt,cnf,conf -k -t 200 -o output1.txt
```

![image-20211029155037654](http://cdn.mxrblog.cn/image-20211029155037654.png)



- 发现一个changelog.txt

![image-20211022201433563](http://cdn.mxrblog.cn/image-20211022201433563.png)

- 再查看system-users.txt

  ![image-20211029155625153](C:\Users\herol\AppData\Roaming\Typora\typora-user-images\image-20211029155625153.png)

- 使用给的用户名和密码进行登录，用户名是rohit，密码是默认密码，经过google，默认密码是pfsense

![image-20211029160451324](http://cdn.mxrblog.cn/image-20211029160451324.png)

![image-20211029161557831](http://cdn.mxrblog.cn/image-20211029161557831.png)

- 检查有什么可以利用的漏洞

![image-20211029162419760](http://cdn.mxrblog.cn/image-20211029162419760.png)

- 使用上面给的python脚本

![image-20211029162959965](http://cdn.mxrblog.cn/image-20211029162959965.png)

- kali上面开启nc端口监听

![image-20211029163029925](http://cdn.mxrblog.cn/image-20211029163029925.png)

- 发现自己已经拥有了root用户权限
- 顺利获得俩个哈希值

![image-20211029163306301](http://cdn.mxrblog.cn/image-20211029163306301.png)

