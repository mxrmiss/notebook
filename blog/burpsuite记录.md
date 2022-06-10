---
title: burpsuite记录
date: 2022/1/14
categories: 渗透测试
tags: [渗透测试, tool]
index_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_37.jpg
banner_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_37.jpg
---



# 1. CA证书(开启https)

- 浏览器打开网址：http://burp，点击右侧的证书按钮开始下载

![image-20220114112120092](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220114112120092.png)

- 从浏览器的设置中找到管理证书选项，下图为微软Edge浏览器

![image-20220114112318625](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220114112318625.png)

- 从选型栏中点击受信任的根证书颁发机构，再点击导入

![image-20220114112550630](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220114112550630.png)

- 文件类型选所有文件，就能看到证书了，导入即可，然后就支持https了

![image-20220114112749859](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220114112749859.png)