---
title: Linux shell(7)-函数
date: 2022/1/29
categories: linux
tags: [linux, shell]
top_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_43.jpg
cover: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_43.jpg
---

# 1. 创建函数

- 和其他编程语言类似，作为脚本语言的bash shell也有自己的函数形式，表示的意义也十分相近。
- 函数创建形式如下：

>a. 第一种格式采用关键字function，后跟分配给该代码块的函数名
>
>```bash
>function name { 
> commands 
>} 
>```
>
>b. 第二种格式函数名后的空括号表明正在定义的是一个函数。
>
>```bash
>name() { 
>commands 
>} 
>```

# 2. 使用函数

- 在行中指定函数名就行：

```bash
$ cat test1 
#!/bin/bash 
# using a function in a script 
function func1 { 
 echo "This is an example of a function" 
} 
count=1 
while [ $count -le 5 ] 
do 
 func1 
 count=$[ $count + 1 ] 
done 
echo "This is the end of the loop" 
func1 
echo "Now this is the end of the script" 
$ 
$ ./test1 
This is an example of a function 
This is an example of a function 
This is an example of a function 
This is an example of a function 
This is an example of a function 
This is the end of the loop
This is an example of a function 
Now this is the end of the script 
$
```

- **注意：**函数名必须是唯一的，否则也会有问题。如果你重定义了函数， 新定义会覆盖原来函数的定义，这一切不会产生任何错误消息。

# 3. 返回值

## 3.1 退出码

- bash shell会把函数当作一个小型脚本，运行结束时会返回一个退出状态码，可以在函数解释后使用 $? 来显示函数的退出码，没有成功则退出码为非零

- 案例1：

```bash
$ cat test4 
#!/bin/bash 
# testing the exit status of a function 
func1() { 
 echo "trying to display a non-existent file" 
 ls -l badfile 
} 
echo "testing the function: " 
func1 
echo "The exit status is: $?" 
$ 
$ ./test4
testing the function: 
trying to display a non-existent file 
ls: badfile: No such file or directory 
The exit status is: 1 
$ 
```

- 案例2：

```bash
$ cat test4b 
#!/bin/bash 
# testing the exit status of a function 
func1() { 
 ls -l badfile 
 echo "This was a test of a bad command" 
} 
echo "testing the function:" 
func1 
echo "The exit status is: $?" 
$ 
$ ./test4b 
testing the function: 
ls: badfile: No such file or directory 
This was a test of a bad command 
The exit status is: 0
```

- 默认情况下，函数的退出状态码是函数中最后一条命令返回的退出状态码。如上面两个案例所示，最后一个命令正确退出码为零，不正确时非零

## 3.2 return命令

- return命令来退出函数并返回特定的退出状态码。
- return命令允许指定一个**整数值**来定义函数的退出状态码

- 注意：

>a. 函数一结束就取返回值
>
>b. 退出码值必须为 0~255，否则会产生一个错误值

- 案例：

```bash
$ cat test5 
#!/bin/bash 
# using the return command in a function 
function dbl { 
 read -p "Enter a value: " value 
 echo "doubling the value" 
 return $[ $value * 2 ] 
} 
dbl 
echo "The new value is $?" 
$
$ ./test5 
Enter a value: 200 
doubling the value 
The new value is 1 
$ 
```

## 3.3 函数赋值到变量

- 命令可以赋值给变量，同样函数也可以，将函数名使用反引号括起来或者以美元符加括号的方式，这和之前的命令复制给变量是一样的.
- 通过这种技术，你还可以返回浮点值和字符串值。
- 案例：

```bash
$ cat test5b 
#!/bin/bash 
# using the echo to return a value 
function dbl { 
 read -p "Enter a value: " value 
 echo $[ $value * 2 ] 
} 
result=$(dbl) 
echo "The new value is $result" 
$ 
$ ./test5b 
Enter a value: 200 
The new value is 400 
$ 
$ ./test5b 
Enter a value: 1000 
The new value is 2000 
$
```

# 4. 变量

## 4.1 传递参数

- 在脚本中指定函数时，必须将参数和函数放在同一行

```bash
func $value1 
或
func 10
```

- 由于函数使用特殊参数环境变量作为自己的参数值，因此它无法直接获取脚本在命令行中的参数值。如下所示：

```bash
$ cat badtest1 
#!/bin/bash 
# trying to access script parameters inside a function 
function badfunc1 { 
 echo $[ $1 * $2 ] 
} 
if [ $# -eq 2 ] 
then 
 value=$(badfunc1) 
 echo "The result is $value" 
else 
 echo "Usage: badtest1 a b" 
fi 
$ 
$ ./badtest1 
Usage: badtest1 a b 
$ ./badtest1 10 15 
./badtest1: * : syntax error: operand expected (error token is "* 
") 
The result is
```

- 这就有点类似局部变量与全局变量，但又不是，局部和全局在后面将会被提及到。尽管函数也使用了$1和$2变量，但它们和脚本主体中的$1和$2变量并不相同。要在函数中使用这些值，必须在调用函数时手动将它们传过去。

- 示例如下：

```bash
$ cat test7 
#!/bin/bash 
# trying to access script parameters inside a function 
function func7 { 
 echo $[ $1 * $2 ] 
} 
if [ $# -eq 2 ] 
then 
 value=$(func7 $1 $2) 
 echo "The result is $value" 
else 
 echo "Usage: badtest1 a b" 
fi 
$ 
$ ./test7 
Usage: badtest1 a b 
$ ./test7 10 15 
The result is 150
```

## 4.2 全局与局部变量

- bash shell的全局与局部变量与其他语言中的类似，如果学了python或者C语言，就会很好理解
- 全局变量在函数外面直接声明就行，在函数内部使用的变量与函数外部的变量要区分，最好不要取同一个变量名字，防止混淆局部与全局变量
- 如果局部与全局变量使用相同的名字，那么可以在局部变量中使用 local 来表明这个变量是局部变量，如下所示：

```bash
$ cat test9 
#!/bin/bash 
# demonstrating the local keyword 
function func1 { 
 local temp=$[ $value + 5 ] 
 result=$[ $temp * 2 ] 
} 
temp=4 
value=6 
func1 
echo "The result is $result" 
if [ $temp -gt $value ] 
then 
 echo "temp is larger" 
else 
 echo "temp is smaller" 
fi 
$ 
$ ./test9 
The result is 22 
temp is smaller 
$
```

# 5. 数组

- bash中的数组和C语言很像，特别是像为数组重命名的过程，和C语言中的很相似
- 如果试图将该数组变量作为函数参数，函数只会取数组变量的第一个值。如下：

```bash
$ cat badtest3 
#!/bin/bash 
# trying to pass an array variable
function testit { 
 echo "The parameters are: $@" 
 thisarray=$1 
 echo "The received array is ${thisarray[*]}" 
} 
 
myarray=(1 2 3 4 5) 
echo "The original array is: ${myarray[*]}" 
testit $myarray 
$ 
$ ./badtest3 
The original array is: 1 2 3 4 5 
The parameters are: 1 
The received array is 1 
```

- 须将该数组变量的值分解成**单个的值**，然后将这些值作为函数参数使 用。在函数内部，可以将所有的参数**重新组合**成一个新的变量。如下：

```bash
$ cat test10 
#!/bin/bash 
# array variable to function test 
function testit { 
 local newarray 
 newarray=(;'echo "$@"') 
 echo "The new array value is: ${newarray[*]}" 
} 
myarray=(1 2 3 4 5) 
echo "The original array is ${myarray[*]}" 
testit ${myarray[*]} 
$ 
$ ./test10 
The original array is 1 2 3 4 5 
The new array value is: 1 2 3 4 5 
$
```

- 构建新数组，函数用echo语句来按正确顺序输出单个 数组值，然后脚本再将它们重新放进一个新的数组变量中。如下：

```bash
$ cat test12 
#!/bin/bash 
# returning an array value 
function arraydblr { 
 local origarray 
 local newarray 
 local elements 
 local i 
 origarray=($(echo "$@")) 
 newarray=($(echo "$@")) 
 elements=$[ $# - 1 ] 
 for (( i = 0; i <= $elements; i++ ))      # 
 {         								#
 newarray[$i]=$[ ${origarray[$i]} * 2 ]    # 
 } 										#
 echo ${newarray[*]} 					#
} 
myarray=(1 2 3 4 5) 
echo "The original array is: ${myarray[*]}" 
arg1=$(echo ${myarray[*]}) 
result=($(arraydblr $arg1)) 
echo "The new array is: ${result[*]}" 
$ 
$ ./test12 
The original array is: 1 2 3 4 5 
The new array is: 2 4 6 8 10
```

# 6. 递归

- 函数调用自身：

```bash
local result='factorial $temp' 
```

- 案例：

```bash
$ cat test13 
#!/bin/bash 
# using recursion 
function factorial { 
 if [ $1 -eq 1 ] 
 then 
 echo 1 
 else 
 local temp=$[ $1 - 1 ] 
 local result=$(factorial $temp) 
 echo $[ $result * $1 ] 
 fi 
} 
 
read -p "Enter value: " value 
result=$(factorial $value) 
echo "The factorial of $value is: $result" 
$ 
$ ./test13 
Enter value: 5 
The factorial of 5 is: 120 
```

# 7. 库函数

## 7.1 创建库函数

- 为了重复利用某个函数，我们可以创建库函数，被创建的库函数可以在任何脚本中被引用
- 使用函数库的关键在于source命令。source命令会在当前shell上下文中执行命令，而不是创建一个新shell。
- 可以用source命令来在shell脚本中运行库文件脚本。这样脚本就可以使用库中的函数了。 source命令有个快捷的别名，称作点操作符。如下：

```bash
. ./myfuncs 
```

- For example:

```bash
$ cat test14 
#!/bin/bash 
# using functions defined in a library file 

. ./myfuncs 
value1=10 
value2=5 
result1=$(addem $value1 $value2) 
result2=$(multem $value1 $value2) 
result3=$(divem $value1 $value2) 
echo "The result of adding them is: $result1" 
echo "The result of multiplying them is: $result2" 
echo "The result of dividing them is: $result3" 
$ 
$ ./test14 
The result of adding them is: 15 
The result of multiplying them is: 50 
The result of dividing them is: 2 
```

## 7.2 在.bashrc 文件中定义函数

- 在这之前，应该知道可以在命令行中使用函数，命令间使用分号隔开，这里不再详述了，详见书上372页
- 在命令行上直接定义shell函数的明显缺点是退出shell时，函数就消失了。

### 7.2.1 直接定义

- 在 .bashrc 文件中直接编辑函数，重新打开bash shell就会生效

```bash
$ cat .bashrc 
# .bashrc 
# Source global definitions 
if [ -r /etc/bashrc ]; then 
 . /etc/bashrc 
fi 
function addem { 
 echo $[ $1 + $2 ] 
} 
$ 
```

### 7.2.2 读取函数文件

- source命令 + 文件路径 → .bashrc文件末尾

```bash
$ cat .bashrc 
# .bashrc 
# Source global definitions 
if [ -r /etc/bashrc ]; then 
 . /etc/bashrc 
fi 
. /home/rich/libraries/myfuncs 
$ 

```

# 参考文献

![image-20220106145912775](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220106145912775-16415048531181-16416260517421-16417357608321-16419697964231-16420444905421.png)
