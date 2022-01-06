---
title: Linux shell(2)-结构化命令
date: 2022/1/7
categories: linux
tags: [linux, shell]
top_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_26.jpg
cover: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_26.jpg
---

# 1. if then 语句

- **if** + *命令1*，*<u>命令1</u>* 如果正确执行则执行 **then** 后面的*<u>命令集</u>*，**fi** 为结束语句，示例如下：

```
if command
then
 commands
fi 
```

- 案例

```
$ cat test2.sh
#!/bin/bash
# testing a bad command
if IamNotaCommand
then
 echo "It worked"
fi
======================================================
echo "We are outside the if statement"
$
$ ./test2.sh
./test2.sh: line 3: IamNotaCommand: command not found
We are outside the if statement
$ 
```

- **注意 if 后面只有一条命令**，执行后正确才会执行下面的 then 语句内容，**then 语句可以包含多条命令**

# 2. if-then-else 语句

- 当 **if** 语句判断失败时，使用 **else** 语句可以很好的显示失败后的命令，**else** 语句和 **then** 语句一样，可以包含多条命令

```
if command
then
 commands
else
 commands
fi 
```

- 案例：

```
$ cat test4.sh
#!/bin/bash
testuser=NoSuchUser

if grep $testuser /etc/passwd
then
 echo "The bash files for user $testuser are:"
 ls -a /home/$testuser/.b*
 echo
else
 echo "The user $testuser does not exist on this system."
 echo
fi
=============================================================
$ ./test4.sh
The user NoSuchUser does not exist on this system.
$ 
```

<br/>

# 3. 嵌套 if

- 使用 if 嵌套语句可以更精准的判断选择内容

```
if command1
then
 commands
elif command2
then
 more commands
fi
```

- 案例1：

```
$ cat test5.sh
#!/bin/bash
# Testing nested ifs - use elif

testus
if grep $testuser /etc/passwd
then
 echo "The user $testuser exists on this system."
elif ls -d /home/$testuser
then
 echo "The user $testuser does not exist on this system."
 echo "However, $testuser has a directory."
fi

========================================================
$ ./test5.sh
/home/NoSuchUser
The user NoSuchUser does not exist on this system.
However, NoSuchUser has a directory.
$
```

- **elif-then** 语句相当于 **if-then** 语句，我们可以使用 **elif-then-else**，
- 案例2：

```
$ cat test5.sh
#!/bin/bash
# Testing nested ifs - use elif & else

testuser=NoSuchUser

if grep $testuser /etc/passwd
then
 echo "The user $testuser exists on this system."
elif ls -d /home/$testuser
then
 echo "The user $testuser does not exist on this system."
 echo "However, $testuser has a directory."
else
 echo "The user $testuser does not exist on this system."
 echo "And, $testuser does not have a directory."
fi
===========================================================
$ ./test5.sh
/home/NoSuchUser
The user NoSuchUser does not exist on this system.
However, NoSuchUser has a directory.
$
$ sudo rmdir /home/NoSuchUser
[sudo] password for Christine:
$
$ ./test5.sh
ls: cannot access /home/NoSuchUser: No such file or directory
The user NoSuchUser does not exist on this system.
And, NoSuchUser does not have a directory.
$ 
```

<br/>

# 4. 测试语句

## 4.1 [ command ]

- 使用中括号在配合 if 语句进行测试，如果中括号里面的命令正确则会继续执行后面的语句
- 注意中括号内命令的两侧要有空格 [ 命令 ]

```
if [ condition ]
then
 commands
fi
```






## 4.2 test（不推荐）

```
test命令可以判断三类条件：
 数值比较
 字符串比较
 文件比较
```

- 可以使用 **test** 配合 **if** 语句进行测试，**test** 语句返回一个布尔值，布尔值为零。继续执行后面的内容，布尔值非零则会退出

```
if test condition
then
 commands
fi
```

- 案例

```
$ cat test6.sh
#!/bin/bash
# Testing the test command

my_variable="Full"

if test $my_variable
then
 echo "The $my_variable expression returns a True"
else
 echo "The $my_variable expression returns a False"
fi
======================================================
$ ./test6.sh
The Full expression returns a True
$ 
```

- 注意 **test** 后面**要有命令**，要不然就会返回一个非零的退出状态码，这样就会退出 **if** 语句

<br/>

## 4.3 数值比较

- test 命令最常见的情形是对两个数值进行比较
- 当然 bash shell 对数值的处理仅限于整数

![image-20220107034243006](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220107034243006.png)

- 案例：

```
$ cat numeric_test.sh
#!/bin/bash
# Using numeric test evaluations

value1=10
value2=11

if [ $value1 -gt 5 ]
then
 echo "The test value $value1 is greater than 5"
fi

if [ $value1 -eq $value2 ]
then
 echo "The values are equal"
else
 echo "The values are different"
fi
$
==================================================
$ ./numeric_test.sh
The test value 10 is greater than 5
The values are different
$ 
```

## 4.4 字符串比较

- 条件测试还允许比较字符串值。

![image-20220107035757139](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220107035757139.png)

### 4.4.1 字符串相等性

- 案例：

```
$ cat test7.sh
#!/bin/bash
# testing string equality

testuser=rich

if [ $USER = $testuser ]
then
 echo "Welcome $testuser"
fi
===================================
$ ./test7.sh
Welcome rich
```

###  4.4.2 字符串顺序

- 大于号和小于号必须使用斜杠 \ 转义，否则shell会把它们当作重定向符号，把字符串值当作文件名；

- <u>大于和小于顺序和sort命令所采用的不同。</u>test命令和测试表达式使用标准的数学比较符号来表示字符串比较，而用文本代码来表 示数值比较。

- 案例1：

```
$ cat test9.sh
#!/bin/bash
# mis-using string comparisons

val1=baseball
val2=hockey

if [ $val1 \> $val2 ] 
then
 echo "$val1 is greater than $val2"
else
 echo "$val1 is less than $val2"
fi
===================================
$ ./test9.sh
baseball is less than hockey
```

- 案例2：

```
$ cat test9b.sh
#!/bin/bash
# testing string sort order
val1=Testing
val2=testing

if [ $val1 \> $val2 ]
then
 echo "$val1 is greater than $val2"
else
 echo "$val1 is less than $val2"
fi
================================================
$ ./test9b.sh
Testing is less than testing
$
$ sort testfile
testing
Tes
```

 ### 4.4.3 字符串大小 

- -n和-z可以检查一个变量是否含有数据。

案例：

```
$ cat test10.sh
#!/bin/bash
# testing string length
val1=testing
val2=''

if [ -n $val1 ]
then
 echo "The string '$val1' is not empty"
else
 echo "The string '$val1' is empty"
fi

if [ -z $val2 ]
then
 echo "The string '$val2' is empty"
else
 echo "The string '$val2' is not empty"
fi

if [ -z $val3 ]
then
 echo "The string '$val3' is empty"
else
 echo "The string '$val3' is not empty"
fi
============================================
$ ./test10.sh
The string 'testing' is not empty
The string '' is empty
The string '' is empty
=============================================================

if [ -n $val1 ]
判断val1变量是否长度非0，而它的长度正好非0，所以then部分被执行了。
if [ -z $var2 ]
判断val2变量是否长度为0，而它正好长度为0，所以then部分被执行了。
if [ -z $val3 ]
判断val3变量是否长度为0。这个变量并未在shell脚本中定义过，所以它的字符串长度仍然
为0，尽管它未被定义过。
```

## 4.5 文件比较

- 测试Linux文件系统上文件和目录的状态。

![image-20220107045246664](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220107045246664.png)

- 案例：
  - -d 测试会检查指定的目录是否存在于系统中。如果你打算将文件写入目录或是准备切换到某个目录中，先进行测试总是件好事情。

```
$ cat test11.sh
#!/bin/bash
# Look before you leap

jump_directory=/home/arthur

if [ -d $jump_directory ]
then
 echo "The $jump_directory directory exists"
 cd $jump_directory
 ls
else
 echo "The $jump_directory directory does not exist"
fi
============================================================
$ ./test11.sh
The /home/arthur directory does not exist
$
```

- 示例代码中使用了-d测试条件来检查jump_directory变量中的目录是否存在：若存在，就 使用cd命令切换到该目录并列出目录中的内容；若不存在，脚本就输出一条警告信息，然后退出。

# 5. 复合条件测试

- if-then语句允许你使用布尔逻辑来组合测试。有两种布尔运算符可用：

>- [ condition1 ] && [ condition2 ]
>- [ condition1 ] || [ condition2 ]
>- 第一种布尔运算使用AND布尔运算符来组合两个条件。
>- 第二种布尔运算使用OR布尔运算符来组合两个条件。

案例：

```
$ cat test22.sh
#!/bin/bash
# testing compound comparisons

if [ -d $HOME ] && [ -w $HOME/testing ]
then
 echo "The file exists and you can write to it"
else
 echo "I cannot write to the file"
fi
===============================
$ ./test22.sh
I cannot write to the file
$
$ touch $HOME/testing
$
$ ./test22.sh
The file exists and you can write to it 
```



# 6. if-then 的高级特性

- bash shell提供了两项可在if-then语句中使用的高级特性： 

>- 用于数学表达式的双括号 
>-  用于高级字符串处理功能的双方括号

## 6.1 使用双括号

- 双括号命令允许你在比较过程中使用高级数学表达式。格式如下：

`(( expression ))` 

- expression 可以是任意的数学赋值或比较表达式。

![image-20220107052609582](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220107052609582.png)

- 注意，不需要将双括号中表达式里的大于号转义。这是双括号命令提供的另一个高级特性

## 6.2 使用双方括号

- 双方括号命令提供了针对字符串比较的高级特性。格式如下：

`[[ expression ]]` 

- 双方括号在bash shell中工作良好。不过不是所有的shell都支持双方括号。

- 案例：

```
$ cat test24.sh
#!/bin/bash
# using pattern matching

if [[ $USER == r* ]]
then
 echo "Hello $USER"
else
 echo "Sorry, I do not know you"
fi
====================================
$ ./test24.sh
Hello
```

<br/>

# 7. case 命令

- 有了case命令，就不需要再写出所有的elif语句来不停地检查同一个变量的值了。case命 令会采用列表格式来检查单个变量的多个值。

```
case variable in
pattern1 | pattern2) commands1;;
pattern3) commands2;;
*) default commands;;
esac 
```

- 案例：

```
$ cat test26.sh
#!/bin/bash
# using the case command

case $USER in
rich | barbara)
 echo "Welcome, $USER"
 echo "Please enjoy your visit";;
testing)
 echo "Special testing account";;
jessica)
 echo "Do not forget to log off when you're done";;
*)
 echo "Sorry, you are not allowed here";;
esac
=
$ ./test26.sh
Welcome, rich
Please enjoy your visit 
```

# 参考文献

![image-20220106145912775](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220106145912775-16415048531181.png)
