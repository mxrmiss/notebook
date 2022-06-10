---
title: HTML-day02
date: 2022/1/11
categories: HTML
tags: HTML
index_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_30.jpg
banner_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_30.jpg
---



# 声明：笔记摘自[菜鸟教程(https://www.runoob.com/)](https://www.runoob.com/)

# 1. [head 元素](https://www.runoob.com/html/html-head.html)

>\<head> 元素包含了所有的头部标签元素。在 \<head>元素中你可以插入脚本（scripts）, 样式文件（CSS），及各种meta信息。
>
>可以添加在头部区域的元素标签为: \<title>, \<style>, \<meta>, \<link>, \<script>, \<noscript> 和 \<base>。

| 标签                                                     | 描述                               |
| -------------------------------------------------------- | ---------------------------------- |
| [\<head>](https://www.runoob.com/tags/tag-head.html)     | 定义了文档的信息                   |
| [\<title>](https://www.runoob.com/tags/tag-title.html)   | 定义了文档的标题                   |
| [\<base>](https://www.runoob.com/tags/tag-base.html)     | 定义了页面链接标签的默认链接地址   |
| [\<link>](https://www.runoob.com/tags/tag-link.html)     | 定义了一个文档和外部资源之间的关系 |
| [\<meta>](https://www.runoob.com/tags/tag-meta.html)     | 定义了HTML文档中的元数据           |
| [\<script>](https://www.runoob.com/tags/tag-script.html) | 定义了客户端的脚本文件             |
| [\<style>](https://www.runoob.com/tags/tag-style.html)   | 定义了HTML文档的样式文件           |



# 2. [样式- CSS](https://www.runoob.com/html/html-css.html)

- CSS (Cascading Style Sheets) 用于渲染HTML元素标签的样式

## 2.1如何使用CSS

>CSS 是在 HTML 4 开始使用的,是为了更好的渲染HTML元素而引入的.
>
>CSS 可以通过以下方式添加到HTML中:
>
>- 内联样式- 在HTML元素中使用"style" **属性**
>- 内部样式表 -在HTML文档头部  \<head> 区域使用 \<style> **元素** 来包含CSS
>- 外部引用 - 使用外部 CSS **文件**
>
>最好的方式是通过外部引用CSS文件.

## 2.2[跳转至CSS教程](https://www.runoob.com/css/css-tutorial.html)



# 3. [图像](https://www.runoob.com/html/html-images.html)

- 在 HTML 中，图像由\<img> 标签定义。
- \<img> 是空标签，意思是说，它只包含属性，并且没有闭合标签。
- 要在页面上显示图像，你需要使用源属性（src）。src 指 "source"。源属性的值是图像的 URL 地址。

- 语法：

```
<img src="url" alt="some_text">
```

- 如果将图像标签置于两个段落之间，那么浏览器会首先显示第一个段落，然后显示图片，最后显示第二段。

## 3.1 Alt 属性

- alt 属性用来为图像定义一串预备的可替换的文本。替换文本属性的值是用户定义的。
- 在浏览器无法载入图像时，替换文本属性告诉读者失去的信息。此时，浏览器将显示这个替代性的文本而不是图像。

```
<img src="boat.gif" alt="Big Boat">
```

## 3.2 图像的高宽

- height（高度） 与 width（宽度）属性用于设置图像的高度与宽度。属性值默认单位为像素:

```
<img src="pulpit.jpg" alt="Pulpit rock" width="304" height="228">
```

- 如果图像指定了高度宽度，页面加载时就会保留指定的尺寸。如果没有指定图片的大小，加载页面时有可能会破坏HTML页面的整体布局。

## 3.3 HTML 图像标签

| 标签                                                 | 描述                       |
| :--------------------------------------------------- | :------------------------- |
| [\<img>](https://www.runoob.com/tags/tag-img.html)   | 定义图像                   |
| [\<map>](https://www.runoob.com/tags/tag-map.html)   | 定义图像地图               |
| [\<area>](https://www.runoob.com/tags/tag-area.html) | 定义图像地图中的可点击区域 |

## 3.4 [更多实例](https://www.runoob.com/html/html-images.html)

# 4. 表格

- 表格由 \<table> 标签来定义。每个表格均有若干行（由 \<tr> 标签定义），每行被分割为若干单元格（由 \<td> 标签定义）。字母 td 指表格数据（table data），即数据单元格的内容。数据单元格可以包含文本、图片、列表、段落、表单、水平线、表格等等。

```html
<table border="1">
    <tr>
        <td>row 1, cell 1</td>
        <td>row 1, cell 2</td>
    </tr>
    <tr>
        <td>row 2, cell 1</td>
        <td>row 2, cell 2</td>
    </tr>
</table>
```

![img](https://herozql.oss-cn-beijing.aliyuncs.com/main/4AEE0F4B-669C-4BBC-BEC4-6953E1B0E278.jpg)

## 4.1 表格表头

- 表格的表头使用 \<th> 标签进行定义。大多数浏览器会把表头显示为粗体居中的文本：

```html
<table border="1">
    <tr>
        <th>Header 1</th>
        <th>Header 2</th>
    </tr>
    <tr>
        <td>row 1, cell 1</td>
        <td>row 1, cell 2</td>
    </tr>
    <tr>
        <td>row 2, cell 1</td>
        <td>row 2, cell 2</td>
    </tr>
</table>
```

![img](https://herozql.oss-cn-beijing.aliyuncs.com/main/CB476DA7-7279-4892-A424-657772E385BA.jpg)

4.2 HTML 表格标签
---------

| 标签                                                         | 描述                 |
| ------------------------------------------------------------ | -------------------- |
| [\<table>](https://www.runoob.com/tags/tag-table.html)       | 定义表格             |
| [\<th>](https://www.runoob.com/tags/tag-th.html)             | 定义表格的表头       |
| [\<tr>](https://www.runoob.com/tags/tag-tr.html)             | 定义表格的行         |
| [\<td>](https://www.runoob.com/tags/tag-td.html)             | 定义表格单元         |
| [\<caption>](https://www.runoob.com/tags/tag-caption.html)   | 定义表格标题         |
| [\<colgroup>](https://www.runoob.com/tags/tag-colgroup.html) | 定义表格列的组       |
| [\<col>](https://www.runoob.com/tags/tag-col.html)           | 定义用于表格列的属性 |
| [\<thead>](https://www.runoob.com/tags/tag-thead.html)       | 定义表格的页眉       |
| [\<tbody>](https://www.runoob.com/tags/tag-tbody.html)       | 定义表格的主体       |
| [\<tfoot>](https://www.runoob.com/tags/tag-tfoot.html)       | 定义表格的页脚       |

## 4.3 [更多实例](https://www.runoob.com/html/html-tables.html)



# 5. 列表

- HTML 支持有序、无序和定义列表:

![image-20220111120841874](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220111120841874.png)

## 5.1 有序列表

- 有序列表是一列项目，列表项目使用数字进行标记。 
- 有序列表始于 \<ol> 标签。每个列表项始于 \<li> 标签。

```html
<ol>
<li>Coffee</li>
<li>Milk</li>
</ol>
```

![image-20220111121020740](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220111121020740.png)

## 5.2 无序列表

- 无序列表是一个项目的列表，此列项目使用粗体圆点（典型的小黑圆圈）进行标记。

- 无序列表使用 \<ul> 标签

```html
<ul>
<li>Coffee</li>
<li>Milk</li>
</ul>
```

![image-20220111121143746](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220111121143746.png)

## 5.3 自定义列表

- 自定义列表不仅仅是一列项目，而是项目及其注释的组合。
- 自定义列表以 \<dl> 标签开始。每个自定义列表项以 \<dt> 开始。每个自定义列表项的定义以 \<dd> 开始。

```html
<dl>
<dt>Coffee</dt>
<dd>- black hot drink</dd>
<dt>Milk</dt>
<dd>- white cold drink</dd>
</dl>
```

![image-20220111121319111](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220111121319111.png)

5.4 HTML 列表标签
---------

| 标签                                             | 描述                 |
| ------------------------------------------------ | -------------------- |
| [\<ol>](https://www.runoob.com/tags/tag-ol.html) | 定义有序列表         |
| [\<ul>](https://www.runoob.com/tags/tag-ul.html) | 定义无序列表         |
| [\<li>](https://www.runoob.com/tags/tag-li.html) | 定义列表项           |
| [\<dl>](https://www.runoob.com/tags/tag-dl.html) | 定义列表             |
| [\<dt>](https://www.runoob.com/tags/tag-dt.html) | 自定义列表项目       |
| [\<dd>](https://www.runoob.com/tags/tag-dd.html) | 定义自定列表项的描述 |

## 5.5 [更多实例](https://www.runoob.com/html/html-lists.html)

