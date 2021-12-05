---
title: Lame
date: 2021-10-14
swiper: false # 将改文章放入轮播图中
swiperImg: '/medias/1.jpg' # 该文章在轮播图中的图片，可以是本地目录下图片也可以是http://xxx图片
img: 'http://cdn.mxrblog.cn/ct_16.jpg' # 该文章图片，可以是本地目录下图片也可以是http://xxx图片
categories: walkthrough
tags: [渗透测试, 靶机]
top: false
---



# Lame

- **hackthebox的一个基础机器，接下来我将使用msf和其他方式对这台机器进行渗透**

### 信息收集

- 首先进行端口扫描以及初步的漏洞扫描

![image-20211010002057077](http://cdn.mxrblog.cn/image-20211010002057077.png)

- 基础信息总结如下：

```
21/tcp  open  ftp         vsftpd 2.3.4
22/tcp  open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.0.20-Debian (workgroup: WORKGROUP)
```

- 通过searchsploit发现可能有两个地方有漏洞，分别是vsftpd 2.3.4和Samba  3.0.20

![image-20211010002846205](http://cdn.mxrblog.cn/image-20211010002846205.png)

### 漏洞利用



- 分别使用msf查看是否可行

![image-20211010004537720](http://cdn.mxrblog.cn/image-20211010004537720.png)

- 发现只有Samba  3.0.20的漏洞才能使用，另一个无法使用，据网上搜索结果说是过期了，并且我们得到shell后会发现我们已经是root用户了

### 寻找flag

- root.txt

![image-20211010005204779](http://cdn.mxrblog.cn/image-20211010005204779.png)



- user.txt

![image-20211010005408791](http://cdn.mxrblog.cn/image-20211010005408791.png)



### 使用python脚本进行渗透测试

- **有很多情况下是我们无法使用msf就能直接能得到结果的，这个时候就需要我们能够找到漏洞，并且在不借用msf的情况下解决问题**

- 之前我们已经发现了只有Samba  3.0.20的漏洞才能使用，接下来将使用该漏洞的python脚本来渗透靶机

![image-20211010012609412](http://cdn.mxrblog.cn/image-20211010012609412.png)

![image-20211010012635694](http://cdn.mxrblog.cn/image-20211010012635694.png)

- python脚本地址

```
sudo apt install python python-pip
pip install --user pysmb
git clone https://github.com/amriunix/CVE-2007-2447.git

```

