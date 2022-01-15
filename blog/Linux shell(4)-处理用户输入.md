---
title: Linux shell(4)-处理用户输入
date: 2022/1/9
categories: linux
tags: [linux, shell]
top_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_29.jpg
cover: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_29.jpg
---

# 1. 命令行参数

## 1.1 读取参数

- 位置参数变量是标准的数字：$0是程序名，$1是第 一个参数，$2是第二个参数，依次类推，直到第九个参数$9
- 如果脚本需要的命令行参数不止9个，你仍然可以处理，但是需要稍微修改一下变量名。在 第9个变量之后，你必须在变量数字周围加上花括号，比如${10}。

- 案例1：

```
$ cat test4.sh
#!/bin/bash 
# handling lots of parameters 
# 
total=$[ ${10} * ${11} ] 
echo The tenth parameter is ${10} 
echo The eleventh parameter is ${11} 
echo The total is $total 
$ 
$ ./test4.sh 1 2 3 4 5 6 7 8 9 10 11 12
The tenth parameter is 10 
The eleventh parameter is 11 
The total is 110 
$
```

- 要在参数值中 包含空格，必须要用引号（单引号或双引号均可）
- 案例2：

```
$ cat test3.sh
#!/bin/bash 
# testing string parameters 
# 
echo Hello $1, glad to meet you. 
$ 
$ ./test3.sh Rich Blum
Hello Rich, glad to meet you. 
$
$ ./test3.sh 'Rich Blum'
Hello Rich Blum, glad to meet you. 
$ 
$ ./test3.sh "Rich Blum"
Hello Rich Blum, glad to meet you. 
$ 
```



##  1.2 读取脚本名

- 可以用$0参数获取shell在命令行启动的脚本名。

```
$ cat test5.sh
#!/bin/bash 
# Testing the $0 parameter 
# 
echo The zero parameter is set to: $0 
$ 
$ bash test5.sh
The zero parameter is set to: test5.sh
```

- 当传给$0变量的实际字符串不仅仅是脚本名，而是完整的脚本路径时， 变量$0就会使用整个路径。

```
$ bash /home/Christine/test5.sh
The zero parameter is set to: /home/Christine/test5.sh 
$ 
```

- basename命令会返回不包含路径的脚本名

```
$ cat test5b.sh
#!/bin/bash 
# Using basename with the $0 parameter 
# 
name=$(basename $0) 
echo 
echo The script name is: $name 
# 
$ bash /home/Christine/test5b.sh
The script name is: test5b.sh 
$ 
$ ./test5b.sh
The script name is: test5b.sh 
```

# 2. 特殊参数变量

## 2.1 $#

- 特殊变量$#含有脚本运行时携带的命令行参数的个数
- 案例1：

```
$ cat test8.sh
#!/bin/bash 
# getting the number of parameters 
# 
echo There were $# parameters supplied. 
$ 
$ ./test8.sh
There were 0 parameters supplied. 
$ 
$ ./test8.sh 1 2 3 4 5
There were 5 parameters supplied. 
$ 
$ ./test8.sh 1 2 3 4 5 6 7 8 9 10
There were 10 parameters supplied. 
$ 
$ ./test8.sh "Rich Blum"
There were 1 parameters supplied. 
```

- 不能在花括号内使用美元符。必须将美元符换成感叹号。

- 案例2:

```
$ cat test10.sh
#!/bin/bash 
# Grabbing the last parameter 
# 
params=$# 
echo 
echo The last parameter is $params 
echo The last parameter is ${!#} 
echo 
# 
$ 
$ bash test10.sh 1 2 3 4 5
The last parameter is 5 
The last parameter is 5 
$ 
$ bash test10.sh
The last parameter is 0 
The last parameter is test10.sh 
$
```

## 2.2 $* 和 $@

- $* 和 $@ 变量可以用来轻松访问所有的参数。这两个变量都能够在单个变量中存储所有的命 令行参数

- $* 变量会将这些参数视为一个整体，而不是多个个体

- $@ 变量会将命令行上提供的所有参数当作同一字符串中的多个独立的单词
- 案例1：

```
$ cat test11.sh
#!/bin/bash 
# testing $* and $@ 
# 
echo 
echo "Using the \$* method: $*" 
echo 
echo "Using the \$@ method: $@" 
$ 
$ ./test11.sh rich barbara katie jessica
Using the $* method: rich barbara katie jessica 
Using the $@ method: rich barbara katie jessica 
```

- 案例2：

```
$ cat test12.sh
#!/bin/bash
# testing $* and $@ 
# 
echo 
count=1 
# 
for param in "$*" 
do 
 echo "\$* Parameter #$count = $param" 
 count=$[ $count + 1 ] 
done 
# 
echo 
count=1 
# 
for param in "$@" 
do 
 echo "\$@ Parameter #$count = $param" 
 count=$[ $count + 1 ] 
done 
$ 
$ ./test12.sh rich barbara katie jessica
$* Parameter #1 = rich barbara katie jessica 
$@ Parameter #1 = rich 
$@ Parameter #2 = barbara 
$@ Parameter #3 = katie 
$@ Parameter #4 = jessica 
```

- $@ 变量会单独处理每个参数。这是遍历命 令行参数的一个绝妙方法

# 3.  移动变量

- 在使用shift命令时，默认情况下它会将每个参数变量向左移动一个位置。所以，变量$3 的值会移到$2中，变量$2的值会移到$1中，而变量$1的值则会被删除（注意，变量$0的值，也 就是程序名，不会改变）

- 案例1：

```
$ cat test13.sh
#!/bin/bash 
# demonstrating the shift command
echo 
count=1 
while [ -n "$1" ] 
do 
 echo "Parameter #$count = $1" 
 count=$[ $count + 1 ] 
 shift 
done 
$ 
$ ./test13.sh rich barbara katie jessica
Parameter #1 = rich 
Parameter #2 = barbara 
Parameter #3 = katie 
Parameter #4 = jessica 
$
```

- 也可以一次性移动多个位置，只需要给shift命令提供一个参数，指明要移动的位 置数就行了

- 案例2：

```
$ cat test14.sh
#!/bin/bash 
# demonstrating a multi-position shift 
# 
echo 
echo "The original parameters: $*" 
shift 2 
echo "Here's the new first parameter: $1" 
$ 
$ ./test14.sh 1 2 3 4 5
The original parameters: 1 2 3 4 5 
Here's the new first parameter: 3
```

# 4. 处理选项

## 4.1 查找选项

- 在提取每个单独参数时，用case语句来判断某个参数是否为选项。

```
$ cat test15.sh
#!/bin/bash 
# extracting command line options as parameters 
# 
echo 
while [ -n "$1" ] 
do 
 case "$1" in 
 -a) echo "Found the -a option" ;; 
 -b) echo "Found the -b option" ;; 
 -c) echo "Found the -c option" ;; 
 *) echo "$1 is not an option" ;; 
 esac 
 shift 
done 
$ 
$ ./test15.sh -a -b -c -d
Found the -a option 
Found the -b option 
Found the -c option 
-d is not an option
```

#### 分离参数和选项

- shell会用双破折线来表明选项列表结束。在 双破折线之后，脚本就可以放心地将剩下的命令行参数当作参数，而不是选项来处理了。

- 案例1：

```
$ cat test16.sh
#!/bin/bash 
# extracting options and parameters 
echo 
while [ -n "$1" ] 
do 
 case "$1" in 
 -a) echo "Found the -a option" ;; 
 -b) echo "Found the -b option";; 
 -c) echo "Found the -c option" ;; 
 --) shift 
 break ;; 
 *) echo "$1 is not an option";; 
 esac 
 shift 
done 
# 
count=1 
for param in $@ 
do 
 echo "Parameter #$count: $param" 
 count=$[ $count + 1 ] 
done
```

- 在遇到双破折线时，脚本用break命令来跳出while循环。由于过早地跳出了循环，我们需 要再加一条shift命令来将双破折线移出参数变量

```
$ ./test16.sh -c -a -b test1 test2 test3
Found the -c option 
Found the -a option 
Found the -b option 
test1 is not an option 
test2 is not an option 
test3 is not an option 
$
========================================
$ ./test16.sh -c -a -b -- test1 test2 test3
Found the -c option 
Found the -a option 
Found the -b option 
Parameter #1: test1
Parameter #2: test2 
Parameter #3: test3 
$ 
```

## 4.2  getopt 命令

- 命令格式：

```
getopt optstring parameters
```

>optstring是这个过程的关键所在。它定义了命令行有效的选项字母，还定义了哪些选项字母需要参数值。
>
> 首先，在optstring中列出你要在脚本中用到的每个命令行选项字母。然后，在每个需要参数值的选项字母后加一个冒号。getopt命令会基于你定义的optstring解析提供的参数。

- 案例1：

```
$ getopt ab:cd -a -b test1 -cd test2 test3
 -a -b test1 -c -d -- test2 test3 
$ 
```

- 它会自动将-cd选项分成两个单独的选项，并插入双破折线来分隔行中的额外参数。

- **在脚本中使用getopt**

```
set -- $(getopt -q ab:cd "$@")
```

- 案例2：

```
$ cat test18.sh
#!/bin/bash 
# Extract command line options & values with getopt 
# 
set -- $(getopt -q ab:cd "$@") 
# 
echo 
while [ -n "$1" ] 
do 
 case "$1" in 
 -a) echo "Found the -a option" ;; 
 -b) param="$2" 
 echo "Found the -b option, with parameter value $param" 
 shift ;; 
 -c) echo "Found the -c option" ;; 
 --) shift 
 break ;; 
 *) echo "$1 is not an option";; 
 esac
 shift 
done 
# 
count=1 
for param in "$@" 
do 
 echo "Parameter #$count: $param" 
 count=$[ $count + 1 ] 
done 
# 
$
```

- getopt命令并不擅长处理带空格和引号的参数值。它会将空格当作参数分隔符，而不是根 据双引号将二者当作一个参数。

## 4.3 getopts 命令

- 与getopt不同，前者将命令行上选项和参数处理后只生成一个输出，而getopts命令能够 和已有的shell参数变量配合默契。<u>它非常适合用解析命令行所有参数的循环中。</u>

- 案例：

```
$ cat test19.sh
#!/bin/bash 
# simple demonstration of the getopts command 
# 
echo 
while getopts :ab:c opt 
do 
 case "$opt" in 
 a) echo "Found the -a option" ;; 
 b) echo "Found the -b option, with value $OPTARG";; 
 c) echo "Found the -c option" ;; 
 *) echo "Unknown option: $opt";; 
 esac 
done 
$ 
$ ./test19.sh -ab test1 -c
Found the -a option 
Found the -b option, with value test1 
Found the -c option 
$
```

>1. 可以在参数值中包含空格。
>2. 可以将选项字母和参数值放在一起使用，而不用加空格。
>3. getopts还能够将命令行上 找到的所有未定义的选项统一输出成问号

# 5. 选项标准化

![image-20220109213331537](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220109213331537.png)

# 6. 获得用户输入

- read命令从标准输入（键盘）或另一个文件描述符中接受输入。

- **read命令包含了-p选项，允许你直接在read命令行指定提示符。**

- 案例1：

```
$ cat test22.sh
#!/bin/bash 
# testing the read -p option 
# 
read -p "Please enter your age: " age 
days=$[ $age * 365 ] 
echo "That makes you over $days days old! " 
# 
$ 
$ ./test22.sh
Please enter your age: 10
That makes you over 3650 days old! 
```

- **可以在read命令行中不指定变量。**如果是这样，read命令会将它收到的任何数据都放进 特殊环境变量REPLY中。

- 案例2：

```
$ cat test24.sh
#!/bin/bash 
# Testing the REPLY Environment variable 
# 
read -p "Enter your name: " 
echo 
echo Hello $REPLY, welcome to my program. 
# 
$ 
$ ./test24.sh
Enter your name: Christine
Hello Christine, welcome to my program.
```

- **-t 选项指定了read命令等待 输入的秒数**。

- 案例3：

```
$ cat test25.sh
#!/bin/bash 
# timing the data entry 
# 
if read -t 5 -p "Please enter your name: " name 
then 
 echo "Hello $name, welcome to my script" 
else 
 echo 
 echo "Sorry, too slow! " 
fi 
$ 
$ ./test25.sh
Please enter your name: Rich
Hello Rich, welcome to my script 
$
$ ./test25.sh
Please enter your name: 
Sorry, too slow! 
$ 
```

- **-n 选项和值1一起使用，告诉read命令在接受单个字符后退出。**只要按下单个字符 回答后，read命令就会接受输入并将它传给变量，无需按回车键。

- 案例4：

```
$ cat test26.sh
#!/bin/bash 
# getting just one character of input 
# 
read -n1 -p "Do you want to continue [Y/N]? " answer 
case $answer in 
Y | y) echo 
 echo "fine, continue on…";; 
N | n) echo 
 echo OK, goodbye 
 exit;; 
esac 
echo "This is the end of the script" 
$ 
$ ./test26.sh
Do you want to continue [Y/N]? Y
fine, continue on… 
This is the end of the script 
$ 
$ ./test26.sh
Do you want to continue [Y/N]? n
OK, goodbye
```

- **-s 选项可以避免在read命令中输入的数据出现在显示器上**，**如：密码输入**

- 案例5：

```
$ cat test27.sh
#!/bin/bash 
# hiding input data from the monitor 
# 
read -s -p "Enter your password: " pass
echo 
echo "Is your password really $pass? " 
$ 
$ ./test27.sh
Enter your password: 
Is your password really T3st1ng? 
```

- **是将文件中的数据传给read命令**。最常见的方法是对文件使用cat命令，将 结果通过管道直接传给含有read命令的while命令。

- 案例6：

```
$ cat test28.sh 
#!/bin/bash 
# reading data from a file 
# 
count=1 
cat test | while read line 
do 
 echo "Line $count: $line" 
 count=$[ $count + 1] 
done 
echo "Finished processing the file" 
$ 
$ cat test
The quick brown dog jumps over the lazy fox. 
This is a test, this is only a test. 
O Romeo, Romeo! Wherefore art thou Romeo? 
$ 
$ ./test28.sh
Line 1: The quick brown dog jumps over the lazy fox. 
Line 2: This is a test, this is only a test. 
Line 3: O Romeo, Romeo! Wherefore art thou Romeo? 
Finished processing the file
```

# 参考文献

![image-20220106145912775](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220106145912775-16415048531181-16416260517421-16417357608321.png)