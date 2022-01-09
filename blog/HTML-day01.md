---
title: HTML-day01
date: 2022/1/8
categories: HTML
tags: HTML
top_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_28.jpg
cover: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_28.jpg
---

# 声明：笔记摘自[菜鸟教程(https://www.runoob.com/)](https://www.runoob.com/)

# 1. HTML简析

## 1.1 代码

- 代码实例：

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>菜鸟教程(runoob.com)</title>
</head>
<body>
 
<h1>我的第一个标题</h1>
 
<p>我的第一个段落。</p>
 
</body>
</html>
```

- 运行结果：

![image-20220108162357451](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220108162357451.png)

- 代码解析

![code analyze](https://herozql.oss-cn-beijing.aliyuncs.com/main/02A7DD95-22B4-4FB9-B994-DDB5393F7F03.jpg)

## 1.2 网页结构

![image-20220108162911719](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220108162911719.png)

# 2. 元素

- HTML 文档由 HTML 元素定义。

![image-20220108164253880](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220108164253880.png)

- **特性：**

>- HTML 元素以**开始标签**起始
>- HTML 元素以**结束标签**终止
>- **元素的内容**是开始标签与结束标签之间的内容
>- 某些 HTML 元素具有**空内容（empty content）**
>- 空元素**在开始标签中进行关闭**（以开始标签的结束而结束）
>- 大多数 HTML 元素可拥有**属性**

# 3. 属性

- 属性是 HTML 元素提供的附加信息。

## 3.1 **特性：**

>- HTML 元素可以设置**属性**
>- 属性可以在元素中添加**附加信息**
>- 属性一般描述于**开始标签**
>- 属性总是以名称/值对的形式出现，**比如：name="value"**。
>- 属性小写

- 案例：

```
<a href="http://www.runoob.com">这是一个链接</a>
// HTML 链接由 <a> 标签定义。链接的地址在 href 属性中指定：
```

## 3.2 引用属性值

属性值应该始终被包括在引号内。

双引号是最常用的，不过使用单引号也没有问题。

![Remark](https://herozql.oss-cn-beijing.aliyuncs.com/main/lamp.gif)**提示:** 在某些个别的情况下，比如属性值本身就含有双引号，那么您必须使用单引号，例如：name='John "ShotGun" Nelson'

## 3.3 属性参考手册

查看完整的HTML属性列表: [HTML 标签参考手册](https://www.runoob.com/tags/html-reference.html)。

下面列出了适用于大多数 HTML 元素的属性：

| 属性  | 描述                                                         |
| :---- | :----------------------------------------------------------- |
| class | 为html元素定义一个或多个类名（classname）(类名从样式文件引入) |
| id    | 定义元素的唯一id                                             |
| style | 规定元素的行内样式（inline style）                           |
| title | 描述了元素的额外信息 (作为工具条使用)                        |

更多标准属性说明： [HTML 标准属性参考手册](https://www.runoob.com/tags/ref-standardattributes.html).

# 4. 标题

- 通过 <h1> - <h6> 标签进行定义的。示例如下：

```html
<h1>这是一个标题。</h1>
<h2>这是一个标题。</h2>
<h3>这是一个标题。</h3>
```

- 浏览器会自动地在标题的前后添加空行。

![image-20220108170703797](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220108170703797.png)

# 5. 水平线

- \<hr> 标签在 HTML 页面中创建水平线，可用于分隔内容。

![image-20220108170634066](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220108170634066.png)

# 6. 注释

- 用法:

```html
<!-- 这是一个注释 -->
```

![image-20220108170908998](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220108170908998.png)

# 7. 段落

- 段落是通过 <p> 标签定义的。
- **注意：**浏览器会自动地在段落的前后添加空行。（</p> 是块级元素）

```
<p>这是一个段落 </p>
<p>这是另一个段落</p>
```

![image-20220108171742562](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220108171742562.png)

# 8. 空白行

- 使用 </br> 作为空白行，可以用来换行

![image-20220108172048644](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220108172048644.png)

# 9. 文本格式化

## 9.1 HTML 文本格式化标签

| 标签                                                      | 描述         |
| --------------------------------------------------------- | ------------ |
| [\<b>](https://www.runoob.com/tags/tag-b.html)            | 定义粗体文本 |
| [\<em>](https://www.runoob.com/tags/tag-em.html)          | 定义着重文字 |
| [\<i>](https://www.runoob.com/tags/tag-i.html)            | 定义斜体字   |
| [\<small>](https://www.runoob.com/tags/tag-small.html)    | 定义小号字   |
| [\<strong>](https://www.runoob.com/tags/tag-strong.html)  | 定义加重语气 |
| [\<sub>](https://www.runoob.com/tags/tag-sub.html)        | 定义下标字   |
| [\<sup>](https://www.runoob.com/html/m/tags/tag-sup.html) | 定义上标字   |
| [\<ins>](https://www.runoob.com/tags/tag-ins.html)        | 定义插入字   |
| [\<del>](https://www.runoob.com/tags/tag-del.html)        | 定义删除字   |

9.2 HTML "计算机输出" 标签
---------------

| 标签                                                 | 描述               |
| ---------------------------------------------------- | ------------------ |
| [\<code>](https://www.runoob.com/tags/tag-code.html) | 定义计算机代码     |
| [\<kbd>](https://www.runoob.com/tags/tag-kbd.html)   | 定义键盘码         |
| [\<samp>](https://www.runoob.com/tags/tag-samp.html) | 定义计算机代码样本 |
| [\<var>](https://www.runoob.com/tags/tag-var.html)   | 定义变量           |
| [\<pre>](https://www.runoob.com/tags/tag-pre.html)   | 定义预格式文本     |

9.3 HTML 引文, 引用, 及标签定义
------------------

| 标签                                                         | 描述               |
| ------------------------------------------------------------ | ------------------ |
| [\<abbr>](https://www.runoob.com/tags/tag-abbr.html)         | 定义缩写           |
| [\<address>](https://www.runoob.com/tags/tag-address.html)   | 定义地址           |
| [\<bdo>](https://www.runoob.com/tags/tag-bdo.html)           | 定义文字方向       |
| [\<blockquote>](https://www.runoob.com/tags/tag-blockquote.html) | 定义长的引用       |
| [\<q>](https://www.runoob.com/tags/tag-q.html)               | 定义短的引用语     |
| [\<cite>](https://www.runoob.com/tags/tag-cite.html)         | 定义引用、引证     |
| [\<dfn>](https://www.runoob.com/tags/tag-dfn.html)           | 定义一个定义项目。 |

# 10. 链接

- HTML 链接语法
- href 属性描述了链接的目标。

```html
<a href="url">链接文本</a>
```

## 10.1 target 属性

- 使用 target 属性，你可以定义被链接的文档在何处显示。

![image-20220108174411844](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220108174411844.png)

## 10.2 id 属性

- id 属性可用于创建一个 HTML 文档书签。

- **提示:** 书签不会以任何特殊方式显示，即在 HTML 页面中是不显示的，所以对于读者来说是**隐藏的**。

>在HTML文档中插入ID:
>
>\<a id="tips">有用的提示部分</a>
>
>在HTML文档中创建一个链接到"有用的提示部分(id="tips"）"：
>
>\<a href="#tips">访问有用的提示部分</a>
>
>或者，从另一个页面创建一个链接到"有用的提示部分(id="tips"）"：
>
>\<a href="https://www.runoob.com/html/html-links.html#tips">访问有用的提示部分</a>

![image-20220108181125100](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220108181125100.png)

## 10.3 注意

>请始终将正斜杠添加到子文件夹。假如这样书写链接：href="https://www.runoob.com/html"，就会向服务器产生两次 HTTP 请求。这是因为服务器会添加正斜杠到这个地址，然后创建一个新的请求，就像这样：href="https://www.runoob.com/html/"。