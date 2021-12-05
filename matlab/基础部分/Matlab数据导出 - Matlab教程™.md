MATLAB中的数据导出(或输出)可以理解为写入文件。 MATLAB允许在其他应用程序中使用读取ASCII文件的数据。 为此，MATLAB提供了几个数据导出选项。

可以创建以下类型的文件：

*   来自数组的矩形，有分隔符的ASCII数据文件。
*   日记(或日志)文件的按键和结果文本输出。
*   使用`fprintf`等低级函数的专用ASCII文件。

MEX文件访问写入特定文本文件格式的C/C++或Fortran例程。

除此之外，还可以将数据导出到电子表格(_Excel_)。

将数字数组导出为有分隔符的ASCII数据文件有两种方法 -

*   使用`save`函数并指定`-ascii`限定符
*   使用`dlmwrite`函数

使用`save`函数的语法是：

```
save my_data.out num_array -ascii 
```

其中，`my_data.out`是创建的分隔ASCII数据文件，`num_array`是一个数字数组，`-ascii`是说明符。

使用`dlmwrite`函数的语法是：

```
dlmwrite('my_data.out', num_array, 'dlm_char') 
```

其中，`my_data.out`是分隔的ASCII数据文件，`num_array`是数组，`dlm_char`是分隔符。

示例
--

以下示例演示了这个概念。创建脚本文件并键入以下代码 -

```
num_array = [ 1 2 3 4 ; 4 5 6 7; 7 8 9 0];
save array_data1.out num_array -ascii;
type array_data1.out
dlmwrite('array_data2.out', num_array, ' ');
type array_data2.out 
```

执行上面示例代码，得到以下结果 -

```
Trial>> num_array = [ 1 2 3 4 ; 4 5 6 7; 7 8 9 0];
save array_data1.out num_array -ascii;
type array_data1.out
dlmwrite('array_data2.out', num_array, ' ');
type array_data2.out

   1.0000000e+00   2.0000000e+00   3.0000000e+00   4.0000000e+00
   4.0000000e+00   5.0000000e+00   6.0000000e+00   7.0000000e+00
   7.0000000e+00   8.0000000e+00   9.0000000e+00   0.0000000e+00

1 2 3 4
4 5 6 7
7 8 9 0 
```

请注意，保存`save -ascii`命令和`dlmwrite`函数不能使用单元格数组作为输入。要从单元格数组的内容创建一个分隔的ASCII文件，可以 -

*   使用`cell2mat`函数将单元阵列转换为矩阵
*   或使用低级文件I/O函数导出单元格数组。

如果使用`save`函数将字符数组写入ASCII文件，则会将ASCII等效字符写入该文件。

例如，把一个单词`hello`写到一个文件 -

```
h = 'hello';
save textdata.out h -ascii
type textdata.out 
```

MATLAB执行上述语句并显示以下结果。这是8位ASCII格式的字符串`“hello”`的字符。

```
1.0400000e+02   1.0100000e+02   1.0800000e+02   1.0800000e+02   1.1100000e+02 
```

写到日记文件
------

日记文件是MATLAB会话的活动日志。`diary`函数在磁盘文件中创建会话的精确副本，不包括图形。

打开`diary`函数，键入 -

```
diary 
```

或者，可以给出日志文件的名称，比如 -

```
diary diary.log 
```

关闭日记函数 -

可以在文本编辑器中打开日记文件。

将数据导出到具有低级I/O的文本数据文件
--------------------

到目前为止，我们已经导出数组。 但是，您可能需要创建其他文本文件，包括数字和字符数据的组合，非矩形输出文件或具有非ASCII编码方案的文件。为了实现这些目的，MATLAB提供了低级别的`fprintf`函数。

在低级I/O文件活动中，在导出之前，需要使用`fopen`函数打开或创建一个文件，并获取文件标识符。 默认情况下，`fopen`会打开一个只读访问的文件。所以应该指定写入或附加的权限，例如`'w'`或`'a'`。

处理文件后，需要用`fclose(fid)`函数关闭它。

以下示例演示了这一概念 -

**示例**

创建脚本文件并在其中键入以下代码 -

```
 x = 0:10:100;
y = [x; log(x)];

fid = fopen('logtable.txt', 'w');

fprintf(fid, 'Log     Function\n\n');

fprintf(fid, '%f    %f\n', y);
fclose(fid);

type logtable.txt 
```

运行文件时，会显示以下结果 -

```
Log     Function

0.000000    -Inf
10.000000    2.302585
20.000000    2.995732
30.000000    3.401197
40.000000    3.688879
50.000000    3.912023
60.000000    4.094345
70.000000    4.248495
80.000000    4.382027
90.000000    4.499810
100.000000    4.605170 
```

