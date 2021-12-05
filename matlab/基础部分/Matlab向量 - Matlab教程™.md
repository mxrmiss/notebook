向量是数字的一维数组。在MATLAB中，允许创建两种类型的向量 -

*   行向量
*   列向量

**行向量**

行向量是通过用方括号中的元素集合来创建的，使用空格或逗号分隔元素。

```
r = [1 18 19 21 41] 
```

MATLAB将执行上述语句并返回以下结果 -

```
Trial>> r = [1 18 19 21 41]

r =

     1    18    19    21    41 
```

**列向量**

列向量是通过用方括号中的元素集合来创建的，分号用于分隔元素。

```
c = [17;  28;  39;  60; 81] 
```

MATLAB将执行上述语句，返回以下结果 -

```
Trial>> c = [17;  28;  39;  60; 81]

c =

    17
    28
    39
    60
    81 
```

引用向量的元素
-------

可以通过多种方式来引用一个或多个向量的元素。向量`v`的第`i`个分量叫作`v(i)`。 例如 -

```
v = [ 1; 2; 3; 4; 5; 6];    
v(3) 
```

MATLAB执行上述语句，返回以下结果 -

```
Trial>> v = [ 1; 2; 3; 4; 5; 6];    % creating a column vector of 6 elements
v(3)

ans =

     3 
```

引用带冒号的向量(如`v(:)`)时，将列出向量的所有组件。

```
v = [ 1; 2; 3; 4; 5; 6];    
v(:) 
```

MATLAB执行上述语句，返回以下结果 -

```
Trial>> v = [ 1; 2; 3; 4; 5; 6];    % creating a column vector of 6 elements
v(:)

ans =

     1
     2
     3
     4
     5
     6 
```

MATLAB可从向量中选择一系列元素。

例如，创建一个`9`个元素的行向量`rv`，然后通过`rv(3：7)`引用`3`到`7`元素，然后引用来向一个新创建的`sub_rv`向量赋值。如下代码所示 -

```
rv = [1 2 3 4 5 6 7 8 9];
sub_rv = rv(3:7) 
```

MATLAB将执行上述语句并返回以下结果 -

```
Trial>> rv = [1 2 3 4 5 6 7 8 9];
sub_rv = rv(3:7)

sub_rv =

     3     4     5     6     7 
```

向量运算
----

在本节中，让我们讨论和学习以下向量的操作 -

*   [向量的加减](http://www.yiibai.com/matlab/matlab_vector_add_subtract.html "向量的加减")
*   [向量的标量乘法](http://www.yiibai.com/matlab/matlab_vector_scalar_multiplication.html "向量的标量乘法")
*   [转置向量](http://www.yiibai.com/matlab/matlab_vector_transpose.html "转置向量")
*   [附加向量](http://www.yiibai.com/matlab/matlab_vector_appending.html "附加向量")
*   [向量的幅值大小](http://www.yiibai.com/matlab/matlab_vector_magnitude.html "向量的幅值大小")
*   [向量点积](http://www.yiibai.com/matlab/matlab_vector_dot_product.html "向量点积")
*   [具有均匀间隔元素的向量](http://www.yiibai.com/matlab/matlab_vector_uniformly_spaced.html "具有均匀间隔元素的向量")

  