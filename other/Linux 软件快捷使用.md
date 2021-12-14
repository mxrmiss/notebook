---
title: Linux软件快捷使用
date: 2021-12-14 14:06
categories: linux
tags: linux
top_img: https://herozql.oss-cn-beijing.aliyuncs.com/bing%E5%A3%81%E7%BA%B8/bing_8.jpg
---



## 安装

- 有时我们的linux软件下载时不是 .deb 的格式，无法直接按装并使用，我们需要解压下载的文件，并且对文件进行权限赋予

```
cd 下载目录
tar ... / unzip...
chmod +x %%.sh
./%%sh
```

- 注意最好不要将软件解压到需要root权限的目录下，会使以后的使用出现问题，保持权限最小，能使用就行

## 添加图标

- 找到软件相对应的图标所在的目录，记住目录位置
- 编写文件 `软件名.desktop` ，如下所示：

```
[Desktop Entry]
Name=软件名  //一般填可执行文件的名字
Exec=可执行文件位置  //在解压的安装包目录下
Type=Application
Icon=相对应图标位置
```

- 将刚才编辑好的文件复制到 `/usr/share/applications` 目录下

```
sudo cp typora.desktop /usr/share/applications/
```

## 命令行执行软件

- 我们希望在命令行输入我们的软件名，然后软件就能直接运行，这需要为我们的软件可执行文件配置环境变量，这里我只说在家目录下的  .profile 文件中添加环境变量，该环境变量的添加只对用户有影响。还有其他添加环境变量的方式，这里不再多述

  ```
  vim ~/.profile
  //再末尾另写一行
  export PATH="$HOME/******:$PATH"  //中间×部分填软件可执行文件所在的目录
  source ~/.profile
  ```

  

- 重启机器验证