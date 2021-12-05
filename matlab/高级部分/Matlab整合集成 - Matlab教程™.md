整合(或也叫作集成)涉及两种本质上不同类型的问题。

*   第一种类型问题是给出了函数的导数，并且想要找到该函数。所以基本上扭转了差异化的过程。 这种反向过程被称为抗分化，或者找到原始函数，或者找到不确定的积分。
*   第二种类型问题是涉及相当多的非常小的数量，然后随着数量的大小接近于零，而术语的数量趋向于无穷大。这个过程导致了定积分的定义。

确定的积分用于查找区域，体积，重心，转动惯量，由力完成的工作以及许多其他应用。

使用MATLAB找到不确定的积分
----------------

根据定义，如果函数`f(x)`的导数是`f'(x)`，那么可以说`f'(x)`相对于`x`的不确定积分是`f(x)`。 例如，由于`x^2`的导数(相对于`x`)为`2x`，可以说`2x`的不确定积分是`x^2`。

在符号中 -

![](https://mxrblog.cn/matlab/649171019_87609.png)

因此可相当于 -

![](https://mxrblog.cn/matlab/800171019_83963.png)

不确定积分并不是唯一的，因为对于常数`c`的任何值，`x^2 + c`的导数也将是`2x`。

这用符号表示为 -

![](https://mxrblog.cn/matlab/303171020_47746.png)

其中，`c`被称为“任意常数”。

MATLAB提供了一个用于计算表达式积分的`int`命令。 为了得出一个函数的无限积分的表达式，它的写法为 -

```
int(f); 
```

例如，引用之前的例子 -

```
syms x 
int(2*x) 
```

MATLAB执行上述语句将返回以下结果 -

```
ans =
 x^2 
```

**示例1**

在这个例子中，有一些常用表达式的积分。 创建脚本文件并在其中键入以下代码 -

```
syms x n
int(sym(x^n))
f = 'sin(n*t)'
int(sym(f))
syms a t
int(a*cos(pi*t))
int(a^x) 
```

MATLAB执行上述语句将返回以下结果 -

```
ans =
 piecewise([n == -1, log(x)], [n ~= -1, x^(n + 1)/(n + 1)])
f =
sin(n*t)
ans =
 -cos(n*t)/n
 ans =
 (a*sin(pi*t))/pi
 ans =
 a^x/log(a) 
```

**示例2**  
创建脚本文件并在其中键入以下代码 -

```
syms x n
int(cos(x))
int(exp(x))
int(log(x))
int(x^-1)
int(x^5*cos(5*x))
pretty(int(x^5*cos(5*x)))
int(x^-5)
int(sec(x)^2)
pretty(int(1 - 10*x + 9 * x^2))
int((3 + 5*x -6*x^2 - 7*x^3)/2*x^2)
pretty(int((3 + 5*x -6*x^2 - 7*x^3)/2*x^2)) 
```

> 请注意，`pretty`函数返回表达式的更可读格式。

MATLAB执行上述语句将返回以下结果 -

```
ans =

sin(x)

ans =

exp(x)

ans =

x*(log(x) - 1)

ans =

log(x)

ans =

(24*cos(5*x))/3125 + (24*x*sin(5*x))/625 - (12*x^2*cos(5*x))/125 + (x^4*cos(5*x))/5 - (4*x^3*sin(5*x))/25 + (x^5*sin(5*x))/5

                                    2             4 
  24 cos(5 x)   24 x sin(5 x)   12 x  cos(5 x)   x  cos(5 x) 
  ----------- + ------------- - -------------- + ----------- - 
     3125            625             125              5 

        3             5 

    4 x  sin(5 x)   x  sin(5 x) 
     ------------- + ----------- 
          25              5

ans =

-1/(4*x^4)

ans =

tan(x)

        2 
  x (3 x  - 5 x + 1)

ans =

- (7*x^6)/12 - (3*x^5)/5 + (5*x^4)/8 + x^3/2

       6      5      4    3 
    7 x    3 x    5 x    x 
  - ---- - ---- + ---- + -- 
     12     5      8     2 
```

MATLAB执行上述语句将返回以下结果 -

使用MATLAB查找定积分
-------------

根据定义，定积分基本上是一个总和的极限。 我们使用定积分来查找曲线和`x`轴之间的面积以及两条曲线之间的面积。定量积分也可用于其他情况，其中所需数量可以表示为总和的极限。

通过传递要计算积分的极限，`int`函数可用于定积分。

参考公式 -

![](https://mxrblog.cn/matlab/799171027_46902.png)

它的写法是 -

```
int(x, a, b) 
```

例如，要计算的值是 -

![](https://mxrblog.cn/matlab/603171029_89898.jpg)

因此，可以书写为 -

```
int(x, 4, 9) 
```

MATLAB执行上述语句将返回以下结果 -

```
ans =
 65/2 
```

以下是以上示例的Octave写法 -

```
pkg load symbolic
symbols

x = sym("x");

f = x;

c = [1, 0];
integral = polyint(c);

a = polyval(integral, 9) - polyval(integral, 4);

display('Area: '), disp(double(a)); 
```

可以使用Octave提供的`quad()`函数编写另一个替代求解代码，如下所示：

```
pkg load symbolic
symbols

f = inline("x");
[a, ierror, nfneval] = quad(f, 4, 9);

display('Area: '), disp(double(a)); 
```

**示例1**

下面来计算`x`轴和曲线`y = x^3-2x + 5`和纵坐标`x = 1`和`x = 2`之间的面积。

所需面积由公式计算 -

![](https://mxrblog.cn/matlab/208171033_25003.png)

创建脚本文件并键入以下代码 -

```
f = x^3 - 2*x +5;
a = int(f, 1, 2)
display('Area: '), disp(double(a)); 
```

MATLAB执行上述语句将返回以下结果 -

```
a =
23/4
Area: 
    5.7500 
```

以下是上面示例的Octave写法 -

```
pkg load symbolic
symbols

x = sym("x");

f = x^3 - 2*x +5;

c = [1, 0, -2, 5];
integral = polyint(c);

a = polyval(integral, 2) - polyval(integral, 1);

display('Area: '), disp(double(a)); 
```

可以使用Octave提供的`quad()`函数给出一个替代求解代码，如下所示：

```
pkg load symbolic
symbols

x = sym("x");

f = inline("x^3 - 2*x +5");

[a, ierror, nfneval] = quad(f, 1, 2);
display('Area: '), disp(double(a)); 
```

MATLAB执行上述语句将返回以下结果 -

```
Area: 

 5.7500 
```

**示例2**

查找曲线下面积：`f(x)= x^2 cos(x)`，对于`-4≤x≤9`。

创建一个脚本文件并写下面的代码 -

```
f = x^2*cos(x);
ezplot(f, [-4,9])
a = int(f, -4, 9)
disp('Area: '), disp(double(a)); 
```

MATLAB执行上述语句将返回以下结果 -

![](https://mxrblog.cn/matlab/999171037_14579.png)

同时也会输出以下内容 -

```
a =

8*cos(4) + 18*cos(9) + 14*sin(4) + 79*sin(9)

Area: 
    0.3326 
```

以下是上面示例的Octave写法 -

```
pkg load symbolic
symbols

x = sym("x");

f = inline("x^2*cos(x)");

ezplot(f, [-4,9])
print -deps graph.eps

[a, ierror, nfneval] = quad(f, -4, 9);

display('Area: '), disp(double(a)); 
```

