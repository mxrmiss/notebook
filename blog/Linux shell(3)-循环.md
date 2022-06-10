---
title: Linux shell(3)-循环
date: 2022/1/8
categories: linux
tags: [linux, shell]
index_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_27.jpg
banner_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_27.jpg
---

# 1. for 命令

- for 命令用于循环，格式如下：

````
for var in list
do
 commands
done 
````

- 也可以将do语句和for语句放在同一行，但必须用分号将其同列表中的值分 开：for var in list; do



## 1.1 读取列表中的值

- for命令最基本的用法就是遍历for命令自身所定义的一系列值。案例如下：

```
$ cat test1
#!/bin/bash
# basic for command
for test in Alabama Alaska Arizona Arkansas California Colorado
do
 echo The next state is $test
done

$ ./test1
The next state is Alabama
The next state is Alaska
The next state is Arizona
The next state is Arkansas
The next state is California
The next state is Colorado
$
```



## 1.2 读取列表中复杂值

- 列表中可能有像单引号等不好读取的值，需要我们对此进行转置

>+ 使用转义字符（反斜线）来将单引号转义；
>
>+ 使用双引号来定义用到单引号的值。

- 如果在单独的数据值中有**空格**，就必须用**双引号**将这些值圈起来。
- 案例如下：

```
$ cat test2
#!/bin/bash
# another example of how not to use the for command
for test in I don\'t know if "this'll" work
do
 echo "word:$test"
done

$ ./test2
word:I
word:don't
word:know
word:if
word:this'll
word:work
$ 
```



## 1.3 从变量读取列表

- 通常shell脚本遇到的情况是，你将一系列值都集中存储在了一个变量中，然后需要遍历变量
  中的整个列表
- 案例如下：

```
$ cat test4
#!/bin/bash
# using a variable to hold the list
list="Alabama Alaska Arizona Arkansas Colorado"
list=$list" Connecticut"
for state in $list
do
echo "Have you ever visited $state?"
done

$ ./test4
Have you ever visited Alabama?
Have you ever visited Alaska?
Have you ever visited Arizona?
Have you ever visited Arkansas?
Have you ever visited Colorado?
Have you ever visited Connecticut?
```



## 1.4 从命令读取值

- 案例如下：

```
$ cat test5
#!/bin/bash
# reading values from a file
file="states"
for state in $(cat $file)
do
echo "Visit beautiful $state"
done

$ cat states
Alabama
Alaska
Arizona
Arkansas
Colorado
Connecticut
Delaware
Florida
Georgia

$ ./test5
Visit beautiful Alabama
Visit beautiful Alaska
Visit beautiful Arizona
Visit beautiful Arkansas
Visit beautiful Colorado
Visit beautiful Connecticut
Visit beautiful Delaware
Visit beautiful Florida
Visit beautiful Georgia
```



## 1.5 更改字段分隔符

- 变量IFS，叫作内部字段分隔符（internal field separator）。
- IFS环境变量定义了bash shell用作字段分隔符的一系列字符。默认情况下，bash shell会将下列字符当作字段分隔符：
  - 空格
  - 制表符
  - 换行符
- 如果bash shell在数据中看到了这些字符中的任意一个，它就会假定这表明了列表中一个新数
  据字段的开始

- 如果你列出了一个名字中有空格的州，for命令仍然会将每个单词当作单独的值。修改IFS的值，使其只能识别换行符

```
IFS=$'\n'
```

- 将这个语句加入到脚本中，告诉bash shell在数据值中忽略空格和制表符。对前一个脚本使用
  这种方法，将获得如下输出。

```
$ cat test5b
#!/bin/bash
# reading values from a file
file="states"
IFS=$'\n'
for state in $(cat $file)
do
echo "Visit beautiful $state"
done

$ ./test5b
Visit beautiful Alabama
Visit beautiful Alaska
Visit beautiful Arizona
Visit beautiful Arkansas
Visit beautiful Colorado
Visit beautiful Connecticut
Visit beautiful Delaware
Visit beautiful Florida
Visit beautiful Georgia
Visit beautiful New York
Visit beautiful New Hampshire
Visit beautiful North Carolina
```

<br/>

>- 在处理代码量较大的脚本时，可能在一个地方需要修改IFS的值，然后忽略这次修改，在脚本的其他地方继续沿用IFS的默认值。一个可参考的安全实践是在改变IFS之前保存原来的IFS值，之后再恢复它。
>- 这种技术可以这样实现：
>
>```
>IFS.OLD=$IFS
>IFS=$'\n'
>```
>
>- 在代码中使用新的IFS值
>
>```
>IFS=$IFS.OLD
>```
>
>这就保证了在脚本的后续操作中使用的是IFS的默认值。

>- 如果要指定多个IFS字符，只要将它们在赋值行串起来就行。
>
>```
>IFS=$'\n':;"
>```
>
>这个赋值会将换行符、冒号、分号和双引号作为字段分隔符。如何使用IFS字符解析数据没
>有任何限制。



## 1.6 用通配符读取目录

- 案例：

```
$ cat test6
#!/bin/bash
# iterate through all the files in a directory
for file in /home/rich/test/*
do
if [ -d "$file" ]
then
echo "$file is a directory"
elif [ -f "$file" ]
then
echo "$file is a file"
fi
done

$ ./test6
/home/rich/test/dir1 is a directory
/home/rich/test/myprog.c is a file
/home/rich/test/myprog is a file
/home/rich/test/myscript is a file
/home/rich/test/newdir is a directory
/home/rich/test/newfile is a file
/home/rich/test/newfile2 is a file
/home/rich/test/testdir is a directory
/home/rich/test/testing is a file
/home/rich/test/testprog is a file
/home/rich/test/testprog.c is a file
```

>- 注意，我们在这个例子的if语句中做了一些不同的处理：
>
>`if [ -d "$file" ]`
>在Linux中，目录名和文件名中包含空格当然是合法的。要适应这种情况，应该将$file变
>量用双引号圈起来。如果不这么做，遇到含有空格的目录名或文件名时就会有错误产生。
>
>```
>./test6: line 6: [: too many arguments
>./test6: line 9: [: too many arguments
>```



# 2. while 命令

- while命令的格式是：

```
while test command
do
other commands
done
```

- 案例：

```
$ cat test10
#!/bin/bash
# while command test
var1=10
while [ $var1 -gt 0 ]
do
echo $var1
var1=$[ $var1 - 1 ]
done

$ ./test10
10
9
8
7
6
5
4
3
2
1
$
```



# 3. until 命令

- until命令和while命令工作的方式完全相反。until命令要求你指定一个通常返回非零退出状态码的测试命令。只有测试命令的退出状态码不为0，bash shell才会执行循环中列出的命令。
- 一旦测试命令返回了退出状态码0，循环就结束了。

- until命令的格式如下：

```
until test commands
do
other commands
done
```

- 案例如下：

```
$ cat test12
#!/bin/bash
# using the until command
var1=100
until [ $var1 -eq 0 ]
do
echo $var1
var1=$[ $var1 - 25 ]
done
$ ./test12
100
75
50
25
$
```



# 4. 嵌套循环

- 案例(1)：

```
$ cat test14
#!/bin/bash
# nesting for loops
for (( a = 1; a <= 3; a++ ))
do
echo "Starting loop $a:"
for (( b = 1; b <= 3; b++ ))
do
echo " Inside loop: $b"
done
done
$ ./test14
Starting loop 1:
Inside loop: 1
Inside loop: 2
Inside loop: 3
Starting loop 2:
Inside loop: 1
Inside loop: 2
Inside loop: 3
Starting loop 3:
Inside loop: 1
Inside loop: 2
Inside loop: 3
$
```

- 案例（2）：

```
$ cat test15
#!/bin/bash
# placing a for loop inside a while loop
var1=5
while [ $var1 -ge 0 ]
do
echo "Outer loop: $var1"
for (( var2 = 1; $var2 < 3; var2++ ))
do
var3=$[ $var1 * $var2 ]
echo " Inner loop: $var1 * $var2 = $var3"
done
var1=$[ $var1 - 1 ]
done
$ ./test15
Outer loop: 5
Inner loop: 5 * 1 = 5
Inner loop: 5 * 2 = 10
Outer loop: 4
Inner loop: 4 * 1 = 4
Inner loop: 4 * 2 = 8
Outer loop: 3
Inner loop: 3 * 1 = 3
Inner loop: 3 * 2 = 6
Outer loop: 2
Inner loop: 2 * 1 = 2
Inner loop: 2 * 2 = 4
Outer loop: 1
Inner loop: 1 * 1 = 1
Inner loop: 1 * 2 = 2
Outer loop: 0
Inner loop: 0 * 1 = 0
Inner loop: 0 * 2 = 0
```

- 案例(3):

```
$ cat test16
#!/bin/bash
# using until and while loops
var1=3
until [ $var1 -eq 0 ]
do
echo "Outer loop: $var1"
var2=1
while [ $var2 -lt 5 ]
do
var3=$(echo "scale=4; $var1 / $var2" | bc)
echo " Inner loop: $var1 / $var2 = $var3"
var2=$[ $var2 + 1 ]
done
var1=$[ $var1 - 1 ]
done
$ ./test16
Outer loop: 3
Inner loop: 3 / 1 = 3.0000
Inner loop: 3 / 2 = 1.5000
Inner loop: 3 / 3 = 1.0000
Inner loop: 3 / 4 = .7500
Outer loop: 2
Inner loop: 2 / 1 = 2.0000
Inner loop: 2 / 2 = 1.0000
Inner loop: 2 / 3 = .6666
Inner loop: 2 / 4 = .5000
Outer loop: 1
Inner loop: 1 / 1 = 1.0000
Inner loop: 1 / 2 = .5000
Inner loop: 1 / 3 = .3333
Inner loop: 1 / 4 = .2500
$
```



# 5. 循环处理文件数据

- 使用嵌套循环
- 修改IFS环境变量

>通过修改IFS环境变量，就能强制for命令将文件中的每行都当成单独的一个条目来处理，
>即便数据中有空格也是如此。一旦从文件中提取出了单独的行，可能需要再次利用循环来提取行中的数据

- 案例：

```
#!/bin/bash
# changing the IFS value
IFS.OLD=$IFS
IFS=$'\n'
for entry in $(cat /etc/passwd)
do
echo "Values in $entry –"
IFS=:
for value in $entry
do
echo " $value"
done
done
$
```



# 6. 控制循环

## 6.1 break

- **跳出单个循环**

>- 跳出当前正在执行的循环。
>
>For example:
>
>```
>$ cat test17
>#!/bin/bash
># breaking out of a for loop
>for var1 in 1 2 3 4 5 6 7 8 9 10
>do
>if [ $var1 -eq 5 ]
>then
>break
>fi
>echo "Iteration number: $var1"
>done
>echo "The for loop is completed"
>
>$ ./test17
>Iteration number: 1
>Iteration number: 2
>Iteration number: 3
>Iteration number: 4
>The for loop is completed
>$
>```
>
>- while循环会在if-then的条件满足时执行break命令，终止

- **跳出内部循环**

>- 在处理多个循环时，break命令会自动终止你所在的最内层的循环。
>- 案例：
>
>```
>$ cat test19
>#!/bin/bash
># breaking out of an inner loop
>for (( a = 1; a < 4; a++ ))
>do
>echo "Outer loop: $a"
>for (( b = 1; b < 100; b++ ))
>do
>if [ $b -eq 5 ]
>then
>break
>fi
>echo " Inner loop: $b"
>done
>done
>
>$ ./test19
>Outer loop: 1
>Inner loop: 1
>Inner loop: 2
>Inner loop: 3
>Inner loop: 4
>Outer loop: 2
>Inner loop: 1
>Inner loop: 2
>Inner loop: 3
>Inner loop: 4
>Outer loop: 3
>Inner loop: 1
>Inner loop: 2
>Inner loop: 3
>Inner loop: 4
>$
>```
>
>- 注意，即使内部循环通过break命令终止了，外部循环依然继续执行。

- **跳出外部循环**

>- 有时你在内部循环，但需要停止外部循环。break命令接受单个命令行参数值：
>
>```
>break n
>```
>
>- 其中n指定了要跳出的循环层级。默认情况下，n为1，表明跳出的是当前的循环。如果你将n设为2，break命令就会停止下一级的外部循环。
>- 案例：
>
>```
>$ cat test20
>#!/bin/bash
># breaking out of an outer loop
>for (( a = 1; a < 4; a++ ))
>do
>echo "Outer loop: $a"
>for (( b = 1; b < 100; b++ ))
>do
>if [ $b -gt 4 ]
>then
>break 2
>fi
>echo " Inner loop: $b"
>done
>done
>$ ./test20
>Outer loop: 1
>Inner loop: 1
>Inner loop: 2
>Inner loop: 3
>Inner loop: 4
>$
>```

## 6.2 continue 命令

- continue命令可以提前中止某次循环中的命令，但并不会完全终止整个循环。

- 案例：

```
$ cat test21
#!/bin/bash
# using the continue command
for (( var1 = 1; var1 < 15; var1++ ))
do
if [ $var1 -gt 5 ] && [ $var1 -lt 10 ]
then
continue
fi
echo "Iteration number: $var1"
done
$ ./test21
Iteration number: 1
Iteration number: 2
Iteration number: 3
Iteration number: 4
Iteration number: 5
Iteration number: 10
Iteration number: 11
Iteration number: 12
Iteration number: 13
Iteration number: 14
$
```

>也可以在while和until循环中使用continue命令，但要特别小心。记住，当shell执行 continue命令时，它会跳过剩余的命令。如果你在其中某个条件里对测试条件变量进行增值，问题就会出现。
>
>```
>$ cat badtest3
>#!/bin/bash
># improperly using the continue command in a while loop
>var1=0
>while echo "while iteration: $var1"
>[ $var1 -lt 15 ]
>do
>if [ $var1 -gt 5 ] && [ $var1 -lt 10 ]
>then
>continue
>fi
>echo " Inside iteration number: $var1"
>var1=$[ $var1 + 1 ]
>done
>$ ./badtest3 | more
>while iteration: 0
>Inside iteration number: 0
>while iteration: 1
>Inside iteration number: 1
>while iteration: 2
>Inside iteration number: 2
>while iteration: 3
>Inside iteration number: 3
>while iteration: 4
>Inside iteration number: 4
>while iteration: 5
>Inside iteration number: 5
>while iteration: 6
>while iteration: 6
>while iteration: 6
>while iteration: 6
>while iteration: 6
>while iteration: 6
>```

- 和break命令一样，continue命令也允许通过命令行参数指定要继续执行哪一级循环：

```
continue n
```

- 其中n定义了要继续的循环层级



## 6.3 处理循环的输出

- 对循环的输出使用管道或进行重定向。这可以通过在done命令之后添加一个处理命令来实现。
- 案例：

```
for file in /home/rich/*
do
if [ -d "$file" ]
then
echo "$file is a directory"
elif
echo "$file is a file"
fi
done > output.txt
```

<br/>

# 7. 实例

```
13.9.1 查找可执行文件
当你从命令行中运行一个程序的时候，Linux系统会搜索一系列目录来查找对应的文件。这
些目录被定义在环境变量PATH中。如果你想找出系统中有哪些可执行文件可供使用，只需要扫
描PATH环境变量中所有的目录就行了。如果要徒手查找的话，就得花点时间了。不过我们可以
编写一个小小的脚本，轻而易举地搞定这件事。
首先是创建一个for循环，对环境变量PATH中的目录进行迭代。处理的时候别忘了设置IFS
分隔符。
IFS=:
for folder in $PATH
do
现在你已经将各个目录存放在了变量$folder中，可以使用另一个for循环来迭代特定目录
中的所有文件。
for file in $folder/*
do
最后一步是检查各个文件是否具有可执行权限，你可以使用if-then测试功能来实现。
if [ -x $file ]
then
echo " $file"
fi
好了，搞定了！将这些代码片段组合成脚本就行了。
$ cat test25
#!/bin/bash
# finding files in the PATH
IFS=:
for folder in $PATH
do
echo "$folder:"
for file in $folder/*
do
if [ -x $file ]
then
echo " $file"
fi
done
done
$
运行这段代码时，你会得到一个可以在命令行中使用的可执行文件的列表。
$ ./test25 | more
/usr/local/bin:
/usr/bin:
/usr/bin/Mail
/usr/bin/Thunar
/usr/bin/X
/usr/bin/Xorg
/usr/bin/[
/usr/bin/a2p
/usr/bin/abiword
/usr/bin/ac
/usr/bin/activation-client
/usr/bin/addr2line
...
输出显示了在环境变量PATH所包含的所有目录中找到的全部可执行文件，数量真是不少！

13.9.2 创建多个用户账户
shell脚本的目标是让系统管理员过得更轻松。如果你碰巧工作在一个拥有大量用户的环境
中，最烦人的工作之一就是创建新用户账户。好在可以使用while循环来降低工作的难度。
你不用为每个需要创建的新用户账户手动输入useradd命令，而是可以将需要添加的新用户
账户放在一个文本文件中，然后创建一个简单的脚本进行处理。这个文本文件的格式如下：
userid,user name
第一个条目是你为新用户账户所选用的用户ID。第二个条目是用户的全名。两个值之间使用
逗号分隔，这样就形成了一种名为逗号分隔值的文件格式（或者是.csv）。这种文件格式在电子表
格中极其常见，所以你可以轻松地在电子表格程序中创建用户账户列表，然后将其保存成.csv格
式，以备shell脚本读取及处理。
要读取文件中的数据，得用上一点shell脚本编程技巧。我们将IFS分隔符设置成逗号，并将
其放入while语句的条件测试部分。然后使用read命令读取文件中的各行。实现代码如下：
while IFS=’,’ read –r userid name
read命令会自动读取.csv文本文件的下一行内容，所以不需要专门再写一个循环来处理。当
read命令返回FALSE时（也就是读取完整个文件时），while命令就会退出。妙极了！
要想把数据从文件中送入while命令，只需在while命令尾部使用一个重定向符就可以了。
将各部分处理过程写成脚本如下。
$ cat test26
#!/bin/bash
# process new user accounts
input="users.csv"
while IFS=',' read -r userid name
do
echo "adding $userid"
useradd -c "$name" -m $userid
done < "$input"
$
$input变量指向数据文件，并且该变量被作为while命令的重定向数据。users.csv文件内容
如下。
$ cat users.csv
rich,Richard Blum
christine,Christine Bresnahan
barbara,Barbara Blum
tim,Timothy Bresnahan
$
必须作为root用户才能运行这个脚本，因为useradd命令需要root权限。
# ./test26
adding rich
adding christine
adding barbara
adding tim
#
来看一眼/etc/passwd文件，你会发现账户已经创建好了。
# tail /etc/passwd
rich:x:1001:1001:Richard Blum:/home/rich:/bin/bash
christine:x:1002:1002:Christine Bresnahan:/home/christine:/bin/bash
barbara:x:1003:1003:Barbara Blum:/home/barbara:/bin/bash
tim:x:1004:1004:Timothy Bresnahan:/home/tim:/bin/bash
#
```



# 参考文献

![image-20220106145912775](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220106145912775-16415048531181-16416260517421.png)
