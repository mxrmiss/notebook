---
title: Debain 安装缺失固件
date: 2021/12/14
categories: linux
tags: [linux, debain]
top_img: https://herozql.oss-cn-beijing.aliyuncs.com/bing%E5%A3%81%E7%BA%B8/bing_4.jpg
---



## 描述

- 我们在安装debain以及基于debain的发行版时通常会遇到缺失固件的问题，系统还会提示我们将缺失的固件下载到U盘等可移动介质。
- 产生这个问题的原因是因为：

>除了需要设备驱动程序，有些硬件还要在使用之前加载 *固件(firmware)* 或 *微码(microcode)*。这对于网卡(特别是无线网卡)来说很常见，但有些 USB 设备甚至是硬盘控制器也需要加载固件。对于许多显卡，可以在毋须固件的情况下使用基本的功能，但使用高级功能就需要先安装合适的固件到系统中。
>
>根据 Debian GNU/Linux 项目的标准，大多数情况下这些固件属于 non-free，不能被包含在主发行版或安装系统里面。如果设备驱动程序被包含到发行版里面，并且 Debian GNU/Linux 可以合法地发布固件，它通常被单独地放置在仓库的 non-free 区里面。

- 根据官网的解释，我们可以手动下载并安装

## 安装

- 官方文档里面有这样一段话：

>然而，这并不意味着该硬件不能在安装时使用。从 Debian GNU/Linux 5.0 开始，`debian-installer` 支持从可移动的介质，比如 U 盘，加载固件或包含固件的软件包。参阅 [第 6.4 节 “加载缺失的固件”](https://www.debian.org/releases/jessie/amd64/ch06s04.html.zh-cn) 了解如何在安装时加载固件文件或软件包的的详细信息。
>
>假如 `debian-installer` 提示需要固件文件而您又没有该固件，或者不想装非自由的固件到系统上，您可以试着跳过固件加载。有些情况下驱动程序只是在特定情况下提示需要额外的固件，而这个设备在很多系统上可以不使用它就能工作(这通常出现在使用 tg3 驱动的网卡上)。

- 我们可以跳转到官方给我们的链接，链接页面中描述了如何安装介质：

>官方的 CD 映像不含商用的固件。这些固件通常是从移动介质，比如 U 盘来加载。另外，包含这些商用固件的非官方的 CD 位于 http://cdimage.debian.org/cdimage/unofficial/non-free/cd-including-firmware/。制作这种 U 盘(或者其他的介质，比如硬盘分区或者软盘)时，固件文件或软件包必须放置在文件系统的根目录或者名为 `/firmware` 的目录下。推荐使用 FAT 文件系统，因为在安装过程的早期阶段它肯定能被支持。
>
>大多数固件的压缩包和 zip 文件可以从这里获得：
>
>- http://cdimage.debian.org/cdimage/unofficial/non-free/firmware/
>
>下载对应版本的压缩包或 zip 文件，然后解压到介质的文件系统里面。
>
>如果您需要的固件没有包含在压缩包里面，可以从档案库(non-free 部分)下载特定的固件。以下概要列出大多数固件软件包，但不保证完整，有些还是非固件软件包：
>
>- http://packages.debian.org/search?keywords=firmware
>
>也可以复制单独的固件文件到介质。要求不严的固件还可以从已经安装的系统或硬件制造商那里获得。

### 注意

- 在上面的链接中，我们可以先下载可能包含我们需要的[**固件软件包的压缩包，**](http://cdimage.debian.org/cdimage/unofficial/non-free/firmware/)如果压缩包里面没有我们想要的再去**[单独搜索下载](http://packages.debian.org/search?keywords=firmware)**

## 演示案例

### debain 蓝牙适配器问题

- 刚安装完debain不久，使用蓝牙时一直扫描不出来设备，并且显示没有找到**蓝牙适配器**
- 首先安装蓝牙管理软件 blueman 和 bluetooth
- 安装完成后还是发现提示没有找到蓝牙适配器，通过google我们可以知道是缺少了相关的驱动，的确，在安装debain的时候就提示了缺少部分固件，需要从介质中读取
- 进入固件压缩包下载页面：http://cdimage.debian.org/cdimage/unofficial/non-free/firmware/

![image-20211214182615129](https://herozql.oss-cn-beijing.aliyuncs.com/mainimage-20211214182615129.png)

![image-20211214183237845](https://herozql.oss-cn-beijing.aliyuncs.com/mainimage-20211214183237845.png)

- 解压

![image-20211214183401637](https://herozql.oss-cn-beijing.aliyuncs.com/mainimage-20211214183401637.png)

- 我们缺失的是这个安装包 `firmware-atheros_20210818-1_all.deb`

```
sudo dpkg -i firmware-atheros_20210818-1_all.deb
```

- 当然，我们也可以全部安装，总共就250MB，不大

```
sudo dpkg -i ./*
```

- 有些和我们当前系统架构不符合的包会无法安装，省略就好