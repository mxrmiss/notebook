到目前为止，我们已经看到所有的例子都在MATLAB以及它的GNU，或者称为_Octave_。 但是，为了求解基本代数方程，MATLAB和Octave都不同，所以这里将分别介绍MATLAB和Octave。

我们还将讨论代数表达式的分解和简化。

在MATLAB中求解基本代数方程
----------------

`solve`函数用于求解代数方程。 在其最简单的形式中，`solve`函数将引用中的方程式作为参数。

例如，在等式`x-5 = 0`中求解`x`，参考以下代码实现 -

```
solve('x-178=0') 
```

MATLAB将执行上述语句并返回以下结果 -

```
Trial>> solve('x-178=0')
ans =

178 
```

也可以这样调用`solve`函数 -

```
Trial>> solve('x-110 = 0')
ans =

110 
```

甚至可以不用包括方程的右侧部分 -

```
Trial>> solve('x-110')
ans =

110 
```

如果方程式涉及多个符号，则默认情况下，MATLAB假定正在求解`x`，但是，`solve`函数具有另一种形式 -

```
solve(equation, variable) 
```

其中，也可以涉及到变量。

例如，要求解`v - u - 3t^2 = 0`(这里为`t`的平方)，对于`v`，在这种情况下，应该书写为 -

```
solve('v-u-3*t^2=0', 'v') 
```

MATLAB执行上述语句将返回以下结果 -

```
ans =
 3*t^2 + u 
```

求解代数中的基本代数方程
------------

`roots`函数用于求解代数中的代数方程，可以重写上面的例子如下：

例如，要在等式`x-5 = 0`中求解`x`的值 -

```
roots([1, -5]) 
```

执行上面示例代码，得到以下结果 -

```
Trial>> roots([1, -5])

ans =

     5 
```

也可以这样调用`roots`函数 -

```
y = roots([1, -5]) 
```

执行上面示例代码，得到以下结果 -

```
Trial>> y = roots([1, -5])

y =

     5 
```

在MATLAB中求解二次方程
--------------

`solve`函数也可以用来求解高阶方程。通常用于求解二次方程。 该函数返回数组中方程的根。

以下示例求解二次方程`x^2 -7x +12 = 0`(注：`x^2`表示`x`的平方)。创建脚本文件并键入以下代码 -

```
eq = 'x^2 -7*x + 12 = 0';
s = solve(eq);
disp('The first root is: '), disp(s(1));
disp('The second root is: '), disp(s(2)); 
```

执行上面示例代码，得到以下结果 -

```
Trial>> eq = 'x^2 -7*x + 12 = 0';
s = solve(eq);
disp('The first root is: '), disp(s(1));
disp('The second root is: '), disp(s(2));

The first root is: 
3

The second root is: 
4 
```

在Octave中求解二次方程
--------------

以下示例解决Octave中的二次方程`x^2-7x +12 = 0`。创建脚本文件并键入以下代码 -

```
s = roots([1, -7, 12]);

disp('The first root is: '), disp(s(1));
disp('The second root is: '), disp(s(2)); 
```

执行上面示例代码，得到以下结果 -

```
Trial>> s = roots([1, -7, 12]);

disp('The first root is: '), disp(s(1));
disp('The second root is: '), disp(s(2));
The first root is: 
     4

The second root is: 
     3 
```

求解MATLAB中的高阶方程
--------------

`solve`函数也可以解决高阶方程。例如，下面演示求解`(x-3)^2(x-7)= 0`(注：`(x-3)^2`表示`(x-3)`的平方)的三次方程 -

MATLAB执行上述语句将返回以下结果 -

```
ans =
  3
  3
  7 
```

在较高阶方程的情况下，根很长，包含很多项。可以通过将这些根的数值转换为`double`来获得数值。 以下示例解决四阶方程`x^4 - 7x^3 + 3x^2 - 5x + 9 = 0`(注：`x^4`表示`x`的`4`次方)。

创建脚本文件并键入以下代码 -

```
eq = 'x^4 - 7*x^3 + 3*x^2 - 5*x + 9 = 0';
s = solve(eq);
disp('The first root is: '), disp(s(1));
disp('The second root is: '), disp(s(2));
disp('The third root is: '), disp(s(3));
disp('The fourth root is: '), disp(s(4));

disp('Numeric value of first root'), disp(double(s(1)));
disp('Numeric value of second root'), disp(double(s(2)));
disp('Numeric value of third root'), disp(double(s(3)));
disp('Numeric value of fourth root'), disp(double(s(4))); 
```

MATLAB执行上述语句将返回以下结果 -

```
The first root is: 
root(z^4 - 7*z^3 + 3*z^2 - 5*z + 9, z, 1)

The second root is: 
root(z^4 - 7*z^3 + 3*z^2 - 5*z + 9, z, 2)

The third root is: 
root(z^4 - 7*z^3 + 3*z^2 - 5*z + 9, z, 3)

The fourth root is: 
root(z^4 - 7*z^3 + 3*z^2 - 5*z + 9, z, 4)

Numeric value of first root
    1.0598

Numeric value of second root
    6.6304

Numeric value of third root
  -0.3451 - 1.0778i

Numeric value of fourth root
  -0.3451 + 1.0778i 
```

> 请注意，最后两个根是复数。

在Octave中求解高阶方程
--------------

以下示例示解四阶方程：`x^4 - 7x3 + 3x^2 - 5x + 9 = 0`。

创建脚本文件并键入以下代码 -

```
v = [1, -7,  3, -5, 9];

s = roots(v);

disp('Numeric value of first root'), disp(double(s(1)));
disp('Numeric value of second root'), disp(double(s(2)));
disp('Numeric value of third root'), disp(double(s(3)));
disp('Numeric value of fourth root'), disp(double(s(4))); 
```

MATLAB执行上述语句将返回以下结果 -

```
Trial>> v = [1, -7,  3, -5, 9];

s = roots(v);
% converting the roots to double type
disp('Numeric value of first root'), disp(double(s(1)));
disp('Numeric value of second root'), disp(double(s(2)));
disp('Numeric value of third root'), disp(double(s(3)));
disp('Numeric value of fourth root'), disp(double(s(4)));
Numeric value of first root
    6.6304

Numeric value of second root
    1.0598

Numeric value of third root
  -0.3451 + 1.0778i

Numeric value of fourth root
  -0.3451 - 1.0778i 
```

MATLAB中求解方程组
------------

`solve`函数也可用于生成包含多个变量的方程组的解。下面来看一个简单的例子来说明这一点。

下面来求解方程式 -

```
5x + 9y = 5

3x – 6y = 4

```

创建脚本文件并键入以下代码 -

```
s = solve('5*x + 9*y = 5','3*x - 6*y = 4');
x = s.x
y = s.y 
```

MATLAB执行上述语句将返回以下结果 -

```
x =

22/19


y =

-5/57 
```

同样，可以示解决更大的线性系统。 考虑以下一组方程式 -

```
x + 3y -2z = 5

3x + 5y + 6z = 7

2x + 4y + 3z = 8 
```

在Octave中求解方程组
-------------

还可以使用不同的方法来示解`n`未知数的`n`线性方程组。下面来看一个简单的例子来说明这一点。

假设要示解方程式 -

```
5x + 9y = 5

3x – 6y = 4

```

这种线性方程组可以写成单矩阵方程`Ax = b`，其中`A`是系数矩阵，`b`是包含线性方程右边的列向量，`x`是表示解的方法的列向量。如下图所示 -

创建脚本文件并键入以下代码 -

```
A = [5, 9; 3, -6];
b = [5;4];
A \ b 
```

执行上面示例代码，得到以下结果 -

```
ans =

   1.157895
  -0.087719 
```

同样，可以示解下面给出的较大的方程组 -

```
x + 3y -2z = 5

3x + 5y + 6z = 7

2x + 4y + 3z = 8

```

在MATLAB中扩展和集合方程
---------------

`expand` 和 `collect`函数分别扩展和集合方程。以下示例演示了这些概念 -

当使用许多符号功能时，应该声明变量为符号。

创建脚本文件并键入以下代码 -

```
syms x 
syms y 

expand((x-5)*(x+9))
expand((x+2)*(x-3)*(x-5)*(x+7))
expand(sin(2*x))
expand(cos(x+y))

collect(x^3 *(x-7))
collect(x^4*(x-3)*(x-5)) 
```

执行上面示例代码，得到以下结果 -

```
 ans =
 x^2 + 4*x - 45
 ans =
 x^4 + x^3 - 43*x^2 + 23*x + 210
 ans =
 2*cos(x)*sin(x)
 ans =
 cos(x)*cos(y) - sin(x)*sin(y)
 ans =
 x^4 - 7*x^3
 ans =
 x^6 - 8*x^5 + 15*x^4 
```

在Octave扩展和集合方程
--------------

需要有`symbolic`包，它提供了`expand`和`collect`函数来分别扩展和集合方程。 以下示例演示了这些概念 -

当使用许多符号功能时，应该声明变量是符号，但是`Octave`具有不同的方法来定义符号变量。注意使用的是`Sin`和`Cos`，它们是定义在`symbolic`包中的。

创建脚本文件并键入以下代码 -

```
% first of all load the package, make sure its installed.
pkg load symbolic

% make symbols module available
symbols

% define symbolic variables
x = sym ('x');
y = sym ('y');
z = sym ('z');

% expanding equations
expand((x-5)*(x+9))
expand((x+2)*(x-3)*(x-5)*(x+7))
expand(Sin(2*x))
expand(Cos(x+y))

% collecting equations
collect(x^3 *(x-7), z)
collect(x^4*(x-3)*(x-5), z) 
```

运行文件时，会显示以下结果 -

```
ans =

-45.0+x^2+(4.0)*x
ans =

210.0+x^4-(43.0)*x^2+x^3+(23.0)*x
ans =

sin((2.0)*x)
ans =

cos(y+x)
ans =

x^(3.0)*(-7.0+x)
ans =

(-3.0+x)*x^(4.0)*(-5.0+x) 
```

代数表达式的因式分解和简化
-------------

因子函数将表达式分解，简化函数简化表达式。 以下示例演示了这一概念 -

**示例**

创建脚本文件并键入以下代码 -

```
syms x
syms y
factor(x^3 - y^3)
f = factor(y^2*x^2,x)
simplify((x^4-16)/(x^2-4)) 
```

执行上面示例代码，得到以下结果 -

```
Trial>> factorization

ans =

[ x - y, x^2 + x*y + y^2]


f =

[ y^2, x, x]


ans =

x^2 + 4 
```

