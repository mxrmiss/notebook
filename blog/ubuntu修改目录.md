---
title: ubuntu修改目录
date: 2022/3/17
categories: linux
tags: [linux, ubuntu]
index_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_46.jpg
banner_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_46.jpg
---



## 1. 修改目录为英文

- 使用中文安装ubuntu时，使用终端会发现部分目录是中文的，不方便我们使用，这里记录一下将中文目录变为英文目录的方法以及避坑

1. 使用命令

  ` export LANG=en_US`

`xdg-user-dirs-gtk-update`

> 注意：如果提示报错，说不存在上面的东西，可以手动安装

2. 在弹出的窗口中询问是否将目录转化为英文路径，同意并关闭

3. 使用命令

   `epxort LANG=zh_CN`

4. 重启`Ubuntu`，下次进入系统后，会提示是否把目录转化为中文，选择不许并选择不再提示，并取消修改



## 2. 找回消失的目录

- 有时候我们切换系统语言后会导致目录出现问题，可能会使桌面目录消失，这样会导致所有的～/home 下的目录全部都显示在桌面上
-  编辑配置文件 `gedit ~/.config/user-dirs.dirs`
- 修改内容：

```
XDG_DESKTOP_DIR="$HOME/Desktop"    # 主要是这段不对导致问题出现

XDG_DOWNLOAD_DIR="$HOME/Downloads"

XDG_TEMPLATES_DIR="$HOME/Templates"

XDG_PUBLICSHARE_DIR="$HOME/Public"

XDG_DOCUMENTS_DIR="$HOME/Documents"

XDG_MUSIC_DIR="$HOME/Music"

XDG_PICTURES_DIR="$HOME/Pictures"

XDG_VIDEOS_DIR="$HOME/Videos"
```



### 参考文献

[本文参考于：https://www.linuxidc.com/Linux/2012-04/58648.htm](https://www.linuxidc.com/Linux/2012-04/58648.htm)