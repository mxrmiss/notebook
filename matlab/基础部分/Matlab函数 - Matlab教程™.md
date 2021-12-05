一个函数是一组在一起执行任务的语句。 在MATLAB中，函数在单独的文件中定义。文件的名称和函数的名称应该是一样的。

函数在自己的工作空间内的变量上运行，这个变量也称为本地工作空间，与在MATLAB命令提示符下访问的工作区(称为_基本工作区_)不同。

函数可以接受多个输入参数，并可能返回多个输出参数。

函数语句的语法是 -

```
function [out1,out2, ..., outN] = myfun(in1,in2,in3, ..., inN) 
```

示例
--

以下名称为`mymax`的函数应写入名称为`mymax.m`的文件中。它需要五个数字作为参数，并返回参数数字值的最大值。

创建名为`mymax.m`的函数文件，从左上角菜单中点击_新建_\->_函数_，并在其中键入以下代码 -

```
function max = mymax(n1, n2, n3, n4, n5)

max =  n1;
if(n2 > max)
    max = n2;
end
if(n3 > max)
   max = n3;
end
if(n4 > max)
    max = n4;
end
if(n5 > max)
    max = n5;
end 
```

上面示例代码中，函数的第一行以关键字`function`开头。它给出了函数的名称和参数的顺序。在这个例子中，`mymax`函数有五个输入参数和一个输出参数。

函数语句之后的注释行提供了帮助文本。当键入时，这些行被打印 -

```
Trial>> help mymax
 This function calculates the maximum of the
  five numbers given as input 
```

现在，我们来调用这个函数 -

```
mymax(11,22,35,81,198) 
```

执行上面代码，得到以下结果 -

```
Trial>> mymax(11,22,35,81,198)

ans =

   198 
```

匿名函数
----

匿名函数就像传统编程语言中的内联函数，在单个MATLAB语句中定义。 它由单个MATLAB表达式和任意数量的输入和输出参数组成。

可以在MATLAB命令行或函数或脚本中定义一个匿名函数。

这样就可以创建简单的函数，而无需为它们创建一个文件。

从表达式创建匿名函数的语法是 -

```
f = @(arglist)expression 
```

**示例**

在这个例子中，编写一个名为`power`的匿名函数，它将使用两个数字作为输入，并将第一个数字返回到第二个数字的幂值。

创建脚本文件并在其中键入以下代码 -

```
power = @(x, n) x.^n;
result1 = power(7, 3)
result2 = power(49, 0.5)
result3 = power(10, -10)
result4 = power (4.5, 1.5) 
```

当运行该文件，得到以下结果 -

```
result1 =  343
result2 =  7
result3 =  1.0000e-10
result4 =  9.5459 
```

主函数和次函数
-------

必须在文件中定义除了匿名函数以外的其它任何函数。每个函数文件包含主要出现的必需的主函数，以及主函数之后的任意数量的可选子函数。

可以从命令行或其他函数的文件外部调用主函数，但不能从命令行或函数文件外的其他函数调用子函数。

子函数仅对函数文件中的主函数和其他子函数可见。

示例
--

下面编写一个名为`quadratic`的函数来计算二次方程的根。该函数需要三个输入参数：二次系数，线性系数和常数项。计算并会返回根。

函数文件`quadratic.m`将包含主函数`quadratic`和次函数和子函数`disc`，它计算判别式。

创建一个函数文件`quadratic.m`并在其中键入以下代码 -

```
function [x1,x2] = quadratic(a,b,c)

d = disc(a,b,c); 
x1 = (-b + d) / (2*a);
x2 = (-b - d) / (2*a);
end 

function dis = disc(a,b,c) 

dis = sqrt(b^2 - 4*a*c);
end 
```

可以从命令提示符调用上述函数 -

```
Trial>> quadratic(2,4,-4)

ans =

    0.7321 
```

嵌套函数
----

可以在一个函数的主体内定义另一个函数。这样的函数被称为嵌套函数。嵌套函数包含任何其他函数的部分或全部组件。

嵌套函数在另一个函数的范围内定义，并且它们共享对包含函数的工作空间的访问。

嵌套函数遵循以下语法 -

```
function x = A(p1, p2)
...
B(p2)
   function y = B(p3)
   ...
   end
...
end 
```

**示例**

下面来重写`quadratic`函数，从上一个例子来看，然而这次`disc`函数将是一个嵌套函数。

创建一个函数文件`quadratic2.m`并在其中键入以下代码 -

```
function [x1,x2] = quadratic2(a,b,c)
function disc  
d = sqrt(b^2 - 4*a*c);
end 
disc;
x1 = (-b + d) / (2*a);
x2 = (-b - d) / (2*a);
end 
```

在命令行窗口中调用`quadratic2.m`函数，得到以下结果 -

```
Trial>> quadratic2(2,4,-4)

ans =

    0.7321 
```

私有函数
----

私有函数是仅在有限的其他函数组中可见的主函数。如果不想公开函数的实现，则可以将其创建为私有函数。

私有函数处在在专用名称为私有的子文件夹中。

它们只对父文件夹中的函数可见。

示例
--

下面来重写二次函数。这一次，计算判别式的`disc`函数将是私有函数。

在工作目录中创建一个名为`private`的子文件夹(_F:\\worksp\\matlab\\private_)。存储以下函数在文件`disc.m`中 -

```
function dis = disc(a,b,c) 

dis = sqrt(b^2 - 4*a*c);
end 
```

在工作目录中创建一个函数`quadratic3.m`(对应目录为：_F:\\worksp\\matlab_)，并在其中键入以下代码：

```
function [x1,x2] = quadratic3(a,b,c)

d = disc(a,b,c); 
x1 = (-b + d) / (2*a);
x2 = (-b - d) / (2*a);
end 
```

在Matlab命令行中，调用以上定义的函数 -

```
Trial>> quadratic3(2,4,-4)

ans =

    0.7321 
```

全局变量
----

全局变量可由多个函数共享。 为此，需要在所有函数中声明变量为全局变量。

如果要从基本工作区访问该变量，则在命令行中声明该变量。

全局声明必须在函数实际使用变量之前进行。 将大写字母用于全局变量的名称是一个很好的做法，以区别于其他变量。

**示例**

下面创建一个名为`average.m`的函数文件，并在其中键入以下代码 -

```
function avg = average(nums)
global TOTAL
avg = sum(nums)/TOTAL;
end 
```

创建脚本文件并在其中键入以下代码 -

```
global TOTAL;
TOTAL = 10;
n = [34, 45, 25, 45, 33, 19, 40, 34, 38, 42];
av = average(n) 
```

执行上面示例代码，得到以下结果 -

```
Trial>> global TOTAL;
TOTAL = 10;
n = [36, 45, 35, 45, 33, 29, 50, 41, 58, 47];
av = average(n)

av =

   41.9000 
```
