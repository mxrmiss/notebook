---
title: powershell魔改
date: 2021/12/18
categories: windows
tags: [windows, powershell, shell]
top_img: https://mxrblog.cn/bing/bing_10.jpg
cover: https://mxrblog.cn/bing/bing_10.jpg
---



## 安装 nerd fonts

- Github搜索nerd fonts ，并下载名为 hack.zip 的压缩包

![image-20211217192217556](https://mxrblog.cn/main/image-20211217192217556.png)

![image-20211217192240533](https://mxrblog.cn/main/image-20211217192240533.png)

- 选择压缩包里的 `Hack Bold Italic Nerd Font Complete Mono Windows Compatible.ttf` 文件

![image-20211217192426419](https://mxrblog.cn/main/image-20211217192426419.png)

- 点击安装

![image-20211217192601648](https://mxrblog.cn/main/image-20211217192601648.png)

## 终端界面初步设置

### 设置默认终端

- 返回桌面，右击打开powershell，点击设置

![image-20211217193057281](https://mxrblog.cn/main/image-20211217193057281.png)

![image-20211217192954988](https://mxrblog.cn/main/image-20211217192954988.png)

- 设置默认终端应用程序 Windows Terminal

![image-20211217193332586](https://mxrblog.cn/main/image-20211217193332586.png)

### 设置外观

- 设置外观为深色，并显示亚巧克力效果

![image-20211217193747966](https://mxrblog.cn/main/image-20211217193747966.png)

- 设置默认值，点击外观，设置文本配色方案为 `One Half Dark`

![image-20211217194147093](https://mxrblog.cn/main/image-20211217194147093.png)

- 设置文本字体为 `Hack NF`

![image-20211217194520679](https://mxrblog.cn/main/image-20211217194520679.png)

- 接着在下面设置，启动亚巧克力效果， 不透明度调到35%

![image-20211217195553148](https://mxrblog.cn/main/image-20211217195553148.png)

- 重启终端，然后界面是这样子的

![image-20211217195230793](https://mxrblog.cn/main/image-20211217195230793.png)

![image-20211217200318161](https://mxrblog.cn/main/image-20211217200318161.png)

### 切换为powershell

- 打开 Mircrosoft Store, 下载powershell

![image-20211217195806484](https://mxrblog.cn/main/image-20211217195806484.png)

- 打开终端，设置默认启动文件为 powershell

![image-20211217200046620](https://mxrblog.cn/main/image-20211217200046620.png)

- 设置完成后我们使用命令显示是这样子的

![image-20211217200202806](https://mxrblog.cn/main/image-20211217200202806.png)

### 改变终端背景颜色

- 点击 **打开json文件** 选项

![image-20211217200821150](https://mxrblog.cn/main/image-20211217200821150.png)

- Ctrl + F ,搜索One Hack, 定位one half dark 主题的位置

![image-20211217201358446](https://mxrblog.cn/main/image-20211217201358446.png)

- 复制主题并且重新命名它

- 在下图箭头处按下复制键，并且设置为红色下划线中的内容

![image-20211217202028291](https://mxrblog.cn/main/image-20211217202028291.png)

- 接着修改背景颜色

![image-20211217202252272](https://mxrblog.cn/main/image-20211217202252272.png)

- 返回终端， 设置 默认值—>外观—>文本 ，设置配色方案为 `One Half Dark (modded)`

![image-20211217202638983](https://mxrblog.cn/main/image-20211217202638983.png)

- 重启后是这样子的

![image-20211217203215462](https://mxrblog.cn/main/image-20211217203215462.png)

## SCOOP（命令行安装程序）

### 安装scoop

![image-20211217203659134](https://mxrblog.cn/main/image-20211217203659134.png)

- 中国大陆无法按照上面图示的命令安装，可以使用 Gitee 的镜像安装
- ***[安装链接](https://gitee.com/glsnames/scoop-installer#installation)*** :fire:

>- scoop 安装配置要求
>
>  - Windows 版本不低于 Windows 7
>
>  - Windows 中的 PowerShell 版本不低于 PowerShell 3
>
>  - 你能 **正常、快速** 的访问 GitHub 并下载上面的资源
>
>  - 你的 Windows 用户名为英文（Windows 用户环境变量中路径值不支持中文字符）
>
>- 安装命令
>
>```
>Invoke-Expression (New-Object System.Net.WebClient).DownloadString('https://gitee.com/glsnames/scoop-installer/raw/master/bin/install.ps1')
>
># or shorter
>iwr -useb gitee.com/glsnames/scoop-installer/raw/master/bin/install.ps1 | iex
>```

![image-20211217214227953](https://mxrblog.cn/main/image-20211217214227953.png)

### scoop 安装软件

***[链接：scoop相关介绍](https://sspai.com/post/52496)***

- 分别执行以下命令

```
scoop install curl sudo jq
scoop install aria2
curl 'https://api.inkdrop.app/' | jq .
scoop install neovim
```

## 安装git

- 官网下载安装就好

## 配置用户文件，并设置命令别名

- 在当前用户目录下配置powershell文件

```
mkdir .config/powershell
nvim .config/powershell/user_profile.ps1
```

- 在配置文件中添加以下内容，也可自行修改

```
# Alias
set-Alias vim nvim
set-Alias ll ls
set-Alias grep findstr
# 注意是你的git安装目录
set-Alias tig 'C:\Apps\Git\usr\bin\tig.exe'
set-Alias less 'C:\Apps\Git\usr\bin\less.exe'
```

- 修改环境变量
- 使用命令：`nvim $PROFILE.CurrentUserCurrentHost`
- 添加以下内容

```
. env:USERPROFILE\.config\powershell\user_profile.ps1
```

- 打开新的标签页，发现可以使用别名了

![image-20211217225950144](https://mxrblog.cn/main/image-20211217225950144.png)

### Scoop 错误警告

- 在使用Scoop的时候，我出现了一些的错误并且已经修正，记录下来用来供大家参考

#### 无法找到Scoop

- 我们在下载完成Scoop后一开始使用是正常的，可是在某一时刻突然就无法使用了，这个时候我们可以为当前用户添加环境变量

- 按徽标键，搜索环境变量，在**用户变量名中**编辑名为path的栏目，向其中添加环境变量 `%USERPROFILE%\scoop\shims`

![image-20211217235518816](https://mxrblog.cn/main/image-20211217235518816.png)

![image-20211217235955186](https://mxrblog.cn/main/image-20211217235955186.png)

#### 其他错误

- 出现错误：

>Get-ChildItem: C:\Users\herol\scoop\apps\scoop\current\lib\commands.ps1:3
>Line |
>   3 |          + (Get-ChildItem "$scoopdir\shims") `
>     |             ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
>     | Cannot find path 'C:\Users\herol\scoop\shims\shims' because it does not exist.

- 修改 `%USERPROFILE%\scoop\apps\scoop\current\lib\commands.ps1:3`
- 删除第三行 \shims 字段

![image-20211218000619930](https://mxrblog.cn/main/image-20211218000619930.png)

## Oh My Posh

- oh my posh 可以自定义修改终端主题
- 终端输入以下命令

```
Install-Module posh-git -Scope CurrentUser -Force
Install-Module oh-my-posh -Scope CurrentUser -Force
```

- 在用户目录下执行以下命令

```
cd .config\powershell\
vim .\user_profile.ps1
```

- 输入以下内容

```
# Prompt
Import-Module posh-git
Import-Module oh-my-posh
Set-PoshPrompt Paradox
```

![image-20211218003650968](https://mxrblog.cn/main/image-20211218003650968.png)

- 重新打开终端，然后回自动加载主题，若是出现找不到 oh-my-posh的错误，把它所在的路径修改为纯英文

![image-20211218003800012](https://mxrblog.cn/main/image-20211218003800012.png)



###未完待续。。。
