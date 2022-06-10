---
title: Linux shell(1)-构建基本脚本
date: 2022/1/6
categories: linux
tags: [linux, shell]
index_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_25.jpg
banner_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_25.jpg
---

# 1. 创建shell脚本文件

- 创建shell脚本文件, 实例命令如下：

```
#!/bin/bash
```

- 在脚本的第一行中写入该命令，感叹号！后面的是使用的shell的路径，上面命令中使用的就是 bash shell，如果需要使用其他shell，如zsh，可以写其他shell的路径

## 1.1 加入环境变量

- 可以将自己创建的shell脚本文件加入环境变量中，详情见笔记手札 《Linux环境变量》

## 1.2 执行shell脚本

- 执行shell脚本首先需要给shell脚本赋予执行权限，如果没有的话，当然一个脚本被创建时一般都是没有执行权限的，命令如下：

```
sudo chmod +x 脚本.sh
```

# 2. 显示消息

- 使用 echo 显示输出的文本

```
#!/bin/bash
# This script displays the date and who's logged on
echo The time and date are:
date
echo "Let's see who's logged into the system:"
who
```

- 当运行这个脚本时，它会产生如下输出。

```
The time and date are:
Mon Feb 21 15:41:13 EST 2014
Let's see who's logged into the system:
Christine tty2 2014-02-21 15:26
Samantha tty3 2014-02-21 15:26
Timothy tty1 2014-02-21 15:26
user tty7 2014-02-19 14:03 (:0)
user pts/0 2014-02-21 15:21 (:0.0)
```

- 一般shell脚本中的命令与文本是不同行输出的，**如果想让文本与命令在同一行输入**，可以使用echo的-n参数，修改上述脚本：

```
echo -n "The time and date are: "
```

- 你需要在字符串的两侧使用引号，保证要显示的字符串尾部有一个空格。命令输出将会在紧接着字符串结束的地方出现。现在的输出会是这样：

```
$ ./test1
The time and date are: Mon Feb 21 15:42:23 EST 2014
Let's see who's logged into the system:
Christine tty2 2014-02-21 15:26
Samantha tty3 2014-02-21 15:26
Timothy tty1 2014-02-21 15:26
user tty7 2014-02-19 14:03 (:0) 
```



# 3. 使用变量

## 3.1 环境变量

- 在使用环境变量的时候，可以使用美元符号 $ 引用环境变量，但是在给环境变量赋值的时候不需要使用美元符号，如果需要显示美元符号，可以在美元符号前面使用反斜杠来转置

## 3.2 用户变量

- 除了环境变量外，shell脚本还允许我们自定义用户变量，用户变量区分大小写，并且在对用户变量赋值的时候等于号 = 两边不能够有空格

# 4. 命令替换

- 使用反引号或 $() 对命令进行替换

- 要么用一对反引号把整个命令行命令围起来：
  `testing='date'`
- 要么使用$()格式：
  `testing=$(date)`
- shell会运行命令替换符号中的命令，并将其输出赋给变量testing。注意，赋值等号和命令替换字符之间没有空格。这里有个使用普通的shell命令输出创建变量的例子。

```
$ cat test5
#!/bin/bash
testing=$(date)
echo "The date and time are: " $testing
$
```

- 变量testing获得了date命令的输出，然后使用echo语句显示出它的值。运行这个shell脚本生成如下输出。

```
$ chmod u+x test5
$ ./test5
The date and time are: Mon Jan 31 20:23:25 EDT 2014
```

- 命令替换会创建一个子shell来运行对应的命令, 在命令行提示符下使用路径./运行命令的话，也会创建出子shell, 如果你使用的是[**内建的shell命令**](http://c.biancheng.net/view/1136.html)，并不会涉及子shell。

#  5. 重定向输入和输出

## 5.1 重定向输出

- 如果输出文件已经存在了，重定向操作符 > 会用新的文件数据覆盖已有文件。
-  有时，你可能并不想覆盖文件原有内容，而是想要将命令的输出追加到已有文件中，比如你正 在创建一个记录系统上某个操作的日志文件。在这种情况下，可以用双大于号（>>）来追加数据。

## 5.2 重定向输入

```
输入重定向和输出重定向正好相反。输入重定向将文件的内容重定向到命令，而非将命令的
输出重定向到文件。
输入重定向符号是小于号（<）：
command < inputfile

这里有个和wc命令一起使用输入重定向的例子。
$ wc < test6
 2 11 60
$
wc命令可以对对数据中的文本进行计数。默认情况下，它会输出3个值：
 文本的行数
 文本的词数
 文本的字节数
通过将文本文件重定向到wc命令，你立刻就可以得到文件中的行、词和字节的计数。这个例
子说明test6文件有2行、11个单词以及60字节。

还有另外一种输入重定向的方法，称为内联输入重定向（inline input redirection）。这种方法
无需使用文件进行重定向，只需要在命令行中指定用于输入重定向的数据就可以了。乍看一眼，
这可能有点奇怪，但有些应用会用到这种方式。
内联输入重定向符号是远小于号（<<）。除了这个符号，你必须指定一个文本标记来划分输
入数据的开始和结尾。任何字符串都可作为文本标记，但在数据的开始和结尾文本标记必须一致。
command << marker
data
marker
在命令行上使用内联输入重定向时，shell会用PS2环境变量中定义的次提示符（参见第6章）
来提示输入数据。下面是它的使用情况。
$ wc << EOF
> test string 1
> test string 2
> test string 3
> EOF 

 3 9 42
$
次提示符会持续提示，以获取更多的输入数据，直到你输入了作为文本标记的那个字符串。
wc命令会对内联输入重定向提供的数据进行行、词和字节计数。
```



# 6. 执行数学运算

## 6.1 使用方括号

- 在bash中，在将一个数学运算结果赋给某个变量时，可以用美元符和 方括号（$[ operation ]）将数学表达式围起来。

 ```
$ var1=$[1 + 5]
$ echo $var1
6 
 ```

- 注意在使用方括号来计算公式时，不用担心shell会误解乘号或其他符号。shell知道它不是通配符，因为它在方括号内。

- bash shell数学运算符只支持整数运算。
- z shell（zsh）提供了完整的浮点数算术操作。如果需要在shell脚本中进行浮点数运算，可 以考虑看看z shell

## 6.2 expr 命令 （不使用）

- 最开始，Bourne shell提供了一个特别的命令用来处理数学表达式。expr命令允许在命令行 上处理数学表达式，但是特别笨拙。

- 要解决这个问题，对于那些容易被shell错误解释的字符，在它们传入expr命令之前，需要使 用shell的转义字符（反斜线）将其标出来。

 `$ expr 5 \* 2`

# 7. 退出脚本

- 正确退出脚本时的状态码是0，不正确则为其他数字，查看状态码的命令是`echo $?`
- 下面是状态码一览表

![image-20220106145335181](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220106145335181.png)

## 自定义退出状态码

- 使用exit，可以在脚本的末尾自定义退出状态码

```
#!/bin/bash
# testing the exit status
var1=10
var2=30
var3=$[$var1 + $var2]
echo The answer is $var3
exit 5
```

- 当查看脚本的退出码时，你会得到作为参数传给exit命令的值。

```
$ chmod u+x test13
$ ./test13
The answer is 40
$ echo $?
5
```

# 参考文献

![image-20220106145912775](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220106145912775.png)
