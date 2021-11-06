# Node

**Node是hackthebox上中等难度的一台靶机**

- nmap扫描端口
- 22和3000端口已经打开

![image-20211031232805664](http://cdn.mxrblog.cn/image-20211031232805664.png)

- 浏览器打开10.10.10.58：3000端口

![image-20211031232933794](http://cdn.mxrblog.cn/image-20211031232933794.png)

- 扫描目录，但实际并没有什么结果

![image-20211031235212125](http://cdn.mxrblog.cn/image-20211031235212125.png)

- 查看网页源代码
- 分别查看链接

![image-20211101000331006](http://cdn.mxrblog.cn/image-20211101000331006.png)

- 进入上图显示的链接后会发现

![image-20211101000429594](http://cdn.mxrblog.cn/image-20211101000429594.png)

- 我们可以找到哈希值在http://10.10.10.58:3000/api/users/

![image-20211031235404465](http://cdn.mxrblog.cn/image-20211031235404465.png)

- 使用免费的哈希破解网站来破解密码

![image-20211101000522300](http://cdn.mxrblog.cn/image-20211101000522300.png)

- 得到结果是manchester
- 使用得到的账户名与密码进行登录

![image-20211101000831999](http://cdn.mxrblog.cn/image-20211101000831999.png)

- 下载文件

![image-20211101002405137](http://cdn.mxrblog.cn/image-20211101002405137.png)

- 查看文件

![image-20211101002429741](http://cdn.mxrblog.cn/image-20211101002429741.png)

- 是base64编码的，使用相应工具进行解码

`cat myplace.backup | base64 -d > myplace.backup.2`

- 发现文件是一个压缩包

![image-20211102141658386](http://cdn.mxrblog.cn/image-20211102141658386.png)

- 并且这个压缩包需要密码

![image-20211102141809116](http://cdn.mxrblog.cn/image-20211102141809116.png)

- 使用fcrackzip工具进行解码

![image-20211102142124006](http://cdn.mxrblog.cn/image-20211102142124006.png)

- 发现一个账号密码

![image-20211102142911820](http://cdn.mxrblog.cn/image-20211102142911820.png)

- 使用ssh进行登录

![image-20211102143215688](http://cdn.mxrblog.cn/image-20211102143215688.png)

- 通过查看版本我们得知这个Linux具有较多的漏洞

![image-20211102143614569](http://cdn.mxrblog.cn/image-20211102143614569.png)

- 查看相对应版本可以利用的工具

- 找到相对应的本地权限提升工具

![image-20211102144134317](http://cdn.mxrblog.cn/image-20211102144134317.png)

- 将该工具传到靶机上进行编译和赋予执行权限，运行后我们就会有root权限了

![image-20211102145027141](http://cdn.mxrblog.cn/image-20211102145027141.png)