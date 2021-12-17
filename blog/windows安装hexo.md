---
title: windows安装hexo
date: 2021-12-13 
categories: blog
tags: [blog, node.js]
top_img: https:/mxrblog.cn/bing/bing_3.jpg
cover: https:/mxrblog.cn/bing/bing_3.jpg
---



# windows安装hexo

## 安装node.js

### 安装

1. 下载

2. 查看版本

```
node -v
```

### 换源

1. 换源

```
npm config set registry https://registry.npm.taobao.org
```

2. 下载express

```
npm install express
```

3. 查看换源情况

```
npm info express
```

![image-20211212141925797](C:/Users/herol/OneDrive/%E6%96%87%E6%A1%A3/GitHub/Figure_bed/main/image-20211212141925797.png)

- 此时换源成功



## 安装hexo

```
npm install -g hexo-cli
```

