---
title: bat批处理
date: 2020-09-14 14:06
swiper: false # 将改文章放入轮播图中
swiperImg: '/medias/1.jpg' # 该文章在轮播图中的图片，可以是本地目录下图片也可以是http://xxx图片
img: 'http://cdn.mxrblog.cn/bing_9.jpg' # 该文章图片，可以是本地目录下图片也可以是http://xxx图片
categories: windows
tags: powershell
top: false
---



### ***Get-ChildItem***

- 查找文件, -path代表路径, -include被包含的文件, -recurse递归匹配, -file查找类型为文件, -ErrorAction SilentlyContinue不报错,继续查询

```
Get-ChildItem -path C:\ -include *interesting-file.txt* -recurse -file -ErrorAction SilentlyContinue
```

---

### ***Get-Content***

- 查看文件内容, 被查看的文件用双引号括起来

![image-20210722181951084](http://cdn.mxrblog.cn/image-20210722181951084.png)

---

### ***Where-Object***

- 对文件进行筛选

***measure***

- 对文件进行计数

![image-20210723074723182](http://cdn.mxrblog.cn/image-20210723074723182.png)

![image-20210722223957073](http://cdn.mxrblog.cn/image-20210722223957073.png)

---

### ***Get-FileHash***

```
Get-FileHash 文件路径 -Algorithm Hash类型| Format-List
```

> **说明**
>
> - `文件路径`：指文件的全路径，需要注意的是如果需要校验的文件路径比较复杂，例如路径中包含空格、括号等特殊符号，则需要在路径前后加上英文双引号。
> - `-Algorithm`：可选项，如果不带-Algorithm参数，也就是不指定Hash类型，那么默认就使用是SHA256算法。
> - `Hash类型`：Hash算法类型，包括：SHA1、SHA256、SHA384、SHA512、MACTripleDES、MD5、RIPEMD160。
> - `Format-List`：格式化输出



---

### ***Get-Location***

- 获取当前工作目录



---



### ***Test-Path***

- 检查路径是否存在

- 命令后面的路径要用双引号括起来



---

### ***Invoke-WebRequest***

- 向 Web 服务器发出请求

---

### ***[certutil](https://zhuanlan.zhihu.com/p/107819644)***

- 对文件进行解码

![image-20210723071445332](http://cdn.mxrblog.cn/image-20210723071445332.png)

---

### ***Get-Localuser***

- 查看用户

![image-20210723072704452](http://cdn.mxrblog.cn/image-20210723072704452.png)

---

### ***Get-Localgroup***

- 查看用户分组

---

### ***Get-NetTCPConnection***

- 查看端口连接情况
- 关于端口的情况可以使用 get-command *get-net*,从里面找到
- 如下,查看有多少端口被监听

![image-20210723082312323](http://cdn.mxrblog.cn/image-20210723082312323.png)



---

### ***Get-Hotfix***

- 查看有多少补丁
- 如下所示,查看布丁ID为某值的某日期

![image-20210723083845728](http://cdn.mxrblog.cn/image-20210723083845728.png)

----

### ***paramrters***

- 参数
- 查看命令中所包含的参数可以使用

```
(get-command  xxx_cmdlet).parameters
```

---

### ***Get-process***

- 查看所有正在运行的进程

---

 

### ***Select-String***

- 查找包含某些字符的文件,如下所示:

```
Get-ChildItem -Path C:\ -file -Recurse | Select-String -Pattern API_KEY
```

---

### ***Get-Acl***

- 查看文件权限及拥有者
- 如下所示,查看C:\的拥有者

![image-20210723100151865](http://cdn.mxrblog.cn/image-20210723100151865.png)

