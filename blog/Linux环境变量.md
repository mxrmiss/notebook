---
title: Linux环境变量
date: 2021/12/25
tags: linux
categories: linux
index_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_12.jpg
banner_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_12.jpg
---



## 1. 环境变量的种类

- 全局环境变量
- 局部环境变量

### 1.1 全局环境变量

- 全局环境变量对于所有的shell都适用（父shell与子shell）

- 全局环境变量可以使用命令`env`和`printenv`显示

  - 显示个别环境变量的值使用`printenv`命令，使用`env`命令会报错

  ![image-20211223202416786](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211223202416786.png)

### 1.2 局部环境变量

- Linux中没有专门查看局部环境变量的命令，只有使用set命令才能查看包括局部环境变量的所有环境变量

## 2. 设置自定义环境变量

### 2.1 设置局部用户自身定义变量

- 局部环境变量小写，系统环境变量大写
- 创建变量， 使用 $变量名 就能够引用环境变量

![image-20211223203630451](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211223203630451.png)

- 当创建的变量有空格的时候，创建变量时应该使用单引号来界定字符串的首和尾，没有单引号的话，bash shell会以为空格后下一个词是另一个要执行的命令

![image-20211223203950280](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211223203950280.png)

- 用户自定义设置的局部环境变量只能在当前shell下使用，在子shell与父shell中都不能使用

### 2.2 设置全局环境变量

- 设置全局环境变量的步骤只比设置局部环境变量多出了一步
- 创建局部变量后可以使用 `export 变量名` 来将局部变量转化为全局环境变量

![image-20211223205545608](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211223205545608.png)

- 删除全局环境变量使用 `unset 变量名` 命令
- **注意：子shell中修改父shell创建的全局环境变量不会影响父shell，只会对当前子shell产生影响**

## 3. 设置PATH环境变量

- 我们有时候自己创建了一个脚本，或者在网上下载了一个软件到本地使用，但是只能在当前目录下执行，我们想在所有的shell 以及其他地方中使用，这时候就应该将脚本所在目录的位置添加到PATH环境变量中.
- 查看PATH环境变量：`echo $PATH`
- Linux中通常有以下几个环境变量文件，我们需要合理地选择：

```
/etc/profile 
/etc/bash.bashrc
$HOME/.bash_profile 
$HOME/.bashrc 
$HOME/.bash_login 
$HOME/.profile 
```

- 在上边列举的几个环境变量文件中，/etc/profile和/etc/bash.bashrc 文件是设置全局环境变量的，其余的都是用户级的环境变量文件

### 3.1 区别

- 在使用层次方面说，/etc/profile，/etc/bash.bashrc 文件都是为所有用户使用的，其他变量文件都是为单个用户使用的
- 从文件关系来说，$HOME/.bash_login，$HOME/.bash_profile，$HOME/.profile 都是会查看引用 $HOME/.bashrc 的内容，而$HOME/.bashrc，/etc/profile 则会借鉴 /etc/bash.bashrc 的内容

- 从执行方式来说，/etc/profile ，$HOME/.bash_profile，$HOME/.bash_login，$HOME/.profile 修改后，都是重启后才能生效的，而/etc/bash.bashrc，$HOME/.bashrc 被修改后重新打开一个执行窗口便能生效，不需要重启
- 从读取顺序来讲，/etc/profile，/etc/bash.bashrc 重启后一定会被读取，而$HOME/.bash_profile，$HOME/.bash_login，$HOME/.profile 只要被读取一个就不会读取其他的

### 3.2 注意

- 各个操作系统包含的环境变量配置文件可能不一样，上述的配置文件只有部分会被使用
- 在配置全局环境变量的时候，/etc/profile会读取/etc/profile.d文件夹中的内容，所以我们最好在/etc/profile.d文件夹中创建 `xxx.sh` 文件，在该文件中写上我们想配置的全局环境变量路径，这样做便于维护

### 3.3 在配置文件中写环境变量路径的方法

- 环境变量的名称为PATH，引用时使用 $PATH，每个环境变量使用  “ ：” 隔开，形如：

```
export PATH="$HOME/XXXdir:$PATH"
export PATH="$PATH:$HOME/XXXdir"
```



***注：本文截图来自于：***

![image-20211223222825885](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211223222825885.png)

