---
title: Linux shell(5)-呈现数据
date: 2022/1/12
categories: linux
tags: [linux, shell]
index_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_32.jpg
banner_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_32.jpg
---

# 1. 理解输入输出

## 1.1 标准文件描述符

- 文件描述符表示对文件对象的描述，Linux有9个文件描述符，bash保留了前三个文件描述符（0、1和2）

>0 代表标准输入，如键盘
>
>1 代表标准输出，如显示屏
>
>2 代表标准错误，一般也是显示屏

![image-20220112113940112](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220112113940112.png)

## 1.2 重定向错误

### 1.2.1 只重定向错误

- STDERR 文件描述符为2，该值必须紧紧地放在重定向符号前，否则不会工作。

```
$ ls -al test badtest test2 2> test5 
-rw-rw-r-- 1 rich rich 158 2014-10-16 11:32 test2 
$ cat test5 
ls: cannot access test: No such file or directory 
ls: cannot access badtest: No such file or directory
```

### 1.2.2 重定向错误与数据

- 如果想重定向错误和正常输出，必须用两个重定向符号。

```
$ ls -al test test2 test3 badtest 2> test6 1> test7 
$ cat test6 
ls: cannot access test: No such file or directory 
ls: cannot access badtest: No such file or directory 
$ cat test7 
-rw-rw-r-- 1 rich rich 158 2014-10-16 11:32 test2 
-rw-rw-r-- 1 rich rich 0 2014-10-16 11:33 test3 
```

- 当使用&>符时，命令生成的所有输出都会发送到同一位置，包括数据和错误
- 为了避免错误信息散落在输出文件中，相较于标准输出，bash shell自动赋予了错误消息更高的优先级。这样你能够集中浏览错误信息了

## 2. 在脚本中重定向输出

>临时重定向
>
>永久重定向

### 2.1 临时重定向

- 在重定向**到文件描述符**时，你 必须在文件描述符数字之前加一个&：

```
echo "This is an error message" >&2 
```

- 案例：

```
$ cat test8 
#!/bin/bash 
# testing STDERR messages 
echo "This is an error" >&2 
echo "This is normal output" 
$
$ ./test8 2> test9 
This is normal output 
$ cat test9 
This is an error 
$ 
```

## 2.2 永久重定向

- 如果脚本中有大量数据需要重定向，那重定向每个echo语句就会很烦琐。可以用exec命令告诉shell在脚本执行期间重定向某个特定文件描述符

- 案例：

```
$ cat test11 
#!/bin/bash 
# redirecting output to different locations 
exec 2>testerror 
echo "This is the start of the script" 
echo "now redirecting all output to another location" 
exec 1>testout 
echo "This output should go to the testout file" 
echo "but this should go to the testerror file" >&2 
$ 
$ ./test11 
This is the start of the script 
now redirecting all output to another location 
$ cat testout 
This output should go to the testout file 
$ cat testerror 
but this should go to the testerror file 
```

# 3. 在脚本中重定向输入

- exec命令允许你将STDIN重定向到Linux系统上的文件中：

```
exec 0< testfile 
```

- 案例：

```
$ cat test12 
#!/bin/bash 
# redirecting file input 
exec 0< testfile 
count=1 
while read line 
do 
 echo "Line #$count: $line" 
 count=$[ $count + 1 ] 
done 
$ ./test12 
Line #1: This is the first line. 
Line #2: This is the second line. 
Line #3: This is the third line.
```

- 将STDIN重定向到文件后， 当read命令试图从STDIN读入数据时，它会到文件去取数据，而不是键盘。

# 4. 创建自己的重定向

## 4.1 创建

- 在shell 中最多可以有9个打开的文件描述符。其他6个从3~8的文件描述符均可用作输入或输出**重定向**。

- 案例：

```
$ cat test14 
#!/bin/bash 
# storing STDOUT, then coming back to it 
exec 3>&1 
exec 1>test14out 
echo "This should store in the output file" 
echo "along with this line." 
exec 1>&3 
echo "Now things should be back to normal" 
$ 
$ ./test14 
Now things should be back to normal 
$ cat test14out 
This should store in the output file 
along with this line.
```

- 在上面的案例中，演示了在脚本中临时重定向输出，然后恢复默认输出设置的常用方法。相当于我们C语言编程中学习的变量交换数值的那么一个过程

## 4.2 关闭

- 要关闭文件描述符，将它重定向到特殊符号&-。见案例1.

```
exec 3>&-
```

- 在关闭文件描述符时还要注意另一件事。如果随后你在脚本中打开了同一个输出文件，shell 会用一个新文件来替换已有文件。这意味着如果你输出数据，它就会覆盖已有文件。见案例2。
- ​	案例1：

```
$ cat badtest 
#!/bin/bash 
# testing closing file descriptors 
exec 3> test17file 
echo "This is a test line of data" >&3 
exec 3>&- 
echo "This won't work" >&3 
$ ./badtest 
./badtest: 3: Bad file descriptor 
```

- 案例2：

```
$ cat test17 
#!/bin/bash 
# testing closing file descriptors 
exec 3> test17file 
echo "This is a test line of data" >&3 
exec 3>&- 
cat test17file 
exec 3> test17file 
echo "This'll be bad" >&3 
$ ./test17 
This is a test line of data 
$ cat test17file 
This'll be bad 
```

# 5. 列出打开的文件描述符

- 因为它会向 非系统管理员用户提供Linux系统的信息，所以许多Linux系统隐藏了该命令
- 在很多Linux系统中（如Fedora），lsof命令位于/usr/sbin目录。要想以普通用户账户来运行 它，必须通过全路径名来引用：`$ /usr/sbin/lsof`

- 这里不多加描述，需要的自行百度

# 6. 阻止命令输出

- 可以将STDERR重定向到一个叫作null文件的特殊文件。null文件跟它的名 字很像，文件里什么都没有。shell输出到null文件的任何数据都不会保存，全部都被丢掉了。
-  在Linux系统上null文件的标准位置是 `/dev/null`。你重定向到该位置的任何数据都会被丢掉， 不会显示
- 由于/dev/null文件不含有任何内容，程序员 通常用它来快速清除现有文件中的数据，而不用先删除文件再重新创建。

```
$ cat testfile 
This is the first line. 
This is the second line. 
This is the third line. 
$ cat /dev/null > testfile 
$ cat testfile 
```

# 7. 创建临时文件

- 系统上的任何用户账户都有权限在读写/tmp目录中的文件
- 系统在启动时自动删除/tmp目录的所有文件。

## 7.1 创建本地临时文件

- mktemp会在本地目录中创建一个文件。
- 要用mktemp命令在本地目录中创建一 个临时文件，你只要指定一个文件名模板就行了。模板可以包含任意文本文件名，在文件名末尾 加上6个X就行了。
- mktemp命令会用6个字符码替换这6个X，从而保证文件名在目录中是唯一的。

```
$ mktemp testing.XXXXXX 
$ ls -al testing* 
-rw------- 1 rich rich 0 Oct 17 21:30 testing.UfIi13
```

## 7.2 在/tmp 目录创建临时文件

- -t 选项会强制mktemp命令来在系统的临时目录来创建该文件。在用这个特性时，mktemp命 令会返回用来创建临时文件的全路径，而不是只有文件名。

```
$ mktemp -t test.XXXXXX 
/tmp/test.xG3374 
$ ls -al /tmp/test* 
-rw------- 1 rich rich 0 2014-10-29 18:41 /tmp/test.xG3374 
```

- 案例：

```
$ cat test20 
#!/bin/bash 
# creating a temp file in /tmp 
tempfile=$(mktemp -t tmp.XXXXXX) 
echo "This is a test file." > $tempfile 
echo "This is the second line of the test." >> $tempfile 
echo "The temp file is located at: $tempfile" 
cat $tempfile 
rm -f $tempfile 
$ ./test20 
The temp file is located at: /tmp/tmp.Ma3390 
This is a test file. 
This is the second line of the test.
```

## 7.3 创建临时目录

- -d 选项告诉mktemp命令来创建一个临时目录而不是临时文件。

- 案例：

```
$ cat test21 
#!/bin/bash 
# using a temporary directory 
tempdir=$(mktemp -d dir.XXXXXX) 
cd $tempdir 
tempfile1=$(mktemp temp.XXXXXX) 
tempfile2=$(mktemp temp.XXXXXX) 
exec 7> $tempfile1 
exec 8> $tempfile2 
echo "Sending data to directory $tempdir" 
echo "This is a test line of data for $tempfile1" >&7 
echo "This is a test line of data for $tempfile2" >&8 
$ ./test21 
Sending data to directory dir.ouT8S8 
$ ls -al
total 72 
drwxr-xr-x 3 rich rich 4096 Oct 17 22:20 ./ 
drwxr-xr-x 9 rich rich 4096 Oct 17 09:44 ../ 
drwx------ 2 rich rich 4096 Oct 17 22:20 dir.ouT8S8/ 
-rwxr--r-- 1 rich rich 338 Oct 17 22:20 test21* 
$ cd dir.ouT8S8 
[dir.ouT8S8]$ ls -al 
total 16 
drwx------ 2 rich rich 4096 Oct 17 22:20 ./ 
drwxr-xr-x 3 rich rich 4096 Oct 17 22:20 ../ 
-rw------- 1 rich rich 44 Oct 17 22:20 temp.N5F3O6 
-rw------- 1 rich rich 44 Oct 17 22:20 temp.SQslb7 
[dir.ouT8S8]$ cat temp.N5F3O6 
This is a test line of data for temp.N5F3O6 
[dir.ouT8S8]$ cat temp.SQslb7 
This is a test line of data for temp.SQslb7 
[dir.ouT8S8]$ 
```

# 8. 记录消息

- tee命令相当于管道的一个T型接头。它将从STDIN过来的数据同时发往两处。一处是 STDOUT，另一处是tee命令行所指定的文件名：

```
tee filename
```

- 默认情况下，tee命令会在每 次使用时覆盖输出文件内容。如果你想将数据追加到文件中，必须用-a选项。

# 参考文献

![image-20220106145912775](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220106145912775-16415048531181-16416260517421-16417357608321-16419697964231.png)