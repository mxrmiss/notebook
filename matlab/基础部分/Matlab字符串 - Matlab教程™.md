在MATLAB中创建一个字符串非常简单。 事实上，在前面的示例中我们已经使用了很多次。 例如，在命令提示符下键入以下内容：

```
my_string = 'Yiibai Yiibai' 
```

MATLAB执行上述语句并返回以下结果 -

```
Trial>> my_string = 'Yiibai Yiibai'

my_string =

    'Yiibai Yiibai' 
```

MATLAB将所有变量视为数组，并将字符串视为字符数组。使用`whos`命令来检查上面创建的变量 -

```
 Trial>> whos
  Name           Size            Bytes  Class    Attributes

  ans            1x92              184  char               
  my_string      1x16               32  char               
  x              1x3               360  cell 
```

有趣的是，可以使用`uint8`或`uint16`等数字转换函数将字符串中的字符转换为数字代码。 `char`函数将整数向量转换回到字符 -

**示例**

创建脚本文件并在其中键入以下代码 -

```
my_string = 'Yiibai''s Tutorial';
str_ascii = uint8(my_string)        
str_back_to_char= char(str_ascii)  
str_16bit = uint16(my_string)       
str_back_to_char = char(str_16bit) 
```

执行上面示例代码，得到以下结果 -

```
str_ascii =

  1×17 uint8 行向量

  1 至 15 列

    89   105   105    98    97   105    39   115    32    84   117   116   111   114   105

  16 至 17 列

    97   108


str_back_to_char =

    'Yiibai's Tutorial'


str_16bit =

  1×17 uint16 行向量

  1 至 15 列

    89   105   105    98    97   105    39   115    32    84   117   116   111   114   105

  16 至 17 列

    97   108


str_back_to_char =

    'Yiibai's Tutorial' 
```

矩形字符数组
------

到目前为止，我们讨论的字符串是一维字符数组; 然而，我们需要存储更多维度的数据。在程序中存储更多的维度文本数据。这是通过创建矩形字符数组来实现的。

创建矩形字符阵列的最简单的方式是根据需要垂直或水平连接两个或更多个一维字符数组。

通过以下任一方式垂直组合字符串 -

*   使用MATLAB连接运算符`[]`并用分号(`;`)分隔每一行。 请注意，在这种方法中，每行必须包含相同数量的字符。对于不同长度的字符串，应该根据需要填充空格字符。
    
*   使用`char`函数。如果字符串的长度不同，则`char`将较短的字符串填充到尾部空白处，以使每行具有相同的字符数。
    

示例
--

创建脚本文件并在其中键入以下代码 -

```
doc_profile = ['Bara Tli                             '; ...
               'Sr. Surgeon                          '; ...
               'R N Tagore Cardiology Research Center']
doc_profile = char('Bara Tli', 'Sr. Surgeon', ...
                   'RN Tagore Cardiology Research Center') 
```

运行文件时，会显示以下结果 -

```
Trial>> doc_profile = ['Bara Tli                             '; ...
               'Sr. Surgeon                          '; ...
               'R N Tagore Cardiology Research Center']
doc_profile = char('Bara Tli', 'Sr. Surgeon', ...
                   'RN Tagore Cardiology Research Center')

doc_profile =

  3×37 char 数组

    'Bara Tli                             '
    'Sr. Surgeon                          '
    'R N Tagore Cardiology Research Center'


doc_profile =

  3×36 char 数组

    'Bara Tli                            '
    'Sr. Surgeon                         '
    'RN Tagore Cardiology Research Center' 
```

可以通过以下任一方式水平组合字符串 -

*   使用MATLAB连接运算符 - `[]`并用逗号(`;`)或空格分隔输入字符串。该方法保留输入数组中的任何尾随空格。
*   使用字符串连接函数 - `strcat`。 此方法会删除输入中的尾随空格。

**示例**

创建脚本文件并在其中键入以下代码 -

```
name =     'Myra Yli                             ';
position = 'Sr. Surgeon                          '; 
worksAt =  'R N Tagore Cardiology Research Center';
profile = [name ', ' position ', ' worksAt]
profile = strcat(name, ', ', position, ', ', worksAt) 
```

执行上面示例代码，得到以下结果 -

```
Trial>> name =     'Myra Yli                             ';
position = 'Sr. Surgeon                          '; 
worksAt =  'R N Tagore Cardiology Research Center';
profile = [name ', ' position ', ' worksAt]
profile = strcat(name, ', ', position, ', ', worksAt)

profile =

    'Myra Yli                             , Sr. Surgeon                          , R N Tagore Cardiology Research Center'


profile =

    'Myra Yli,Sr. Surgeon,R N Tagore Cardiology Research Center' 
```

将字符串组合成单元格数组
------------

从前面的学习中，很明显，组合不同长度的字符串可能会很痛苦，因为数组中的所有字符串都必须具有相同的长度。在字符串的末尾使用了空格，使其长度相等。

然而，组合字符串的更有效的方法是将生成的数组转换为单元格数组。

MATLAB单元格数组可以在数组中保存不同大小和类型的数据。单元格数组提供了一种更灵活的方法来存储不同长度的字符串。

`cellstr`函数将字符数组转换为字符串的单元格数组。

示例
--

创建脚本文件并在其中键入以下代码 -

```
name =     'Myra Tli                             ';
position = 'St. Sargeon                          '; 
worksAt =  'R N Tagore Cardiology Research Center';
profile = char(name, position, worksAt);
profile = cellstr(profile);
disp(profile) 
```

执行上面示例代码，得到以下结果 -

```
Trial>> name =     'Myra Tli                             ';
position = 'St. Sargeon                          '; 
worksAt =  'R N Tagore Cardiology Research Center';
profile = char(name, position, worksAt);
profile = cellstr(profile);
disp(profile)
    'Myra Tli'
    'St. Sargeon'
    'R N Tagore Cardiology Research Center' 
```

MATLAB中的字符串函数
-------------

MATLAB提供了许多字符串函数来创建，组合，解析，比较和操作字符串。

下表简要介绍了MATLAB中的字符串函数。

用于存储字符数组中的文本，组合字符数组等的函数 -

| 函数 | 描述 |
| --- | --- |
| `blanks` | 创建空白字符串 |
| `cellstr` | 从字符数组创建字符串数组 |
| `char` | 转换为字符数组(字符串) |
| `iscellstr` | 确定输入是字符串的单元格数组 |
| `ischar` | 确定项目是否是字符数组 |
| `sprintf` | 将数据格式化为字符串 |
| `strcat` | 水平连接字符串 |
| `strjoin` | 将单元格中的字符串连接到单个字符串中 |

识别字符串部分，查找和替换子串的函数 -

| 函数 | 描述 |
| --- | --- |
| `ischar` | 确定项目是否是字符数组 |
| `isletter` | 数组元素是否为字母 |
| `isspace` | 数组元素是空格 |
| `isstrprop` | 确定字符串是否是指定的类别 |
| `sscanf` | 从字符串读取格式化数据 |
| `strfind` | 在另一个字符串中查找一个字符串 |
| `strrep` | 查找并替换子串 |
| `strsplit` | 在指定的分隔符处拆分字符串 |
| `strtok` | 字符串的选定部分 |
| `validatestring` | 检查文本字符串的有效性 |
| `symvar` | 确定表达式中的符号变量 |
| `regexp` | 匹配正则表达式(区分大小写) |
| `regexpi` | 匹配正则表达式(不区分大小写) |
| `regexprep` | 用正则表达式替换字符串 |
| `regexptranslate` | 用正则表达式替换字符串 |

字符串比较的函数 -

| 函数 | 描述 |
| --- | --- |
| `strcmp` | 比较字符串(区分大小写) |
| `strcmpi` | 比较字符串(不区分大小写) |
| `strncmp` | 比较字符串的前`n`个字符(区分大小写) |
| `strncmpi` | 比较字符串的前`n`个字符(不区分大小写) |

将字符串更改为大写或小写，创建或删除空格的函数 -

| 函数 | 描述 |
| --- | --- |
| `deblank` | 从字符串末尾剥去尾随空格 |
| `strtrim` | 从字符串中删除前导和尾随的空格 |
| `lower` | 将字符串转换为小写 |
| `upper` | 将字符串转换为大写字母 |
| `strjust` | 对齐字符数组 |

例子
--

以下示例说明了一些上述字符串函数 -

**格式化字符串**  
创建脚本文件并在其中键入以下代码 -

```
A = pi*1000*ones(1,5);
sprintf(' %f \n %.2f \n %+.2f \n %12.2f \n %012.2f \n', A) 
```

执行上面示例代码，得到以下结果 -

```
ans =  3141.592654 
 3141.59 
 +3141.59 
      3141.59 
 000003141.59 
```

**字符串连接**

创建脚本文件并在其中键入以下代码 -

```
 str_array = {'red','blue','green', 'yellow', 'orange'};

str1 = strjoin(str_array, "-")
str2 = strjoin(str_array, ",") 
```

执行上面示例代码，得到以下结果 -

```
str1 = red-blue-green-yellow-orange
str2 = red,blue,green,yellow,orange 
```

**查找和替换字符串**

创建脚本文件并在其中键入以下代码 -

```
students = {'Bara Ali', 'Neha Bhatnagar', ...
            'Nonica Malik', 'Madhu Gautam', ...
            'Nadhu Sharma', 'Bhawna Sharma',...
            'Muha Ali', 'Reva Dutta', ...
            'Tunaina Ali', 'Sofia Kabir'};

new_student = strrep(students(8), 'Reva', 'Poulomi')

first_names = strtok(students) 
```

执行上面示例代码，得到以下结果 -

```
Trial>> students = {'Bara Ali', 'Neha Bhatnagar', ...
            'Nonica Malik', 'Madhu Gautam', ...
            'Nadhu Sharma', 'Bhawna Sharma',...
            'Muha Ali', 'Reva Dutta', ...
            'Tunaina Ali', 'Sofia Kabir'};

% The strrep function searches and replaces sub-string.
new_student = strrep(students(8), 'Reva', 'Poulomi')
% Display first names
first_names = strtok(students)

new_student =

  1×1 cell 数组

    {'Poulomi Dutta'}


first_names =

  1×10 cell 数组

  1 至 7 列

    {'Bara'}    {'Neha'}    {'Nonica'}    {'Madhu'}    {'Nadhu'}    {'Bhawna'}    {'Muha'}

  8 至 10 列

    {'Reva'}    {'Tunaina'}    {'Sofia'} 
```

**比较字符串**

创建脚本文件并在其中键入以下代码 -

```
str1 = 'This is test'
str2 = 'This is text'
if (strcmp(str1, str2))
 sprintf('%s and %s are equal', str1, str2)
else
 sprintf('%s and %s are not equal', str1, str2)
end 
```

执行上面示例代码，得到以下结果 -

```
str1 = This is test
str2 = This is text
ans = This is test and This is text are not equal 
```

