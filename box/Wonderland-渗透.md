# Wonderland



##### **tryhackme的渗透测试机器之一**





- 端口扫描

![image-20210918182421147](http://cdn.mxrblog.cn/image-20210918182421147.png)

- 查看80端口， 仅仅一张图片

![image-20210918182505779](http://cdn.mxrblog.cn/image-20210918182505779.png)

- 目录扫描（本次试用多个扫描工具，其中dirsearch最快），然而并没有发现什么有意义的东西

![image-20210918182754260](http://cdn.mxrblog.cn/image-20210918182754260.png)

- 漏洞扫描，使用了nmap以及kali自带的漏洞数据库searchsploit，发现openssh漏洞可能可以被利用，暂且搁置一旁

![image-20210918183118451](http://cdn.mxrblog.cn/image-20210918183118451.png)

![image-20210918183156400](http://cdn.mxrblog.cn/image-20210918183156400.png)

- 没有什么线索，将web上面唯一的图片下载下来，查看是否有什么线索，使用隐写工具steghide查看是否有东西藏在图片中
- 注意，第一次使用这个工具的时候我发生了一些错误，可能是我没有设置一些参数，导致了png类型的照片无法被分解，但是当我重新下载图片，并使图片的格式为jpg时便可以了

![image-20210918184426647](http://cdn.mxrblog.cn/image-20210918184426647.png)

- 猜想，可能这个中间加有空格的rabbit其实是一个文件目录，进行查看：

![image-20210919012200048](http://cdn.mxrblog.cn/image-20210919012200048.png)

- 提示我们使用ssh进行登录（首先我想到了密码可能藏在了HTML网页，并且也猜到了使用ssh进行登录，但是我却并没有更深入地去证实我的想法，事实证明应该更大胆的去尝试）

![image-20210919013027153](http://cdn.mxrblog.cn/image-20210919013027153.png)

![image-20210919014348646](http://cdn.mxrblog.cn/image-20210919014348646.png)



- 发现alice可以以rabbit的身份运行那个python文件，并且在文件中还导入了random包

![image-20210919022824444](http://cdn.mxrblog.cn/image-20210919022824444.png)



- 我们可以在与python文件同目录下建立一个同为random.py文件名的py文件，然后当walus_and_the_carpenter.py运行时就会优先调用和其同一目录下的文件

![image-20210919022650571](http://cdn.mxrblog.cn/image-20210919022650571.png)



- 在rabbit用户文件夹下我发现了一个二进制文件，将其传至攻击机器上面进行字符化

![image-20210919092624637](http://cdn.mxrblog.cn/image-20210919092624637.png)

![image-20210919092704074](http://cdn.mxrblog.cn/image-20210919092704074.png)

- 发现这个二进制文件在没有制定绝对路径的情况下执行date

![image-20210919093103686](C:\Users\herol\AppData\Roaming\Typora\typora-user-images\image-20210919093103686.png)

- 这时我们可以将某路径添加到环境变量中去，然后在该路径中添加具有提权功能的date文件

![image-20210919094235241](http://cdn.mxrblog.cn/image-20210919094235241.png)

- 添加完后对文件给予执行权限，然后执行文件，我们会发现我们已经切换到了hatter用户

![image-20210919101331329](http://cdn.mxrblog.cn/image-20210919101331329.png)

- 我们得到了hatter的密码，尝试ssh登录用户

![image-20210919101525835](http://cdn.mxrblog.cn/image-20210919101525835.png)



- 使用命令查看是否某些东西具有cap_setuid+ep功能集

`getcap -r / 2>/dev/null`

![image-20210919104635589](http://cdn.mxrblog.cn/image-20210919104635589.png)

- 使用以下命令可以提权到root

```
/usr/bin/perl -e 'use POSIX (setuid); POSIX::setuid(0); exec "/bin/bash";'
```

![image-20210919104832117](http://cdn.mxrblog.cn/image-20210919104832117.png)

- 获得root权限之后再拿到所有的flag

![image-20210919105330137](http://cdn.mxrblog.cn/image-20210919105330137.png)

