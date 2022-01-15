---
title: python正则表达式
date: 2022/1/12
categories: python
tags: [python, 正则]
top_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_31.jpg
cover: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_31.jpg
---

# 1. re 模块

- re 模块使 Python 语言拥有全部的正则表达式功能。
- compile 函数根据一个模式字符串和可选的标志参数生成一个正则表达式对象。该对象拥有一系列方法用于正则表达式匹配和替换，与re模块有同样的正则函数
- [官方说明文档](https://docs.python.org/zh-cn/3/library/re.html#module-contents)

# 2. re.match 函数

- re.match 尝试从字符串的起始位置匹配一个模式
- 匹配成功返回一个匹配的对象，否则返回None。
- 函数语法：

```
re.match(pattern,  string, flags\=0)
```

- 函数参数说明：

| 参数    | 描述                                                         |
| ------- | ------------------------------------------------------------ |
| pattern | 匹配的正则表达式                                             |
| string  | 要匹配的字符串。                                             |
| flags   | 标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。参见：[正则表达式修饰符 - 可选标志]( |

- 我们可以使用group(num) 或 groups() 匹配对象函数来获取匹配表达式。

| 匹配对象方法 | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| group(num=0) | 匹配的整个表达式的字符串，group() 可以一次输入多个组号，在这种情况下它将返回一个包含那些组所对应值的元组。 |
| groups()     | 返回一个包含所有小组字符串的元组，从 1 到 所含的小组号。     |

- 案例：

```python
#!/usr/bin/python3
import re
 
line = "Cats are smarter than dogs"
# .* 表示任意匹配除换行符（\n、\r）之外的任何单个或多个字符
# (.*?) 表示"非贪婪"模式，只保存第一个匹配到的子串
matchObj = re.match( r'(.*) are (.*?) .*', line, re.M|re.I)
 
if matchObj:
   print ("matchObj.group() : ", matchObj.group())
   print ("matchObj.group(1) : ", matchObj.group(1))
   print ("matchObj.group(2) : ", matchObj.group(2))
else:
   print ("No match!!")

==================================================
# 结果：
matchObj.group()  :  Cats are smarter than dogs
matchObj.group(1)  :  Cats 
matchObj.group(2)  : smarter
```

# 3. re.search函数

- re.search 扫描整个字符串并返回第一个成功的匹配。
- 匹配成功返回一个匹配的对象，否则返回None。

- 函数语法：

  ```
  re.search(pattern,  string, flags\=0)
  ```


- 函数参数说明：

| 参数    | 描述                                                         |
| ------- | ------------------------------------------------------------ |
| pattern | 匹配的正则表达式                                             |
| string  | 要匹配的字符串。                                             |
| flags   | 标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。 |

- 我们可以使用group(num) 或 groups() 匹配对象函数来获取匹配表达式。

| 匹配对象方法 | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| group(num=0) | 匹配的整个表达式的字符串，group() 可以一次输入多个组号，在这种情况下它将返回一个包含那些组所对应值的元组。 |
| groups()     | 返回一个包含所有小组字符串的元组，从 1 到 所含的小组号。     |

- 案例：

```python
#!/usr/bin/python3
 
import re
 
line = "Cats are smarter than dogs"
 
searchObj = re.search( r'(.*) are (.*?) .*', line, re.M|re.I)
 
if searchObj:
   print ("searchObj.group() : ", searchObj.group())
   print ("searchObj.group(1) : ", searchObj.group(1))
   print ("searchObj.group(2) : ", searchObj.group(2))
else:
   print ("Nothing found!!")

=================================================
# 结果：
searchObj.group()  :  Cats are smarter than dogs
searchObj.group(1)  :  Cats 
searchObj.group(2)  : smarter
```

# 4. 替换字符串

- Python 的re模块提供了re.sub用于替换字符串中的匹配项。

- 语法：

```
re.sub(pattern, repl,  string, count\=0, flags\=0)
```

- 参数：

>*   pattern : 正则中的模式字符串。
>*   repl : 替换的字符串，也可为一个函数。
>*   string : 要被查找替换的原始字符串。
>*   count : 模式匹配后替换的最大次数，默认 0 表示替换所有的匹配。
>*   flags : 编译时用的匹配模式，数字形式。

- 前三个为必选参数，后两个为可选参数。

- repl 为字符串，案例：

```python
#!/usr/bin/python3
import re
 
phone = "2004-959-559 # 这是一个电话号码"
 
# 删除注释
num = re.sub(r'#.*$', "", phone)
print ("电话号码 : ", num)
 
# 移除非数字的内容
num = re.sub(r'\D', "", phone)
print ("电话号码 : ", num)

========================================
# 结果：
电话号码 :  2004-959-559 
电话号码 :  2004959559
```

- repl 是一个函数，案例：

```python
#!/usr/bin/python
 
import re
 
# 将匹配的数字乘于 2
def double(matched):
    value = int(matched.group('value'))
    return str(value * 2)
 
s = 'A23G4HFD567'
print(re.sub('(?P<value>\d+)', double, s))
==============================================
# 结果：
A46G8HFD1134
```

# 5. compile 函数

- compile 函数用于编译正则表达式，生成一个正则表达式（ Pattern ）对象

- 语法格式为：

```
re.compile(pattern[, flags])
```

>**参数：**
>
>- pattern : 一个字符串形式的正则表达式
>
>- **flags** 可选，表示匹配模式，比如忽略大小写，多行模式等，具体参数为：
>
>  >- re.I 忽略大小写
>  >- re.L 表示特殊字符集 \w, \W, \b, \B, \s, \S 依赖于当前环境
>  >- re.M 多行模式
>  >- re.S 即为' . '并且包括换行符在内的任意字符（' . '不包括换行符）
>  >- re.U 表示特殊字符集 \w, \W, \b, \B, \d, \D, \s, \S 依赖于 Unicode 字符属性数据库
>  >- re.X 为了增加可读性，忽略空格和' # '后面的注释

- 案例：

```python
>>>import re
>>> pattern = re.compile(r'\d+')                    # 用于匹配至少一个数字 # pattern为正则表达式对象
>>> m = pattern.match('one12twothree34four')        # 查找头部，没有匹配 # pattern对象使用函数
>>> print( m )
None
>>> m = pattern.match('one12twothree34four', 2, 10) # 从'e'的位置开始匹配，没有匹配
>>> print( m )
None
>>> m = pattern.match('one12twothree34four', 3, 10) # 从'1'的位置开始匹配，正好匹配
>>> print( m )                                        # 返回一个 Match 对象
<_sre.SRE_Match object at 0x10a42aac0>
>>> m.group(0)   # 可省略 0
'12'
>>> m.start(0)   # 可省略 0
3
>>> m.end(0)     # 可省略 0
5
>>> m.span(0)    # 可省略 0
(3, 5)
```

>- `group([group1, …])` 方法用于获得一个或多个分组匹配的字符串，当要获得整个匹配的子串时，可直接使用 `group()` 或 `group(0)`；
>- `start([group])` 方法用于获取分组匹配的子串在整个字符串中的起始位置（子串第一个字符的索引），参数默认值为 0；
>- `end([group])` 方法用于获取分组匹配的子串在整个字符串中的结束位置（子串最后一个字符的索引+1），参数默认值为 0；
>- `span([group])` 方法返回 `(start(group), end(group))`。

## 注意

- 在底层代码中，re 函数于compile 函数调用的是同一个函数，根据官方手册给出的建议，当匹配次数较少的时候两种方法差别不大，但当匹配次数较多的时候使用compile生成对象更好

# 6. findall 函数

- 在字符串中找到正则表达式所匹配的所有子串，并返回一个列表，**如果有多个匹配模式**，则返回元组列表，如果没有找到匹配的，则返回空列表。

```python
re.findall(pattern, string, flags=0)
或
pattern.findall(string[, pos[, endpos]]) # pattern对象正则
```

- 参数：

>- **pattern** 匹配模式。
>- **string** 待匹配的字符串。
>- **pos** 可选参数，指定字符串的起始位置，默认为 0。
>- **endpos** 可选参数，指定字符串的结束位置，默认为字符串的长度。

- 案例：

```python
import re
 
result1 = re.findall(r'\d+','runoob 123 google 456')
 
pattern = re.compile(r'\d+')   # 查找数字
result2 = pattern.findall('runoob 123 google 456')
result3 = pattern.findall('run88oob123google456', 0, 10)
 
print(result1)
print(result2)
print(result3)

======================================================
# 结果：
['123', '456']
['123', '456']
['88', '12']
```

- 多个匹配模式，返回元组列表：

```python
import re

result = re.findall(r'(\w+)=(\d+)', 'set width=20 and height=10')
print(result)

==============================================
# 结果：
[('width', '20'), ('height', '10')]
```

# 7. finditer 函数

- 和 findall 类似，在字符串中找到正则表达式所匹配的所有子串，并把它们作为一个迭代器返回

```
re.finditer(pattern, string, flags=0)
```

- 两个方法基本类似，只不过一个是返回列表，一个是返回迭代器。我们知道列表是一次性生成在内存中，而迭代器是需要使用时一点一点生成出来的，内存使用更优。

- 如果可能存在大量的匹配项的话，建议使用**finditer函数**，一般情况使用**findall函数**基本没啥影响。

- 参数：

| 参数    | 描述                                                         |
| ------- | ------------------------------------------------------------ |
| pattern | 匹配的正则表达式                                             |
| string  | 要匹配的字符串。                                             |
| flags   | 标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。 |

- 案例：

```python
import re
 
it = re.finditer(r"\d+","12a32bc43jf3") 
for match in it: 
    print (match.group() )
    
==============================================
# 结果：
12 
32 
43 
3
```

# 8. split 函数

- split 方法按照能够匹配的子串将字符串分割后返回列表，它的使用形式如下：

```
re.split(pattern, string[, maxsplit=0, flags=0])
```

- 参数：

| 参数     | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| pattern  | 匹配的正则表达式                                             |
| string   | 要匹配的字符串。                                             |
| maxsplit | 分割次数，maxsplit=1 分割一次，默认为 0，不限制次数。        |
| flags    | 标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。参见：[正则表达式修饰符 - 可选标志](#flags) |

- 案例：

```
>>>import re
>>> re.split('\W+', 'runoob, runoob, runoob.')
['runoob', 'runoob', 'runoob', '']
>>> re.split('(\W+)', ' runoob, runoob, runoob.') 
['', ' ', 'runoob', ', ', 'runoob', ', ', 'runoob', '.', '']
>>> re.split('\W+', ' runoob, runoob, runoob.', 1) 
['', 'runoob, runoob, runoob.']
 
>>> re.split('a*', 'hello world')   # 对于一个找不到匹配的字符串而言，split 不会对其作出分割
['hello world']
```

- **`str`模块也有一个 split函数 ，那这两个函数该怎么选呢？**

>str.split函数功能简单，不支持正则分割，而re.split支持正则。在 不需要正则支持 且 数据量和数次不多 的情况下使用`str.split`函数更合适，反之则使用`re.split`函数。

# 9 . 速查表

>- 模式字符串使用特殊的语法来表示一个正则表达式。
>
>- 字母和数字表示他们自身。一个正则表达式模式中的字母和数字匹配同样的字符串。
>- 多数字母和数字前加一个反斜杠时会拥有不同的含义。
>- 标点符号只有被转义时才匹配自身，否则它们表示特殊的含义。
>- 反斜杠本身需要使用反斜杠转义。
>- 由于正则表达式通常都包含反斜杠，所以你最好使用原始字符串来表示它们。模式元素(如 r'\\t'，等价于 \\\\t )匹配相应的特殊字符。
>- 下表列出了正则表达式模式语法中的特殊元素。如果你使用模式的同时提供了可选的标志参数，某些模式元素的含义会改变。

| 模式           | 描述                                                         |
| -------------- | ------------------------------------------------------------ |
| ^              | 匹配字符串的开头                                             |
| $              | 匹配字符串的末尾。                                           |
| .              | 匹配任意字符，除了换行符，当re.DOTALL标记被指定时，则可以匹配包括换行符的任意字符。 |
| \[...\]        | 用来表示一组字符,单独列出：\[amk\] 匹配 'a'，'m'或'k'        |
| \[^...\]       | 不在\[\]中的字符：\[^abc\] 匹配除了a,b,c之外的字符。         |
| re\*           | 匹配0个或多个的表达式。                                      |
| re+            | 匹配1个或多个的表达式。                                      |
| re?            | 匹配0个或1个由前面的正则表达式定义的片段，非贪婪方式         |
| re{ n}         | 匹配n个前面表达式。例如，"o{2}"不能匹配"Bob"中的"o"，但是能匹配"food"中的两个o。 |
| re{ n,}        | 精确匹配n个前面表达式。例如，"o{2,}"不能匹配"Bob"中的"o"，但能匹配"foooood"中的所有o。"o{1,}"等价于"o+"。"o{0,}"则等价于"o\*"。 |
| re{ n, m}      | 匹配 n 到 m 次由前面的正则表达式定义的片段，贪婪方式         |
| a              | b                                                            |
| (re)           | 匹配括号内的表达式，也表示一个组                             |
| (?imx)         | 正则表达式包含三种可选标志：i, m, 或 x 。只影响括号中的区域。 |
| (?-imx)        | 正则表达式关闭 i, m, 或 x 可选标志。只影响括号中的区域。     |
| (?: re)        | 类似 (...), 但是不表示一个组                                 |
| (?imx: re)     | 在括号中使用i, m, 或 x 可选标志                              |
| (?-imx: re)    | 在括号中不使用i, m, 或 x 可选标志                            |
| (?#...)        | 注释.                                                        |
| (?= re)        | 前向肯定界定符。如果所含正则表达式，以 ... 表示，在当前位置成功匹配时成功，否则失败。但一旦所含表达式已经尝试，匹配引擎根本没有提高；模式的剩余部分还要尝试界定符的右边。 |
| (?! re)        | 前向否定界定符。与肯定界定符相反；当所含表达式不能在字符串当前位置匹配时成功。 |
| (?> re)        | 匹配的独立模式，省去回溯。                                   |
| \\w            | 匹配数字字母下划线                                           |
| \\W            | 匹配非数字字母下划线                                         |
| \\s            | 匹配任意空白字符，等价于 \[\\t\\n\\r\\f\]。                  |
| \\S            | 匹配任意非空字符                                             |
| \\d            | 匹配任意数字，等价于 \[0-9\]。                               |
| \\D            | 匹配任意非数字                                               |
| \\A            | 匹配字符串开始                                               |
| \\Z            | 匹配字符串结束，如果是存在换行，只匹配到换行前的结束字符串。 |
| \\z            | 匹配字符串结束                                               |
| \\G            | 匹配最后匹配完成的位置。                                     |
| \\b            | 匹配一个单词边界，也就是指单词和空格间的位置。例如， 'er\\b' 可以匹配"never" 中的 'er'，但不能匹配 "verb" 中的 'er'。 |
| \\B            | 匹配非单词边界。'er\\B' 能匹配 "verb" 中的 'er'，但不能匹配 "never" 中的 'er'。 |
| \\n, \\t, 等。 | 匹配一个换行符。匹配一个制表符, 等                           |
| \\1...\\9      | 匹配第n个分组的内容。                                        |
| \\10           | 匹配第n个分组的内容，如果它经匹配。否则指的是八进制字符码的表达式。 |


-------

# 参考链接

- [参考链接1](https://www.runoob.com/regexp/regexp-syntax.html)
- [参考链接2](https://zhuanlan.zhihu.com/p/127807805)

