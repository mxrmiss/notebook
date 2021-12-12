# powershell

#### 软链接

- 设置软连接

```
New-Item -Path [链接地址] -ItemType SymbolicLink -Value [目标文件地址]
```

- 删除软连接

```
(Get-Item [被链接的地址]).Delete()
```

