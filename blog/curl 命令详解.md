---
title: curl 命令详解
date: 2022/1/13
categories: linux
tags: [linux, tool]
index_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_36.jpg
banner_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_36.jpg
---



> **转载于**：[Linux curl 命令详解 - 云+社区 - 腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1718259)

# 1. 命令概要

该命令设计用于在没有用户交互的情况下工作。

curl 是一个工具，用于传输来自服务器或者到服务器的数据。「向服务器传输数据或者获取来自服务器的数据」

可支持的协议有（DICT、FILE、FTP、FTPS、GOPHER、HTTP、HTTPS、IMAP、IMAPS、LDAP、LDAPS、POP3、POP3S、RTMP、RTSP、SCP、SFTP、SMTP、SMTPS、TELNET和TFTP）。

curl提供了大量有用的技巧，比如代理支持、用户身份验证、FTP上传、HTTP post、SSL连接、cookie、文件断点续传、Metalink等等。正如你将在下面看到的，这些特性的数量会让您头晕目眩！

# 2. 访问的URL

你可以在命令行上指定任意数量的url。它们将按指定的顺序依次获取。

你可以指定多个url，或url的部分通过在花括号内编写部分集，如：

```
1 http://site.{one,two,three}.com
2 # 参见
3 curl http://www.zhangblog.com/2019/06/16/hexo{04,05,06}/ \-I  # 查看信息
```

或者可以使用\[\]得到字母数字序列的序列，如：

```
1 ftp://ftp.numericals.com/file\[1\-100\].txt
2 ftp://ftp.numericals.com/file\[001\-100\].txt   # 前导用零
3 ftp://ftp.letters.com/file\[a\-z\].txt 
4 # 参见
5 curl http://www.zhangblog.com/2019/06/16/hexo\[04\-06\]/ \-I     # 查看信息
```

不支持嵌套序列，但可以使用几个相邻的序列：

```
http://any.org/archive\[1996\-1999\]/vol\[1\-4\]/part{a,b,c}.html
```

你可以指定一个步长计数器的范围，以获得每第n个数字或字母：

```
http://www.numericals.com/file\[1\-100:10\].txt 
http://www.letters.com/file\[a\-z:2\].txt
```

如果指定URL而没有protocol:// prefix，默认为HTTP。

# 3. 常用选项一

curl通常在操作过程中显示一个进度表，显示传输的数据量、传输速度和估计的剩余时间等。

\-#, --progress-bar
-------------------

将curl进度显示为一个简单的进度条；而不是标准的、具有更多信息的进度表。

```
1 \[root@iZ28xbsfvc4Z 20190702\]\# curl \-O http://www.zhangblog.com/2019/06/16/hexo04/index.html  # 默认的进度表
2   %  Total    %  Received  % Xferd  Average  Speed   Time    Time     Time  Current
3                                     Dload   Upload   Total   Spent    Left  Speed
4 100  97299  100  97299     0     0   186k       0 \--:\--:\-- \--:\--:\-- \--:\--:\--  186k
5 \[root@iZ28xbsfvc4Z 20190702\]\# 
6 \[root@iZ28xbsfvc4Z 20190702\]\# curl \-\# \-O http://www.zhangblog.com/2019/06/16/hexo04/index.html  #简单的进度条
7 ######################################################################## 100.0%
```

\-0, --http1.0
--------------

(HTTP)强制curl使用HTTP 1.0发出请求，而不是使用其内部首选的HTTP 1.1。

\-1, --tlsv1
------------

(SSL)强制curl使用TLS 1.x 版本，当与远程TLS服务进行协商时。 可以使用选项 --tlsv1.0、--tlsv1.1和 --tlsv1.2来更精确地控制TLS版本(如果使用的SSL后端支持这种级别的控制)。

\-2, --sslv2
------------

(SSL)强制curl使用TLS 2 版本，当与远程TLS服务进行协商时。

\-3, --sslv3
------------

(SSL)强制curl使用TLS 3 版本，当与远程TLS服务进行协商时。

\-4, --ipv4
-----------

如果curl能够将一个地址解析为多个IP版本(比如它支持ipv4和ipv6)，那么这个选项告诉curl只将名称解析为IPv4地址。

\-6, --ipv6
-----------

如果curl能够将一个地址解析为多个IP版本(比如它支持ipv4和ipv6)，那么这个选项告诉curl只将名称解析为IPv6地址。

\-a, --append
-------------

(FTP/SFTP)当在上传中使用时，这将告诉curl追加到目标文件而不是覆盖它。如果文件不存在，将创建它。注意，一些SSH服务器(包括OpenSSH)会忽略此标志。

\-A, --user-agent <agent string>
--------------------------------

(HTTP)指定要发送到HTTP服务端的User-Agent字符串。当然也可以使用 -H, --header 选项来设置。 用于模拟客户端，如：谷歌浏览器、火狐浏览器、IE 浏览器等等。

如果多次使用此选项，则将使用最后一个选项。

**模仿浏览器访问**

```
curl \-A "Mozilla/5.0 (Windows NT 10.0; Win64; x64) Chrome/75.0.3770.999" http://www.zhangblog.com/2019/06/24/domainexpire/
```

\--basic
--------

(HTTP)告诉curl使用HTTP基本身份验证。这是默认的。

# 4. 常用选项二

\-b, --cookie <name=data>
-------------------------

(HTTP)将数据作为cookie传递给HTTP服务器。它应该是之前从服务端接收到的“Set-Cookie:”行中的数据。数据格式为“NAME1=VALUE1;NAME2 = VALUE2”。

如果行中没有使用 ‘=’ 符号，则将其视为一个文件名，用于读取先前存储的cookie行，如果它们匹配，则应在此会话中使用。 要读取cookie文件的文件格式应该是纯HTTP头文件或Netscape/Mozilla cookie文件格式。

**注意**：使用 -b, --cookie 指定的文件仅用作输入。文件中不会存储cookies。要存储cookies，可以使用 -c, --cookie-jar 选项，或者您甚至可以使用 -D, --dump-header 将HTTP头保存到文件中。

\-c, --cookie-jar <file name>
-----------------------------

(HTTP)指定希望curl在完成操作后将所有cookie写入哪个文件。 Curl写之前从指定文件读取的所有cookie，以及从远程服务端接收的所有cookie。 如果没有已知的cookie，则不会写入任何文件。该文件将使用Netscape cookie文件格式编写。如果你将文件名设置为单个破折号 “-” ，cookie将被标准输出。

该命令行选项将激活cookie引擎，使curl记录并使用cookies。激活它的另一种方法是使用 -b, --cookie 选项。

如果不能创建或写入cookie jar，那么整个curl操作就不会失败，甚至不能清楚地报告错误。使用 -v 会得到一个警告，但这是你得到的关于这种可能致命的情况的唯一可见反馈。

如果多次使用此选项，将使用最后指定的文件名。

\--connect-timeout <seconds>
----------------------------

连接服务端的超时时间。这只限制了连接阶段，一旦curl连接了此选项就不再使用了。

也可参见：-m, --max-time 选项。

```
1 # 当前 https://www.zhangXX.com 是国外服务器，访问受限
2 \[root@iZ28xbsfvc4Z ~\]\# curl \--connect\-timeout 10 https://www.zhangXX.com | head
3   % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
4                                  Dload  Upload   Total   Spent    Left  Speed
5   0     0    0     0    0     0      0      0 \--:\--:\--  0:00:10 \--:\--:\--     0
6 curl: (28) Connection timed out after 10001 milliseconds
```

\--create-dirs
--------------

当与 -o 选项一起使用时，curl将根据需要创建必要的本地目录层次结构。

这个选项只创建与 -o 选项相关的dirs，没有其他内容。如果 -o 文件名没有使用dir，或者其中提到的dir已经存在，则不会创建dir。

**示例**

```
curl \-o ./hexo04/index.html \--create\-dirs http://www.zhangblog.com/2019/06/16/hexo04
```

\-C, --continue-at <offset>
---------------------------

按给定偏移量继续/恢复以前的文件传输。给定的偏移量是将被跳过的确切字节数，从源文件的开头开始计算，然后再将其传输到目标文件。

使用 “-C -“「注意有空格和无空格的情况」，告诉curl自动找出在哪里/如何恢复传输。然后，它使用给定的输出/输入文件来解决这个问题。

```
1 # 下载一个 2G 的文件，可以反复测试，查看结果
2 curl \-C \- \-o tmp.data http://www.zhangblog.com/uploads/tmp/tmp.data
```

\-d, --data <data>
------------------

使用该选项，那么默认请求方式为 POST。 (HTTP)在POST请求中向HTTP服务器发送指定的数据，与浏览器在用户填写HTML表单并按下submit按钮时所做的相同。这将导致curl使用content-type application/x-www-form-urlencoded将数据传递给服务器。也可参见：-F，-form 。

如果这些命令在同一个命令行使用多次，这些数据片段将使用指定的分隔符 & 合并。 因此，使用 ‘-d name=daniel -d skill=lousy’ 将生成一个类似 ‘name=daniel&skill=lousy’ 的post块，也可以直接这样合并使用。

\-d, --data 与 --data-ascii 相同。post数据为纯粹的二进制数据时，那么使用 --data-binary 选项。要对表单字段的值进行url编码，可以使用 --data-urlencode。

如果您以字母@开始数据，那么其余的应该是一个文件名，以便从其中读取数据。或者 - 如果您希望curl从stdin【标准输入】读取数据。文件的内容必须已经是url编码的。还可以指定多个文件。因此，Posting数据名为 “foobar” 的文件将使用 --data @foobar 完成。

**示例**

**请求信息**:

```
 1 \[root@iZ28xbsfvc4Z 20190712\]\# curl \-sv \--local\-port 9000 \-d 'user=zhang&pwd=123456' http://www.zhangblog.com/2019/06/24/domainexpire/ | head \-n1 
 2 \* About to connect() to www.zhangblog.com port 80 (#0)
 3 \*   Trying 120.27.48.179...
 4 \* Local port: 9000
 5 \* Connected to www.zhangblog.com (120.27.48.179) port 80 (#0)
 6 \> POST /2019/06/24/domainexpire/ HTTP/1.1   # 可见请求方式为POST
 7 \> User\-Agent: curl/7.29.0
 8 \> Host: www.zhangblog.com
 9 \> Accept: \*
```

**抓包信息**

```
\[root@iZ28xbsfvc4Z tcpdump\]\# tcpdump \-i any port 9000 \-A \-s 0
```

![1](https://herozql.oss-cn-beijing.aliyuncs.com/main/1-16420823207353.png)

\--data-ascii <data>
--------------------

参见 -d, --data

\--data-binary <data>
---------------------

(HTTP) POST数据完全按照指定的方式，没有任何额外的处理。

如果您以字母@开始数据，其余的应该是文件名。 数据是以类似于 --data-ascii 的方式发布的，只不过保留了换行，而且永远不会进行转换【数据不转换】。

如果多次使用此选项，第一个选项后面的选项将按照 -d, --data 中的描述追加数据。

\--data-urlencode <data>
------------------------

(HTTP)这个Post 数据，与另一个 --data 选项类似，除执行url编码以外。

将响应协议头写入指定的文件。

如果多次使用此选项，则将使用最后一个选项。

当你想要存储HTTP站点发送给你的头文件时，使用此选项非常方便。

```
1 \[root@iZ28xbsfvc4Z 20190703\]\# curl \-D baidu\_header.info www.baidu.com 
2 ………………
3 \[root@iZ28xbsfvc4Z 20190703\]\# ll
4 total 4
5 \-rw\-r\--r\-- 1 root root 400 Jul  3 10:11 baidu\_header.info  # 生成的头文件
```

之后第二次curl调用通过 -b, --cookie 选项，可以从头部读取 cookies 。然而 -c, --cookie-jar 选项是存储 cookies 更好的方法。

\--digest
---------

(HTTP)启用HTTP摘要身份验证。这是一种身份验证方案，可以防止密码以明文通过网络发送。将此选项与普通的 -u, --user 选项组合使用，以设置用户名和密码。 相关选项请参见 --ntlm, --negotiate 和 --anyauth。

如果多次使用此选项，则只使用第一个选项。

\-e, --referer <URL>
--------------------

(HTTP)将 “Referer Page” 【从哪个页面跳转过来的】信息发送到HTTP服务器。当然也可以使用 -H, --header 标志来设置。

如果多次使用此选项，则将使用最后一个选项。

```
curl \-e 'https:www.baidu.com' http://www.zhangblog.com/2019/06/24/domainexpire/
```

\-f, --fail
-----------

(HTTP)在服务器错误上静默失败(完全没有输出)。这主要是为了使脚本等更好地处理失败的尝试。

在通常情况下，当HTTP服务器无法交付文档时，它会返回一个HTML文档，说明原因(通常还会描述原因)。此标志将阻止curl输出该值并返回错误22。

```
 1 \[root@iZ28xbsfvc4Z 20190713\]\# curl http://www.zhangblog.com/201912312
 2 <html\>
 3 <head\><title\>404 Not Found</title\></head\>
 4 <body bgcolor\="white"\>
 5 <center\><h1\>404 Not Found</h1\></center\>
 6 <hr\><center\>nginx/1.14.2</center\>
 7 </body\>
 8 </html\>
 9 \[root@iZ28xbsfvc4Z 20190713\]\# curl \-f http://www.zhangblog.com/201912312
10 curl: (22) The requested URL returned error: 404 Not Found
```

\-F, --form <name=content>
--------------------------

(HTTP)这允许curl模拟用户按下submit按钮后填充的表单。

该情况让curl 可使用Content-Type multipart/form-data POST数据。也可以上传二进制文件等。

@文件名：使一个文件作为文件上传附加在post中。 <文件名：从文件中获取该文本字段的内容。

**例如，要将密码文件发送到服务器，其中“password”是表单字段的名称，/etc/passwd将作为输入:**

```
curl \-F password\=@/etc/passwd www.mypasswords.com
```

**您还可以使用 ‘type=’ 告诉curl使用什么 Content-Type ，方法类似于：** 

```
curl \-F "web=@index.html;type=text/html" url.com
或
curl \-F "name=daniel;type=text/foo" url.com
```

**可以通过设置 filename= 更改本地上传的文件名，如下：** 

```
curl \-F "file=@localfile;filename=nameinpost" url.com
```

上传的文件名从改为了 nameinpost

**如果文件名/路径包括 ‘,’ 或 ‘;’ ，必须用双引号括起来：** 

```
curl \-F "file=@\\"localfile\\";filename=\\"nameinpost\\"" url.com
或
curl \-F 'file=@"localfile";filename="nameinpost"' url.com
```

最外层可用单引号或双引号。

这个选项可以多次使用。

**_请勿如下使用_**

```
curl \-F 'user=zhang&password=pwd' url.com   # 这种用法是错误的
```

\--form-string <name=string>
----------------------------

(HTTP)类似于 --form，只是命名参数的value字符串按字面意思使用。 在值中以 ‘@’ 和 ‘<’ 开头的字符，以及 ‘;type=’ 字符串没有特殊的含义。

如果字符串值有可能意外触发 --form 的 “@” 或 “<” 特性，请优先使用此选项。

\-g, --globoff
--------------

这个选项关闭了“URL全局解析器”。当您设置这个选项时，您可以指定包含字母 {}\[\] 的url，而不需要curl本身来解释它们。

注意，这些字母不是正常的合法URL内容，但是它们应该按照URI标准进行编码。

\-G, --get
----------

使用此选项时，将使所有使用 -d, --data 或 --data-binary 指定的数据在HTTP GET请求中使用，而不是在POST请求中使用。 数据将被追加到URL的一个 ‘?’ 的分隔符后。

如果与 -I 结合使用，POST数据将被替换追加到带有HEAD请求的URL中。

如果多次使用此选项，则只使用第一个选项。

**示例**

```
 1 \[root@iZ28xbsfvc4Z 20190712\]\# curl \-sv \-G \--local\-port 9000 \-d 'user=zhang&pwd=123456' http://www.zhangblog.com/2019/06/24/domainexpire/ | head \-n1 
 2 或则
 3 \[root@iZ28xbsfvc4Z 20190713\]\# curl \-sv \--local\-port 9000 "http://www.zhangblog.com/2019/06/24/domainexpire/?user=zhang&pwd=123456" | head \-n1
 4 \* About to connect() to www.zhangblog.com port 80 (#0)
 5 \*   Trying 120.27.48.179...
 6 \* Local port: 9000
 7 \* Connected to www.zhangblog.com (120.27.48.179) port 80 (#0)
 8 \> GET /2019/06/24/domainexpire/?user\=zhang&pwd\=123456 HTTP/1.1  # 可见请求方式为 GET，且参数追加到了URI后
 9 \> User\-Agent: curl/7.29.0
10 \> Host: www.zhangblog.com
11 \> Accept: \*
```

**抓包信息**

```
\[root@iZ28xbsfvc4Z tcpdump\]\# tcpdump \-i any port 9000 \-A \-s 0
```

![2](https://herozql.oss-cn-beijing.aliyuncs.com/main/2-16420823986184.png)

(HTTP) 要发送到服务端的自定义请求头。

此选项可多次用于添加/替换/删除多个headers。

```
1 curl \-H 'Connection: keep-alive' \-H 'Referer: https://sina.com.cn' \-H 'User-Agent: Mozilla/1.0' http://www.zhangblog.com/2019/06/24/domainexpire/
```

\--ignore-content-length
------------------------

(HTTP)忽略Content-Length 头信息。

\-i, --include
--------------

(HTTP)在输出的内容中包含HTTP 头信息。

```
curl \-i https://www.baidu.com
```

\-I, --head
-----------

(HTTP/FTP/FILE)只获取HTTP头文件。 在FTP或FILE 文件上使用时，curl只显示文件大小和最后修改时间。

```
curl \-I https://www.baidu.com
```

\-k, --insecure
---------------

(SSL)允许curl执行不安全的SSL连接和传输。 所有SSL连接都尝试使用默认安装的CA证书包来确保安全。

**示例**

```
 1 \[root@iZ28xbsfvc4Z ~\]\# curl https://140.205.16.113/  # 被拒绝
 2 curl: (51) Unable to communicate securely with peer: requested domain name does not match the server's certificate.
 3 \[root@iZ28xbsfvc4Z ~\]\# 
 4 \[root@iZ28xbsfvc4Z ~\]\# curl \-k https://140.205.16.113/  # 允许执行不安全的证书连接
 5 <!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN"\>
 6 <html\>
 7 <head\><title\>403 Forbidden</title\></head\>
 8 <body bgcolor\="white"\>
 9 <h1\>403 Forbidden</h1\>
10 <p\>You don't have permission to access the URL on this server.<hr/\>Powered by Tengine</body\>
11 </html\>
```

\--keepalive-time <seconds>
---------------------------

keepalive 时长。如果使用no-keepalive，则此选项无效。

如果多次使用此选项，则将使用最后一个选项。如果未指定，该选项默认为60秒。

\--key <key>
------------

(SSL/SSH)私钥文件名。允许你在这个单独的文件中提供你的私钥。

对于SSH，如果没有指定，curl尝试如下顺序：’~/.ssh/id\_rsa’，’~/.ssh/id\_dsa’，’./id\_rsa’，’./id\_dsa’。

如果多次使用此选项，则将使用最后一个选项。

\--key-type <type>
------------------

(SSL)私钥文件类型。指定 --key 提供的私钥的类型。支持DER、PEM和ENG。如果没有指定，则定为PEM。

如果多次使用此选项，则将使用最后一个选项。

\-L, --location
---------------

(HTTP/HTTPS) 跟踪重定向 如果服务器报告请求页面已移动到另一个位置(用location: header和3XX响应代码表示)，此选项将使curl在新位置上重做请求。

如果与 -i, --include 或 -I, --head 一起使用，将显示所有请求页面的标题。

```
 1 \[root@iZ28xbsfvc4Z ~\]\# curl \-I \-L https://baidu.com/ 
 2 HTTP/1.1 302 Moved Temporarily  # 302 重定向
 3 Server: bfe/1.0.8.18
 4 Date: Thu, 04 Jul 2019 03:07:15 GMT
 5 Content\-Type: text/html
 6 Content\-Length: 161
 7 Connection: keep\-alive
 8 Location: http://www.baidu.com/
 9 
10 HTTP/1.1 200 OK
11 Accept\-Ranges: bytes
12 Cache\-Control: private, no\-cache, no\-store, proxy\-revalidate, no\-transform
13 Connection: Keep\-Alive
14 Content\-Length: 277
15 Content\-Type: text/html
16 Date: Thu, 04 Jul 2019 03:07:15 GMT
17 Etag: "575e1f60-115"
18 Last\-Modified: Mon, 13 Jun 2016 02:50:08 GMT
19 Pragma: no\-cache
20 Server: bfe/1.0.8.18
```

\--limit-rate <speed>
---------------------

指定要使用curl的最大传输速率。

如果有一个有限的管道，并且希望传输不要使用您的全部带宽，那么这个特性是非常有用的。

```
curl \--limit\-rate 500 http://www.baidu.com/
curl \--limit\-rate 2k http://www.baidu.com/
```

**单位**：默认字节，除非添加后缀。附加 “k” 或 “K” 表示千字节， “m” 或 “M” 表示兆字节，而 “g” 或 “G” 表示千兆字节。例如:200K, 3m和1G。

给定的速率是整个传输过程中计算的平均速度。这意味着curl可能在短时间内使用更高的传输速度，但是随着时间的推移，它只使用给定的速率。

如果多次使用此选项，则将使用最后一个选项。

\--local-port <num>\[-num\]
---------------------------

指定本地的一个端口或端口范围去连接。

请注意，端口号本质上是一种稀缺资源，有时会很忙，因此将此范围设置为太窄可能会导致不必要的连接失败。

```
curl \--local\-port 9000 http://www.baidu.com/
curl \--local\-port 9000\-9999 http://www.baidu.com/
```

\-m, --max-time <seconds>
-------------------------

允许整个操作花费的最大时间(以秒为单位)。

这对于防止由于网络或链接变慢而导致批处理作业挂起数小时非常有用。

也可参见：--connect-timeout 选项

```
 1 \[root@iZ28xbsfvc4Z ~\]\# curl \-m 10 \--limit\-rate 5 http://www.baidu.com/ | head  # 超过10秒后，断开连接
 2   % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 3                                  Dload  Upload   Total   Spent    Left  Speed
 4   2  2381    2    50    0     0      4      0  0:09:55  0:00:10  0:09:45     4
 5 curl: (28) Operation timed out after 10103 milliseconds with 50 out of 2381 bytes received
 6 <!DOCTYPE html\>
 7 <!\--STATUS OK\--\><html\> <head\><met
 8 ### 或
 9 \[root@iZ28xbsfvc4Z ~\]\# curl \-m 10 https://www.zhangXX.com | head   # 超过10秒后，断开连接
10   % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
11                                  Dload  Upload   Total   Spent    Left  Speed
12   0     0    0     0    0     0      0      0 \--:\--:\--  0:00:10 \--:\--:\--     0
13 curl: (28) Connection timed out after 10001 milliseconds
```

\--max-filesize <bytes>
-----------------------

指定要下载的文件的最大大小(以字节为单位)。 如果请求的文件大于这个值，那么传输将不会启动，curl将返回退出代码63。

**示例**

```
 1 \[root@iZ28xbsfvc4Z ~\]\# curl \-I http://www.zhangblog.com/uploads/hexo/00.jpg # 正常
 2 HTTP/1.1 200 OK
 3 Server: nginx/1.14.2
 4 Date: Thu, 04 Jul 2019 07:24:24 GMT
 5 Content\-Type: image/jpeg
 6 Content\-Length: 18196
 7 Last\-Modified: Mon, 24 Jun 2019 01:43:02 GMT
 8 Connection: keep\-alive
 9 ETag: "5d102aa6-4714"
10 Accept\-Ranges: bytes
11 
12 \[root@iZ28xbsfvc4Z ~\]\# echo $?
13 0
14 \[root@iZ28xbsfvc4Z ~\]\# 
15 \[root@iZ28xbsfvc4Z ~\]\# 
16 \[root@iZ28xbsfvc4Z ~\]\# curl \--max\-filesize 1000 \-I http://www.zhangblog.com/uploads/hexo/00.jpg # 受限异常
17 HTTP/1.1 200 OK
18 Server: nginx/1.14.2
19 Date: Thu, 04 Jul 2019 07:24:54 GMT
20 Content\-Type: image/jpeg
21 curl: (63) Maximum file size exceeded
22 \[root@iZ28xbsfvc4Z ~\]\# 
23 \[root@iZ28xbsfvc4Z ~\]\# echo $?
24 63
```

\--max-redirs <num>
-------------------

设置允许的最大重定向跟踪数。

如果也使用了 -L, --location，则此选项可用于防止curl在悖论中无限重定向。 默认情况下，限制为50重定向。将此选项设置为-1，使其无限。

\--no-keepalive
---------------

禁用在TCP连接上使用keepalive消息，因为默认情况下curl启用了它们。

注意，这是文档中已否定的选项名。因此，您可以使用 --keepalive 来强制keepalive。

\-o, --output <file>
--------------------

输出到一个文件，而不是标准输出。

如果使用 {} 或 \[\] 来获取多个documents。可以使用 ‘#’ 后跟说明符中的一个数字。该变量将替换为正在获取URL的当前字符串。就像：

```
curl http://{one,two}.site.com \-o "file\_#1.txt"
curl http://{site,host}.host\[1\-5\].com \-o "#1\_#2"
```

**示例1**

```
 1 \[root@iZ28xbsfvc4Z 20190703\]\# curl "http://www.zhangblog.com/2019/06/16/hexo{04,05,06}/" \-o "file\_#1.info"   # 注意curl 的地址需要用引号括起来
 2 或
 3 \[root@iZ28xbsfvc4Z 20190703\]\# curl "http://www.zhangblog.com/2019/06/16/hexo\[04-06\]/" \-o "file\_#1.info"   # 注意curl 的地址需要用引号括起来
 4 \[1/3\]: http://www.zhangblog.com/2019/06/16/hexo04/ \--\> file\_04.info
 5   % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 6                                  Dload  Upload   Total   Spent    Left  Speed
 7 100 97299  100 97299    0     0  1551k      0 \--:\--:\-- \--:\--:\-- \--:\--:\-- 1557k
 8 
 9 \[2/3\]: http://www.zhangblog.com/2019/06/16/hexo05/ \--\> file\_05.info
10 100 54409  100 54409    0     0   172M      0 \--:\--:\-- \--:\--:\-- \--:\--:\--  172M
11 
12 \[3/3\]: http://www.zhangblog.com/2019/06/16/hexo06/ \--\> file\_06.info
13 100 56608  100 56608    0     0   230M      0 \--:\--:\-- \--:\--:\-- \--:\--:\--  230M
14 \[root@iZ28xbsfvc4Z 20190703\]\# 
15 \[root@iZ28xbsfvc4Z 20190703\]\# ll
16 total 212
17 \-rw\-r\--r\-- 1 root root 97299 Jul  4 16:51 file\_04.info
18 \-rw\-r\--r\-- 1 root root 54409 Jul  4 16:51 file\_05.info
19 \-rw\-r\--r\-- 1 root root 56608 Jul  4 16:51 file\_06.info
```

**示例2**

```
 1 \[root@iZ28xbsfvc4Z 20190703\]\# curl "http://www.{baidu,douban}.com" \-o "site\_#1.txt"  # 注意curl 的地址需要用引号括起来
 2 \[1/2\]: http://www.baidu.com \--\> site\_baidu.txt
 3   % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 4                                  Dload  Upload   Total   Spent    Left  Speed
 5 100  2381  100  2381    0     0  46045      0 \--:\--:\-- \--:\--:\-- \--:\--:\-- 46686
 6 
 7 \[2/2\]: http://www.douban.com \--\> site\_douban.txt
 8 100   162  100   162    0     0   3173      0 \--:\--:\-- \--:\--:\-- \--:\--:\--  3173
 9 \[root@iZ28xbsfvc4Z 20190703\]\# 
10 \[root@iZ28xbsfvc4Z 20190703\]\# ll
11 total 220
12 \-rw\-r\--r\-- 1 root root  2381 Jul  4 16:53 site\_baidu.txt
13 \-rw\-r\--r\-- 1 root root   162 Jul  4 16:53 site\_douban.txt
```

\-O, --remote-name
------------------

写入到本地文件，名称与远程文件的名称相同。(只使用远程文件的文件部分，路径被切断。)

用于保存的远程文件名是从给定的URL中提取的，没有其他内容。 因此，文件将保存在当前工作目录中。如果希望将文件保存在另一个目录中，请确保在curl调用 -O, --remote-name之前更改当前工作目录!

```
1 \[root@iZ28xbsfvc4Z 20190712\]\# curl \-O https://www.baidu.com   # 使用了 \-O 选项，必须指定到具体的文件  错误使用
2 curl: Remote file name has no length!
3 curl: try 'curl --help' or 'curl --manual' for more information
4 \[root@iZ28xbsfvc4Z 20190712\]\# curl \-O https://www.baidu.com/index.html   # 使用了 \-O 选项，必须指定到具体的文件  正确使用
5   % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
6                                  Dload  Upload   Total   Spent    Left  Speed
7 100  2443  100  2443    0     0  13289      0 \--:\--:\-- \--:\--:\-- \--:\--:\-- 13349
```

\--pass <phrase>
----------------

(SSL/SSH)私钥密码

如果多次使用此选项，则将使用最后一个选项。

\--post301
----------

告诉curl当301重定向时，不要将POST请求转换为GET请求。

非rfc行为在web浏览器中无处不在，因此curl在缺省情况下进行转换以保持一致性。但是，服务器可能需要在重定向之后将POST保留为POST。

这个选项只有在使用 -L, --location 时才有意义

\--post302
----------

告诉curl当302重定向时，不要将POST请求转换为GET请求。

非rfc行为在web浏览器中无处不在，因此curl在缺省情况下进行转换以保持一致性。但是，服务器可能需要在重定向之后将POST保留为POST。

这个选项只有在使用 -L, --location 时才有意义

\--post303
----------

告诉curl当303重定向时，不要将POST请求转换为GET请求。

非rfc行为在web浏览器中无处不在，因此curl在缺省情况下进行转换以保持一致性。但是，服务器可能需要在重定向之后将POST保留为POST。

这个选项只有在使用 -L, --location 时才有意义

**说明：**  上述三个选项都是为了防止在重定向过程中，原来的 POST 请求，变为 GET请求。为了防止该情况，有两种处理方式。 1、使用上述选项可避免； 2、使用 -X POST 选项和命令。

**示例**

```
\[root@iZ28xbsfvc4Z ~\]\# curl \-Lsv \-d 'user=zhang' https://baidu.com | head \-n1
```

开始是POST请求，302 重定向后变为了 GET请求。

![3](https://herozql.oss-cn-beijing.aliyuncs.com/main/3-16420824756625.png)

```
\[root@iZ28xbsfvc4Z ~\]\# curl \-Lsv \-d 'user=zhang' \--post301 \--post302 \--post303 https://baidu.com | head \-n1
```

前后都是 POST 请求。但是选项较多。

![4](https://herozql.oss-cn-beijing.aliyuncs.com/main/4-16420824841756.png)

```
\[root@iZ28xbsfvc4Z ~\]\# curl \-Lsv \-d 'user=zhang' \-X POST https://baidu.com | head \-n1
```

前后都是 POST 请求。推荐使用此命令。

![5](https://herozql.oss-cn-beijing.aliyuncs.com/main/5-16420824919007.png)

\--pubkey <key>
---------------

(SSH)公钥文件名。允许在这个单独的文件中提供公钥。

如果多次使用此选项，则将使用最后一个选项。

\-r, --range <range>
--------------------

(HTTP/FTP/SFTP/FILE)从HTTP/1.1、FTP或SFTP服务器或本地文件检索字节范围。范围可以通过多种方式指定。用于分段下载。

有时文件比较大，或者难以迅速传输，而利用分段传输，可以实现稳定、高效并且有保障的传输，更具有实用性，同时容易对差错文件进行更正。

0-499：指定前500个字节 500-999：指定第二个500字节 -500：指定最后500个字节 9500-：指定9500字节及之后的字节 0-0,-1：指定第一个和最后一个字节 500-700,600-799：从偏移量500开始指定300字节 100-199,500-599：指定两个单独100字节的范围

**分段下载**

```
 1 \[root@iZ28xbsfvc4Z 20190715\]\# curl \-I http://www.zhangblog.com/uploads/hexo/00.jpg   # 查看文件大小
 2 HTTP/1.1 200 OK
 3 Server: nginx/1.14.2
 4 Date: Mon, 15 Jul 2019 03:23:44 GMT
 5 Content\-Type: image/jpeg
 6 Content\-Length: 18196   # 文件大小
 7 Last\-Modified: Fri, 05 Jul 2019 08:04:58 GMT
 8 Connection: keep\-alive
 9 ETag: "5d1f04aa-4714"
10 Accept\-Ranges: bytes
11 \[root@iZ28xbsfvc4Z 20190715\]\# curl \-r 0\-499   \-o 00\-jpg.part1 http://www.zhangblog.com/uploads/hexo/00.jpg
12 \[root@iZ28xbsfvc4Z 20190715\]\# curl \-r 500\-999 \-o 00\-jpg.part2 http://www.zhangblog.com/uploads/hexo/00.jpg
13 \[root@iZ28xbsfvc4Z 20190715\]\# curl \-r 1000\-   \-o 00\-jpg.part3 http://www.zhangblog.com/uploads/hexo/00.jpg
```

**查看下载文件**

```
1 \[root@iZ28xbsfvc4Z 20190715\]\# ll
2 total 36
3 \-rw\-r\--r\-- 1 root root   500 Jul 15 11:25 00\-jpg.part1
4 \-rw\-r\--r\-- 1 root root   500 Jul 15 11:25 00\-jpg.part2
5 \-rw\-r\--r\-- 1 root root 17196 Jul 15 11:26 00\-jpg.part3
```

**文件合并**

```
1 \[root@iZ28xbsfvc4Z 20190715\]\# cat 00\-jpg.part1 00\-jpg.part2 00\-jpg.part3 \> 00.jpg
2 \[root@iZ28xbsfvc4Z 20190715\]\# ll
3 total 56
4 \-rw\-r\--r\-- 1 root root 18196 Jul 15 11:29 00.jpg
```

\-R, --remote-time
------------------

使curl尝试获取远程文件的时间戳，如果可用，则使本地文件获得相同的时间戳【针对修改时间戳Modify】。

```
curl \-o nfs1.info \-R http://www.zhangblog.com/2019/07/05/nfs1/
```

\--retry <num>
--------------

传输出现问题时，重试的次数。数字设置为0将使curl不重试(这是缺省值)。

出现的瞬时错误如：timeout、FTP 4xx响应状代码或HTTP 5xx响应状代码。

当curl准备重试传输时，它将首先等待一秒钟，之后对于所有即将到来的重试，它将把等待时间延长一倍，直到达到10分钟，这将是其余重试之间的延迟。

\--retry-delay <seconds>
------------------------

传输出现问题时，设置重试间隔时间。将此延迟设置为零将使curl使用默认的延迟时间。

\--retry-max-time <seconds>
---------------------------

传输出现问题时，设置最大重试时间。将此选项设置为0则不超时重试。

\-s, --silent
-------------

静默或静音模式。不显示进度表/条或错误消息。

**示例**

```
1 \[root@iZ28xbsfvc4Z 20190713\]\# curl https://www.baidu.com | head \-n1  # 默认有进度表
2   % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
3                                  Dload  Upload   Total   Spent    Left  Speed
4 100  2443  100  2443    0     0  13346      0 \--:\--:\-- \--:\--:\-- \--:\--:\-- 13349
5 <!DOCTYPE html\>
6 \[root@iZ28xbsfvc4Z 20190713\]\# curl \-s https://www.baidu.com | head \-n1
7 <!DOCTYPE html\>
```

\-S, --show-error
-----------------

当与 -s 一起使用时，如果curl失败，curl将显示一条错误消息。

```
1 \[root@iZ28xbsfvc4Z 20190713\]\# curl \-s https://140.205.16.113/ 
2 \[root@iZ28xbsfvc4Z 20190713\]\# 
3 \[root@iZ28xbsfvc4Z 20190713\]\# curl \-sS https://140.205.16.113/ 
4 curl: (51) Unable to communicate securely with peer: requested domain name does not match the server's certificate.
```

\--stderr <file>
----------------

将错误信息重定向到一个文件。如果文件名是普通的 ‘-‘，则将其写入stdout。

如果多次使用此选项，则将使用最后一个选项。

```
1 \[root@iZ28xbsfvc4Z 20190713\]\# curl \--stderr err.info https://140.205.16.113/ 
2 \[root@iZ28xbsfvc4Z 20190713\]\# ll
3 total 92
4 \-rw\-r\--r\-- 1 root root   116 Jul 13 10:19 err.info
5 \[root@iZ28xbsfvc4Z 20190713\]\# cat err.info 
6 curl: (51) Unable to communicate securely with peer: requested domain name does not match the server's certificate.
```

\-T, --upload-file <file>
-------------------------

这将指定的本地文件传输到远程URL。如果指定的URL中没有文件部分，Curl将附加本地文件名。

注意：必须在最后一个目录上使用尾随 / 来真正证明Curl没有文件名，否则Curl会认为您的最后一个目录名是要使用的远程文件名。这很可能导致上传操作失败。 如果在HTTP(S)服务器上使用此命令，则将使用PUT命令。

同时也支持多个文件上传，如下：

```
curl \-T "{file1,file2}" http://www.uploadtothissite.com
或则
curl \-T "img\[1-1000\].png" ftp://ftp.picturemania.com/upload/
```

\--trace <file>
---------------

对指定文件进行debug。包括所有传入和传出数据。

此选项会覆盖之前使用的 -v、--verbose或 --trace-ascii。

如果多次使用此选项，则将使用最后一个选项。

```
curl \--trace trace.info https://www.baidu.com
```

\--trace-ascii <file>
---------------------

对指定文件进行debug。包括所有传入和传出数据。

这非常类似于 --trace，但是省略了十六进制部分，只显示转储的ASCII部分。使它输出更小，对于我们来说可能更容易阅读。

此选项会覆盖之前使用的 -v、--verbose或 --trace。

如果多次使用此选项，则将使用最后一个选项。

```
curl \--trace\-ascii trace2.info https://www.baidu.com
```

\--trace-time
-------------

为curl显示的每个跟踪或冗长的行添加时间戳。

```
curl \--trace\-ascii trace3.info \--trace\-time https://www.baidu.com
```

\-v, --verbose
--------------

显示详细操作信息。主要用于调试。

以 > 开头的行表示curl发送的”header data”；< 表示curl接收到的通常情况下隐藏的”header data”；而以 \* 开头的行表示curl提供的附加信息。

```
 1 \[root@iZ28xbsfvc4Z 20190712\]\# curl \-v https://www.baidu.com
 2 \* About to connect() to www.baidu.com port 443 (#0)
 3 \*   Trying 180.101.49.12...
 4 \* Connected to www.baidu.com (180.101.49.12) port 443 (#0)
 5 \* Initializing NSS with certpath: sql:/etc/pki/nssdb
 6 \*   CAfile: /etc/pki/tls/certs/ca\-bundle.crt
 7   CApath: none
 8 \* SSL connection using TLS\_ECDHE\_RSA\_WITH\_AES\_128\_GCM\_SHA256
 9 \* Server certificate:
10 \*     subject: CN\=baidu.com,O\="Beijing Baidu Netcom Science Technology Co., Ltd",OU\=service operation department,L\=beijing,ST\=beijing,C\=CN
11 \*     start date: May 09 01:22:02 2019 GMT
12 \*     expire date: Jun 25 05:31:02 2020 GMT
13 \*     common name: baidu.com
14 \*     issuer: CN\=GlobalSign Organization Validation CA \- SHA256 \- G2,O\=GlobalSign nv\-sa,C\=BE
15 \> GET / HTTP/1.1
16 \> User\-Agent: curl/7.29.0
17 \> Host: www.baidu.com
18 \> Accept: \*
```

\-w, --write-out <format>
-------------------------

在完成和成功操作后要在stdout上显示什么。

支持如下变量，具体含义请自行参见curl文档。

```
content\_type
filename\_effective
ftp\_entry\_path
http\_code
http\_connect
local\_ip
local\_port
num\_connects
num\_redirects
redirect\_url
remote\_ip
remote\_port
size\_download
size\_header
size\_request
size\_upload
speed\_download
speed\_upload
ssl\_verify\_result
time\_appconnect
time\_connect
time\_namelookup
time\_pretransfer
time\_redirect
time\_starttransfer
time\_total
url\_effective
```

**示例**

```
1 \[root@iZ28xbsfvc4Z 20190713\]\# curl \-o /dev/null \-s \-w %{content\_type} www.baidu.com  # 输出结果没有换行
2 text/html\[root@iZ28xbsfvc4Z 20190713\]\# 
3 \[root@iZ28xbsfvc4Z 20190713\]\# curl \-o /dev/null \-s \-w %{http\_code} www.baidu.com  # 输出结果没有换行
4 200\[root@iZ28xbsfvc4Z 20190713\]\# 
5 \[root@iZ28xbsfvc4Z 20190713\]\# curl \-o /dev/null \-s \-w %{local\_port} www.baidu.com  # 输出结果没有换行
6 37346\[root@iZ28xbsfvc4Z 20190713\]\# 
7 \[root@iZ28xbsfvc4Z 20190713\]#
```

\-x, --proxy <\[protocol://\]\[user:password@\]proxyhost\[:port\]>
------------------------------------------------------------------

使用指定的HTTP代理。如果没有指定端口号，则假定它位于端口1080。

\-X, --request <command>
------------------------

(HTTP)指定与HTTP服务器通信时的请求方式。默认GET

```
curl \-vs \-X POST https://www.baidu.com | head \-n1
```

![6](https://herozql.oss-cn-beijing.aliyuncs.com/main/6-16420825504738.png)

```
curl \-vs \-X PUT https://www.baidu.com | head \-n1
```

![6](https://herozql.oss-cn-beijing.aliyuncs.com/main/6-16420825552919.png)

# 5. 选项简明表

```bash
在以下选项中，(H) 表示仅适用 HTTP/HTTPS ，(F) 表示仅适用于 FTP
    --anyauth      选择 "any" 认证方法 (H)
-a, --append        添加要上传的文件 (F/SFTP)
    --basic        使用HTTP基础认证（Basic Authentication）(H)
    --cacert FILE  CA 证书，用于每次请求认证 (SSL)
    --capath DIR    CA 证书目录 (SSL)
-E, --cert CERT[:PASSWD] 客户端证书文件及密码 (SSL)
    --cert-type TYPE 证书文件类型 (DER/PEM/ENG) (SSL)
    --ciphers LIST  SSL 秘钥 (SSL)
    --compressed    请求压缩 (使用 deflate 或 gzip)
-K, --config FILE  指定配置文件
    --connect-timeout SECONDS  连接超时设置
-C, --continue-at OFFSET  断点续转
-b, --cookie STRING/FILE  Cookies字符串或读取Cookies的文件位置 (H)
-c, --cookie-jar FILE  操作结束后，要写入 Cookies 的文件位置 (H)
    --create-dirs  创建必要的本地目录层次结构
    --crlf          在上传时将 LF 转写为 CRLF
    --crlfile FILE  从指定的文件获得PEM格式CRL列表
-d, --data DATA    HTTP POST 数据 (H)
    --data-ascii DATA  ASCII 编码 HTTP POST 数据 (H)
    --data-binary DATA  binary 编码 HTTP POST 数据 (H)
    --data-urlencode DATA  url 编码 HTTP POST 数据 (H)
    --delegation STRING GSS-API 委托权限
    --digest        使用数字身份验证 (H)
    --disable-eprt  禁止使用 EPRT 或 LPRT (F)
    --disable-epsv  禁止使用 EPSV (F)
-D, --dump-header FILE  将头信息写入指定的文件
    --egd-file FILE  为随机数据设置EGD socket路径(SSL)
    --engine ENGINGE  加密引擎 (SSL). "--engine list" 指定列表
-f, --fail          连接失败时不显示HTTP错误信息 (H)
-F, --form CONTENT  模拟 HTTP 表单数据提交（multipart POST） (H)
    --form-string STRING  模拟 HTTP 表单数据提交 (H)
    --ftp-account DATA  帐户数据提交 (F)
    --ftp-alternative-to-user COMMAND  指定替换 "USER [name]" 的字符串 (F)
    --ftp-create-dirs  如果不存在则创建远程目录 (F)
    --ftp-method [MULTICWD/NOCWD/SINGLECWD] 控制 CWD (F)
    --ftp-pasv      使用 PASV/EPSV 替换 PORT (F)
-P, --ftp-port ADR  使用指定 PORT 及地址替换 PASV (F)
    --ftp-skip-pasv-ip 跳过 PASV 的IP地址 (F)
    --ftp-pret      在 PASV 之前发送 PRET (drftpd) (F)
    --ftp-ssl-ccc  在认证之后发送 CCC (F)
    --ftp-ssl-ccc-mode ACTIVE/PASSIVE  设置 CCC 模式 (F)
    --ftp-ssl-control ftp 登录时需要 SSL/TLS (F)
-G, --get          使用 HTTP GET 方法发送 -d 数据  (H)
-g, --globoff      禁用的 URL 队列 及范围使用 {} 和 []
-H, --header LINE  要发送到服务端的自定义请求头 (H)
-I, --head          仅显示响应文档头
-h, --help          显示帮助
-0, --http1.0      使用 HTTP 1.0 (H)
    --ignore-content-length  忽略 HTTP Content-Length 头
-i, --include      在输出中包含协议头 (H/F)
-k, --insecure      允许连接到 SSL 站点，而不使用证书 (H)
    --interface INTERFACE  指定网络接口／地址
-4, --ipv4          将域名解析为 IPv4 地址
-6, --ipv6          将域名解析为 IPv6 地址
-j, --junk-session-cookies 读取文件中但忽略会话cookie (H)
    --keepalive-time SECONDS  keepalive 包间隔
    --key KEY      私钥文件名 (SSL/SSH)
    --key-type TYPE 私钥文件类型 (DER/PEM/ENG) (SSL)
    --krb LEVEL    启用指定安全级别的 Kerberos (F)
    --libcurl FILE  命令的libcurl等价代码
    --limit-rate RATE  限制传输速度
-l, --list-only    只列出FTP目录的名称 (F)
    --local-port RANGE  强制使用的本地端口号
-L, --location      跟踪重定向 (H)
    --location-trusted 类似 --location 并发送验证信息到其它主机 (H)
-M, --manual        显示全手动
    --mail-from FROM  从这个地址发送邮件
    --mail-rcpt TO  发送邮件到这个接收人(s)
    --mail-auth AUTH  原始电子邮件的起始地址
    --max-filesize BYTES  下载的最大文件大小 (H/F)
    --max-redirs NUM  最大重定向数 (H)
-m, --max-time SECONDS  允许的最多传输时间
    --metalink      处理指定的URL上的XML文件
    --negotiate    使用 HTTP Negotiate 认证 (H)
-n, --netrc        必须从 .netrc 文件读取用户名和密码
    --netrc-optional 使用 .netrc 或 URL; 将重写 -n 参数
    --netrc-file FILE  设置要使用的 netrc 文件名
-N, --no-buffer    禁用输出流的缓存
    --no-keepalive  禁用 connection 的 keepalive
    --no-sessionid  禁止重复使用 SSL session-ID (SSL)
    --noproxy      不使用代理的主机列表
    --ntlm          使用 HTTP NTLM 认证 (H)
-o, --output FILE  将输出写入文件，而非 stdout
    --pass PASS    传递给私钥的短语 (SSL/SSH)
    --post301      在 301 重定向后不要切换为 GET 请求 (H)
    --post302      在 302 重定向后不要切换为 GET 请求 (H)
    --post303      在 303 重定向后不要切换为 GET 请求 (H)
-#, --progress-bar  以进度条显示传输进度
    --proto PROTOCOLS  启用/禁用 指定的协议
    --proto-redir PROTOCOLS  在重定向上 启用/禁用 指定的协议
-x, --proxy [PROTOCOL://]HOST[:PORT] 在指定的端口上使用代理
    --proxy-anyauth 在代理上使用 "any" 认证方法 (H)
    --proxy-basic  在代理上使用 Basic 认证  (H)
    --proxy-digest  在代理上使用 Digest 认证 (H)
    --proxy-negotiate 在代理上使用 Negotiate 认证 (H)
    --proxy-ntlm    在代理上使用 NTLM 认证 (H)
-U, --proxy-user USER[:PASSWORD]  代理用户名及密码
    --proxy1.0 HOST[:PORT]  在指定的端口上使用 HTTP/1.0 代理
-p, --proxytunnel  使用HTTP代理 (用于 CONNECT)
    --pubkey KEY    公钥文件名 (SSH)
-Q, --quote CMD    在传输开始前向服务器发送命令 (F/SFTP)
    --random-file FILE  读取随机数据的文件 (SSL)
-r, --range RANGE  仅检索范围内的字节
    --raw          使用原始HTTP传输，而不使用编码 (H)
-e, --referer      Referer URL (H)
-J, --remote-header-name 从远程文件读取头信息 (H)
-O, --remote-name  将输出写入远程文件
    --remote-name-all 使用所有URL的远程文件名
-R, --remote-time  将远程文件的时间设置在本地输出上
-X, --request COMMAND  使用指定的请求命令
    --resolve HOST:PORT:ADDRESS  将 HOST:PORT 强制解析到 ADDRESS
    --retry NUM  出现问题时的重试次数
    --retry-delay SECONDS 重试时的延时时长
    --retry-max-time SECONDS  仅在指定时间段内重试
-S, --show-error    显示错误. 在选项 -s 中，当 curl 出现错误时将显示
-s, --silent        Silent模式。不输出任务内容
    --socks4 HOST[:PORT]  在指定的 host + port 上使用 SOCKS4 代理
    --socks4a HOST[:PORT]  在指定的 host + port 上使用 SOCKSa 代理
    --socks5 HOST[:PORT]  在指定的 host + port 上使用 SOCKS5 代理
    --socks5-hostname HOST[:PORT] SOCKS5 代理，指定用户名、密码
    --socks5-gssapi-service NAME  为gssapi使用SOCKS5代理服务名称
    --socks5-gssapi-nec  与NEC Socks5服务器兼容
-Y, --speed-limit RATE  在指定限速时间之后停止传输
-y, --speed-time SECONDS  指定时间之后触发限速. 默认 30
    --ssl          尝试 SSL/TLS (FTP, IMAP, POP3, SMTP)
    --ssl-reqd      需要 SSL/TLS (FTP, IMAP, POP3, SMTP)
-2, --sslv2        使用 SSLv2 (SSL)
-3, --sslv3        使用 SSLv3 (SSL)
    --ssl-allow-beast 允许的安全漏洞，提高互操作性(SSL)
    --stderr FILE  重定向 stderr 的文件位置. - means stdout
    --tcp-nodelay  使用 TCP_NODELAY 选项
-t, --telnet-option OPT=VAL  设置 telnet 选项
    --tftp-blksize VALUE  设备 TFTP BLKSIZE 选项 (必须 >512)
-z, --time-cond TIME  基于时间条件的传输
-1, --tlsv1        使用 => TLSv1 (SSL)
    --tlsv1.0      使用 TLSv1.0 (SSL)
    --tlsv1.1      使用 TLSv1.1 (SSL)
    --tlsv1.2      使用 TLSv1.2 (SSL)
    --trace FILE    将 debug 信息写入指定的文件
    --trace-ascii FILE  类似 --trace 但使用16进度输出
    --trace-time    向 trace/verbose 输出添加时间戳
    --tr-encoding  请求压缩传输编码 (H)
-T, --upload-file FILE  将文件传输（上传）到指定位置
    --url URL      指定所使用的 URL
-B, --use-ascii    使用 ASCII/text 传输
-u, --user USER[:PASSWORD]  指定服务器认证用户名、密码
    --tlsuser USER  TLS 用户名
    --tlspassword STRING TLS 密码
    --tlsauthtype STRING  TLS 认证类型 (默认 SRP)
    --unix-socket FILE    通过这个 UNIX socket 域连接
-A, --user-agent STRING  要发送到服务器的 User-Agent (H)
-v, --verbose      显示详细操作信息
-V, --version      显示版本号并退出
-w, --write-out FORMAT  完成后输出什么
    --xattr        将元数据存储在扩展文件属性中
-q                .curlrc 如果作为第一个参数无效
```