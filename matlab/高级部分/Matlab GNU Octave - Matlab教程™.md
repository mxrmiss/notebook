GNU Octave是像MATLAB这样的高级编程语言，它能与MATLAB兼容。它也是用于数值计算。

Octave具有以下MATLAB的常见功能 -

*   矩阵是基本数据类型
*   它内置了对复杂数字的支持
*   它具有内置的数学函数和库
*   它支持用户定义的函数

GNU Octave也是免费的可再分发软件。可以根据自由软件基金会发布的GNU通用公共许可证(GPL)的条款重新分发和/或修改它。

MATLAB vs Octave
----------------

大多数MATLAB程序都运行在Octave中运行，但是某些Octave程序可能无法在MATLAB中运行，因为Octave不支持MATLAB的某些语法。

例如，MATLAB仅支持单引号，但是Octave支持单引号和双引号来定义字符串。 如果正在寻找Octave教程，那么请从头开始学习本教程，因为在本教程中涵盖了MATLAB和Octave。

**兼容的示例**

本教程中涵盖的几乎所有示例都与MATLAB以及Octave兼容。在MATLAB和Octave中尝试以下示例，它产生相同的结果 -

此示例为函数 -

![](http://www.yiibai.com/uploads/images/201710/0810/843091041_71780.png)

创建一个3D曲面图。 创建脚本文件并键入以下代码 -

```
[x,y] = meshgrid(-2:.2:2);
g = x .* exp(-x.^2 - y.^2);
surf(x, y, g)
print -deps graph.eps 
```

MATLAB运行文件时，将显示以下三维图 -

![](https://mxrblog.cn/matlab/701091046_49837.png)

**不兼容的例子**

虽然MATLAB的所有核心功能都可以在Octave中使用，但是还有一些功能，例如微分和积分微积分，这两种语言都不完全匹配。本教程尝试给出两种类型的示例，它们的语法不同。

考虑以下示例，其中MATLAB和Octave使用不同的函数来获得曲线的面积：`f(x)= x^2 cos(x)`，对于`x`有`-4≤x≤9`。以下是MATLAB版本的代码 -

```
f = 'x^2*cos(x)';
ezplot(f, [-4,9])
a = int(f, -4, 9)
disp('Area: '), disp(double(a)); 
```

MATLAB运行文件时，将显示以下图形 -

![](https://mxrblog.cn/matlab/810091052_34992.png)

但是为了给Octave提供相同曲线的区域，但是需要使用`symbolic`包，如下所示：

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

