在MATLAB环境中，每个变量都是数组或矩阵。

可以以简单的方式分配变量。 例如，

```
x = 12 
```

MATLAB执行上述语句并返回以下结果 -

```
Trial>> x = 12       

x =

    12 
```

它创建一个名为`x`的`1×1`矩阵，并将值`3`存储在其元素中。再来看一个例子，如下，

```
x = sqrt(16) 
```

MATLAB执行上述语句并返回以下结果 -

```
Trial>> x = sqrt(16)     

x =

     4 
```

请注意 -

*   当变量输入到系统中，可以在接下来代码中引用。
*   变量在使用前必须有值。
*   当表达式返回未分配给任何变量的结果时，系统将其分配给名为`ans`的变量，稍后可以使用它。

例如，

```
sqrt(99) 
```

MATLAB执行上述语句并返回以下结果 -

```
Trial>> sqrt(99)

ans =

    9.9499 
```

可以使用这个`ans`变量 -

```
sqrt(99);
99.499/ans

```

MATLAB执行上述语句并返回以下结果 -

```
Trial>> sqrt(99);
99.499/ans

ans =

   10.0000 
```

下面我们再来看另一个例子 -

```
x = 7 * 8;
y = x * 7.89 
```

MATLAB执行上述语句并返回以下结果 -

```
Trial>> x = 7 * 8;
y = x * 7.89

y =

  441.8400 
```

多重分配赋值
------

可以在同一行上拥有多个赋值。 例如，

```
a = 2; b = 7; c = a * b 
```

MATLAB执行上述语句并返回以下结果 -

```
c = 14 
```

变量历史
----

`who`命令显示使用过的所有变量名。

```
Trial>> who

您的变量为:

ans  x    y 
```

`whos`命令更多地显示变量 -

*   当前在内存中的变量
*   每个变量的类型
*   每个变量的内存分配
*   是否是复合的变量？

执行结果如下 -

```
Trial>> whos 
  Name      Size            Bytes  Class     Attributes

  ans       1x1                 8  double              
  x         1x1                 8  double              
  y         1x1                 8  double 
```

清除命令从存储器中删除所有(或指定的)变量。

```
clear x     
clear 
```

长任务
---

长任务可以通过使用省略号(`...`)扩展到另一行。 例如，

```
initial_velocity = 0;
acceleration = 9.8;
time = 20;
final_velocity = initial_velocity ...
    + acceleration * time 
```

MATLAB执行上述语句并返回以下结果 -

```
Trial>> initial_velocity = 0;
acceleration = 9.8;
time = 20;
final_velocity = initial_velocity + acceleration * time

final_velocity =

   196 
```

格式命令
----

默认情况下，MATLAB显示四位小数位数。这称为：_短格式_。

但是，如果要更精确，则需要使用`format`命令。

`format long`命令显示十进制后的`16`位数字。

例如 -

```
Trial>> format long
x = 7 + 10/3 + 5 ^ 1.2

x =

  17.231981640639408 
```

另一个示例如下 -

```
Trial>> format short
x = 7 + 10/3 + 5 ^ 1.2

x =

   17.2320 
```

`format bank`命令将数字舍入到小数点后两位。例如，

```
format bank
daily_wage = 177.45;
weekly_wage = daily_wage * 6 
```

MATLAB执行上述语句并返回以下结果 -

```
Trial>> format bank
daily_wage = 177.45;
weekly_wage = daily_wage * 6

weekly_wage =

       1064.70 
```

MATLAB使用指数符号显示大数字。

`format short e`命令以指数形式显示四位小数加上指数。

例如，

```
format short e
4.678 * 4.9 
```

MATLAB执行上述语句并返回以下结果 -

```
Trial>> format short e
4.678 * 4.9

ans =

   2.2922e+01 
```

`format long e`命令允许以指数形式显示十六位小数加上指数。 例如，

```
format long e
x = pi 
```

MATLAB执行上述语句并返回以下结果 -

```
Trial>> format long e
x = pi

x =

     3.141592653589793e+00 
```

`format rat`命令给出计算结果最接近的合理表达式。 例如，

```
format rat
4.678 * 4.9 
```

MATLAB执行上述语句并返回以下结果 -

```
Trial>> format rat
4.678 * 4.9

ans =

    2063/90 
```

创建向量
----

向量是数字的一维数组。MATLAB允许创建两种类型的向量：

*   行向量
*   列向量

**行向量**是通过用方括号中的元素集合来创建的，使用空格或逗号分隔元素。

例如，

```
r = [7 8 9 10 11] 
```

MATLAB执行上述语句并返回以下结果 -

```
Trial>> r = [7 8 9 10 11]

r =

       7              8              9             10             11 
```

**另一个示例**

```
r = [7 8 9 10 11];
t = [2, 3, 4, 5, 6];
res = r + t 
```

MATLAB执行上述语句并返回以下结果 -

```
Trial>> r = [7 8 9 10 11];
t = [2, 3, 4, 5, 6];
res = r + t

res =

       9             11             13             15             17 
```

**列向量**通过用方括号中的元素集合来创建，使用分号(`;`)来分隔元素。

```
c = [7;  8;  9;  10; 11] 
```

MATLAB执行上述语句并返回以下结果 -

```
Trial>> c = [7;  8;  9;  10; 11]

c =

       7       
       8       
       9       
      10       
      11 
```

创建矩阵
----

矩阵是数字的二维数组。

在MATLAB中，通过将每行作为一系列空格或逗号分隔的元素输入矩阵，并以行号分隔一行。 例如，创建一个`3x3`的矩阵：

```
m = [1 2 3; 4 5 6; 7 8 9] 
```

MATLAB执行上述语句并返回以下结果 -

```
Trial>> m = [1 2 3; 4 5 6; 7 8 9]

m =

       1              2              3       
       4              5              6       
       7              8              9 
```
