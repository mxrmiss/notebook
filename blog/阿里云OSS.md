---
title: Linux安装阿里云OSS
date: 2022/3/17
categories: linux
tags: linux
index_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_45.jpg
banner_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_45.jpg
---



## 1. 安装

- 本次主要记载在ubuntu上面使用阿里云oss

- 进入阿里云官网，进入OSS页面，找到 Linux 的安装包下载选项
- 选择自己的路径解压
- 安装过程中会出现错误，解决如下

```
# First error
./oss-browser: error while loading shared libraries: libgconf-2.so.4: cannot open shared object file: No such file or directory

sudo apt-get install libgconf-2-4

# Second error
Gtk-Message: 09:11:20.756: Failed to load module "canberra-gtk-module"

sudo apt install libcanberra-gtk-module
```



## 2. 运行

- 直接 `./oss-browser` 即可，或者也可以将加载到环境变量中，打开oss的速度较慢，需要等待几秒钟才能够显示出来



  ## 3. Picgo配置文件(aliyun)

```json
{
  "picBed": { // 图床的配置
    "uploader": "aliyun", // 图床类型的选择，这里选择阿里云
    "aliyun": {
      "accessKeyId": "*********", // 图床的访问ID
      "accessKeySecret": "*********", // 图床的访问密钥
      "bucket": "bucket_name", // bucket名字
      "area": "oss-cn-beijing", // bucket地区
      "path": "", // bucket内部文件夹
      "customUrl": "", // 外网访问bucket域名
      "options": ""
    },
    "picgoPlugins": {} // 为插件预留
  },
  "picgoPlugins": {}
}
```

