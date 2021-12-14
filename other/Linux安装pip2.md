---
title: linux安装pip2
date: 2021/12/14
categories: linux
tags: [linux, pip]
top_img: https://herozql.oss-cn-beijing.aliyuncs.com/bing%E5%A3%81%E7%BA%B8/bing_5.jpg
---



# linux安装pip2

- 下载安装pip 2.7

```
curl  https://bootstrap.pypa.io/pip/2.7/get-pip.py  -o get-pip.py

python get-pip.py
```

- 设置软链接

```
sudo ln -s /home/kali/.local/bin/pip2 /usr/bin/pip2
```

- 安装一些包

```
pip install --upgrade setuptools
```

- 安装impactet (自己选择)

```
pip install impacket
```

