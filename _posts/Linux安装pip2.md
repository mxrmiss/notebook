---
title: Linux安装pip2
date: 2020-09-14 14:06
swiper: false # 将改文章放入轮播图中
swiperImg: '/medias/1.jpg' # 该文章在轮播图中的图片，可以是本地目录下图片也可以是http://xxx图片
img: 'http://cdn.mxrblog.cn/bing_8.jpg' # 该文章图片，可以是本地目录下图片也可以是http://xxx图片
categories: linux
tags: [linux, pip, python]
top: false
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

