# SMB 信息收集

### 1、扫描

```
smbmap -H 10.129.1.225
nmap -p 445 --script=smb-enum-shares 10.129.1.225
```

### 2、登录

```
smbclient //10.129.1.225/general -U " " -N
-U 用户名，不写为空
-N 不用密码登录
```

### 3、命令

```
get xxx    //下载文件
put xxx    //上传文件
```

