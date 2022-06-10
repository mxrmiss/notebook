---
title: python可迭代对象、迭代器和生成器
date: 2022/1/22
categories: python
tags: python
index_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_40.jpg
banner_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_40.jpg
---

# 1. 迭代器与生成器的区别

>迭代器与列表的区别在于，构建迭代器的时候，不像列表把所有元素一次性加载到内存，而是以一种延迟计算（lazy
>evaluation）方式返回元素，这正是它的优点。比如列表中含有一千万个整数，需要占超过100M的内存，而迭代器只需要
>几十个字节的空间。因为它并没有把所有元素装载到内存中，而是等到调用next()方法的时候才返回该元素（按需调用
>call by need 的方式，本质上 for 循环就是不断地调用迭代器的next()方法）。

## 1.1 案例：列表与迭代器

- 列表不论遍历多少次，表头位置始终是第一个元素
- 迭代器遍历结束后，不再指向原来的表头位置，而是为最后元素的下一个位置

```python
# 列表与迭代器的区别
a = [1, 3, 5, 7]
a_iter = iter(a)

print('第一次遍历\n列表：')
for i in a:
    print(i, ' ', end='')
print('\n迭代器：')
for i in a_iter:
    print(i, ' ', end='')

print('\n第二次遍历\n列表：')
for i in a:
    print(i, ' ', end='')
print('\n迭代器：')
for i in a_iter:
    print(i, ' ', end='')
```

- 运行结果：

```
第一次遍历
列表：
1  3  5  7  
迭代器：
1  3  5  7  
第二次遍历
列表：
1  3  5  7  
迭代器：

进程已结束,退出代码0
```

# 2. 生成器

- 生成器一定是迭代器（反之不成立），因此任何生成器也是一种懒加载的模式生成值。下面来用生成器来实现斐波那契数列的例子：

```python
def fib(max):
    prev, curr = 0, 1
    while max > 0:
        max -= 1
        yield curr
        prev, curr = curr, prev + curr

if __name__ == '__main__':
    fib = fib(6)
    # 调用next()的过程
    for n in fib:
        print(n)
    # raise StopIteration
    print(next(fib))
```

- 上面是生成器函数，再来看下生成器的表达式，生成器表达式是列表推导式的生成器版本，看起来像列表推导式，但是它返回的是一个生成器对象而不是列表对象。

```python
>>> x = (x*x for x in range(10))
>>> type(x)
<class 'generator'>
>>> y = [x*x for x in range(10)]
>>> type(y)
<class 'list'>
```

- 我们再看看一个例子：

```python
# 函数有了yield之后，函数名+（）就变成了生成器
# return在生成器中代表生成器的中止，直接报错
# next的作用是唤醒并继续执行
# send的作用是唤醒并继续执行，发送一个信息到生成器内部

import time

def func(n):
    for i in range(0, n):
        # yield相当于return，下一次循环从yield的下一行开始
        arg = yield i
        print('func', arg)

if __name__ == '__main__':
    f = func(6)
    while True:
        print('main-next:', next(f))
        print('main-send:', f.send(100))
        time.sleep(1)
```

- 运行结果为：

```python
main-next: 0
func 100
main-send: 1
func None
main-next: 2
func 100
main-send: 3
func None
main-next: 4
func 100
main-send: 5
func None
Traceback (most recent call last):
  File "demo.py", line 13, in <module>
    print('main-next:', next(f))
StopIteration
```

- `yield`就是`return`返回一个值，并且记住这个返回的位置，下次迭代就从这个位置后（下一行）开始。`next`方法和`send`方法都可以返回下一个元素，区别在于`send`可以传递参数给`yield`表达式，这时传递的参数会作为yield表达式的值，而yield的参数是返回给调用者的值。

# 参考链接

- [参考链接1](https://www.jianshu.com/p/1319018dde31)
- [参考链接2](https://zhuanlan.zhihu.com/p/341439647#:~:text=迭代器与列表的区别在于，构建迭代器的时候，不像列表把所有元素一次性加载到内存，而是以一种延迟计算（lazy,evaluation）方式返回元素，这正是它的优点。 比如列表中含有一千万个整数，需要占超过100M的内存，而迭代器只需要几十个字节的空间。)
- [参考链接3](https://www.cnblogs.com/wj-1314/p/8490822.html#:~:text=%E7%94%9F%E6%88%90%E5%99%A8%E6%98%AF%E4%B8%80%E4%B8%AA%E7%89%B9%E6%AE%8A%E7%9A%84%E7%A8%8B%E5%BA%8F%EF%BC%8C%E5%8F%AF%E4%BB%A5%E8%A2%AB%E7%94%A8%E4%BD%9C%E6%8E%A7%E5%88%B6%E5%BE%AA%E7%8E%AF%E7%9A%84%E8%BF%AD%E4%BB%A3%E8%A1%8C%E4%B8%BA%EF%BC%8Cpython%E4%B8%AD%E7%94%9F%E6%88%90%E5%99%A8%E6%98%AF%E8%BF%AD%E4%BB%A3%E5%99%A8%E7%9A%84%E4%B8%80%E7%A7%8D%EF%BC%8C%E4%BD%BF%E7%94%A8yield%E8%BF%94%E5%9B%9E%E5%80%BC%E5%87%BD%E6%95%B0%EF%BC%8C%E6%AF%8F%E6%AC%A1%E8%B0%83%E7%94%A8yield%E4%BC%9A%E6%9A%82%E5%81%9C%EF%BC%8C%E8%80%8C%E5%8F%AF%E4%BB%A5%E4%BD%BF%E7%94%A8next,%28%29%E5%87%BD%E6%95%B0%E5%92%8Csend%20%28%29%E5%87%BD%E6%95%B0%E6%81%A2%E5%A4%8D%E7%94%9F%E6%88%90%E5%99%A8%E3%80%82)
