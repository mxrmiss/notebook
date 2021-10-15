# 关于ssh的常见问题

### 参数

```
-p 指定端口
-i 指定密匙

```

### 出现报错

**1、**

![image-20211014005045566](http://cdn.mxrblog.cn/image-20211014005045566.png)

- 解决方法：

![image-20211014003304538](http://cdn.mxrblog.cn/image-20211014003304538.png)

The best resolution for these failures is to upgrade/configure the server to not use deprecated algorithms. If that is not possible, you can force the client to re-enable the `diffie-hellman-group1-sha1` key exchange algorithm with the `-oKexAlgorithms=+diffie-hellman-group1-sha1` option on the command-line:

```
ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 user@host
```

or in the `~/.ssh/config` file:

```
Host somehost.example.org
    KexAlgorithms +diffie-hellman-group1-sha1
    
//注意：这里的两行代码分开写，另外Host后面的网址不要用IP地址代替（如果dns无法解析就在hosts文件中添加即可）    
```

**2、**

