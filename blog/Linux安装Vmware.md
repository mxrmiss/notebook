---
title: linux安装Vmware Workstation
date: 2022/5/5
categories: linux
tags: [linux, ubuntu]
top_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_58.jpg
cover: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_58.jpg
---



## 1. 安装

- **这里以VMware Workstation 16.2.3 在ubuntu上面安装为例**

- 在vmware workstation官网下载linux版本的进行安装
- chmod +x VMware-Workstation-Full-16.2.3-xxxxxx.bundle
- ./VMware-Workstation-Full-16.2.3-xxxxxx.bundle



## 2 .安装错误

- 在linux上面安装vmware可能会出现一些错误，可能会弹出一个“**VMware Kernel Module Updater**” 的弹窗，让你重新编译一些文件
- 查看vmware的版本

```
╰─$ vmware --version
[AppLoader] Use shipped Linux kernel AIO access library.
An up-to-date "libaio" or "libaio1" package from your system is preferred.
VMware Workstation 16.2.3 build-19376536
```

- 创建一个名为 vmware.sh 的文件，并在文件中添加以下的内容：

```
#!/bin/bash

VMWARE_VERSION=workstation-16.2.3
TMP_FOLDER=/tmp/patch-vmware
rm -fdr $TMP_FOLDER
mkdir -p $TMP_FOLDER
cd $TMP_FOLDER
git clone https://github.com/mkubecek/vmware-host-modules.git
cd $TMP_FOLDER/vmware-host-modules
git checkout $VMWARE_VERSION
git fetch
make
sudo make install
sudo rm /usr/lib/vmware/lib/libz.so.1/libz.so.1
sudo ln -s /lib/x86_64-linux-gnu/libz.so.1
/usr/lib/vmware/lib/libz.so.1/libz.so.1
sudo /etc/init.d/vmware restart
```

- **注意**：

>1. 上面的shell脚本中包含了github仓库，如果因为网络原因，可以使用proxy。或可以将仓库同步至gitee中，注意同时要修改脚本中的一些内容
>2. VMWARE_VERSION这个变量的值需要填自己的vmware版本号，在之前我们已经查过了

- 对vmware.sh文件进行编译并运行

```
chmod +x vmware.sh

./vmware.sh
```

- 查看过程，如果出现没有 make 命令的错误，我们需要下载make这个命令

```
sudo apt install make
```

- 然后我们再进行安装就不会出现错误了



## 文章参考：https://unixcop.com/workstation-failed-install-vmware-kernel-module/