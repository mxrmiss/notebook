---
title: C-结构体
date: 2022/3/23
categories: C
tags: C
top_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_44.jpg
cover: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_44.jpg
---



## 1. 定义

### 1.1 结构体类型的定义

- 见书上197页

- 如下代码所示：

```c
struct student
{
    long int num;
    char name[20];
    char sex;
    int age;
};
```

- 关键字struct，结构体类型名字student。关键字和结构体类型名字组合成新的类型标识符，地位如同int，char等。

### 1.2 结构体类型变量的定义

- 定义结构体类型的同事是定义其变量，代码如下：

```c
struct staff
{
    char name[20];
    char department[20];
    int salary;
    int cost;
    int realsum;
};
struct staff worker1, worker2;
```

- 书上还介绍了两种方法，上面的最好，容易移植，并且清晰明了

## 2. 引用

- 见书上200页

- 取结构成员的地址时，“&” 要放在结构变量名前面而不是结构成员前面
- 相同结构体类型的结构体变量可以通过整体引用来赋值，如 `worker2 = worker1;`
- C语言不允许对结构体类型变量进行任何逻辑运算

## 3. 初始化

- 见书上203页

- 可以对成员逐个赋值，也可以整体赋值
- 整体赋值如：

>`struct staff worker1 = {"Wang_Wei", "see", 1200, 100, 1100};`

## 4. 嵌套结构

- 见书上204页

- 指向结构体自身的指针是允许的，也是链表、队列或复杂的数据结构所必须的

## 5. 结构体类型数组

- 见书上206页
- 每一个数组元素相当于同一个结构变量拷贝后填上不同的数据，也就是相同的框架，不同的数据
- 见书上207页代码，能很好地解释

## 6. 结构体类型指针

