MATLAB支持包括有符号和无符号整数以及单精度和双精度浮点数的各种数字类型。 默认情况下，MATLAB将所有数值存储为双精度浮点数。

可以选择将任何数字或数组的数字存储为整数或单精度数字。

所有数字类型都支持基本的数组运算和数学运算。

转换为各种数值数据类型
-----------

MATLAB提供以下函数来将数值转换为各种数字数据类型 -

| 函数 | 描述说明 |
| --- | --- |
| `double` | 转换为双精度数 |
| `single` | 转换为单精度数 |
| `int8` | 转换为`8`位有符号整数 |
| `int16` | 转换为`16`位有符号整数 |
| `int32` | 转换为`32`位有符号整数 |
| `int64` | 转换为`64`位有符号整数 |
| `uint8` | 转换为`8`位无符号整数 |
| `uint16` | 转换为`16`位无符号整数 |
| `uint32` | 转换为`32`位无符号整数 |
| `uint64` | 转换为`64`位无符号整数 |

**示例**

创建脚本文件并键入以下代码 -

```
x = single([5.32 3.47 6.28]) .* 7.5
x = double([5.32 3.47 6.28]) .* 7.5
x = int8([5.32 3.47 6.28]) .* 7.5
x = int16([5.32 3.47 6.28]) .* 7.5
x = int32([5.32 3.47 6.28]) .* 7.5
x = int64([5.32 3.47 6.28]) .* 7.5 
```

执行上面示例代码，得到以下结果 -

```
x =

   39.900   26.025   47.100

x =

   39.900   26.025   47.100

x =

  38  23  45

x =

  38  23  45

x =

  38  23  45

x =

  38  23  45 
```

**示例**

让我们再来扩展上面的例子。 创建脚本文件并键入以下代码 -

```
x = int32([5.32 3.47 6.28]) .* 7.5
x = int64([5.32 3.47 6.28]) .* 7.5
x = num2cell(x) 
```

执行上面示例代码，得到以下结果 -

```
Trial>> x = int32([5.32 3.47 6.28]) .* 7.5
x = int64([5.32 3.47 6.28]) .* 7.5
x = num2cell(x)

x =

  1×3 int32 行向量

   38   23   45


x =

  1×3 int64 行向量

   38   23   45


x =

  1×3 cell 数组

    {[38]}    {[23]}    {[45]} 
```

最小和最大的整数
--------

`intmax()`和`intmin()`函数返回可以用所有类型的整数表示的最大值和最小值。

这两个函数将整数数据类型作为参数，例如`int_max(int8)`或`intmin(int64)`，并返回可以使用整数数据类型表示的最大值和最小值。

**示例**

以下示例说明如何获取最小和最大的整数值。 创建脚本文件并在其中写下面的代码 -

```
 str = 'The range for int8 is:\n\t%d to %d ';
sprintf(str, intmin('int8'), intmax('int8'))
str = 'The range for int16 is:\n\t%d to %d ';
sprintf(str, intmin('int16'), intmax('int16'))
str = 'The range for int32 is:\n\t%d to %d ';
sprintf(str, intmin('int32'), intmax('int32'))
str = 'The range for int64 is:\n\t%d to %d ';
sprintf(str, intmin('int64'), intmax('int64'))

str = 'The range for uint8 is:\n\t%d to %d ';
sprintf(str, intmin('uint8'), intmax('uint8'))
str = 'The range for uint16 is:\n\t%d to %d ';
sprintf(str, intmin('uint16'), intmax('uint16'))
str = 'The range for uint32 is:\n\t%d to %d ';
sprintf(str, intmin('uint32'), intmax('uint32'))
str = 'The range for uint64 is:\n\t%d to %d ';
sprintf(str, intmin('uint64'), intmax('uint64')) 
```

执行上面示例代码，得到以下结果 -

```
ans =

    'The range for int8 is:
         -128 to 127 '


ans =

    'The range for int16 is:
         -32768 to 32767 '


ans =

    'The range for int32 is:
         -2147483648 to 2147483647 '


ans =

    'The range for int64 is:
         -9223372036854775808 to 9223372036854775807 '


ans =

    'The range for uint8 is:
         0 to 255 '


ans =

    'The range for uint16 is:
         0 to 65535 '


ans =

    'The range for uint32 is:
         0 to 4294967295 '


ans =

    'The range for uint64 is:
         0 to 1.844674e+19 ' 
```

最小和最大的浮点数
---------

`realmax()`和`realmin()`函数返回可以用浮点数表示的最大值和最小值。

当使用参数`'single'`调用这两个函数时，返回使用单精度数据类型表示的最大值和最小值，当使用参数`'double'`调用时，返回可以表示的最大值和最小值的双精度数据类型。

**示例**

以下示例说明如何获取最小和最大的浮点数。 创建脚本文件并在其中写下面的代码 -

```
 str = 'The range for single is:\n\t%g to %g and\n\t %g to  %g';
sprintf(str, -realmax('single'), -realmin('single'), ...
   realmin('single'), realmax('single'))

str = 'The range for double is:\n\t%g to %g and\n\t %g to  %g';
sprintf(str, -realmax('double'), -realmin('double'), ...
   realmin('double'), realmax('double')) 
```

执行上面示例代码，得到以下结果 -

```
ans =

    'The range for single is:
         -3.40282e+38 to -1.17549e-38 and
          1.17549e-38 to  3.40282e+38'


ans =

    'The range for double is:
         -1.79769e+308 to -2.22507e-308 and
          2.22507e-308 to  1.79769e+308' 
```

  
