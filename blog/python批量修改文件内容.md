---
title: python批量修改文件内容
date: 2022/1/25
categories: python
tags: [python, script]
top_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_42.jpg
cover: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_42.jpg
---

- 脚本如下：

```python
"""
这个脚本用于批量修改文件内容，比如批量修改文件中的链接，每次仅可以修改一处内容
"""
import os
import re
import sys

mark_url = input('请输入文件所在的文件夹地址:')
modify_it = input('请输入被修改的部分:')
want_it = input('请输入你想修改成的内容:')
par = re.compile(modify_it)
modify_list = []  # 可修改文件名的列表
file_list = os.listdir(mark_url)  # 待修改文件夹
print("修改前：\n" + str(file_list))  # 输出文件夹中包含的文件
current_path = os.getcwd()  # 得到进程当前工作目录
os.chdir(mark_url)  # 将当前工作目录修改为待修改文件夹的位置

for filename in file_list:
    with open(filename, 'r', encoding='utf-8') as f:
        content = f.read()
    with open(filename, 'w', encoding='utf-8') as f:
        # 查看文件是否相匹配
        if modify_it in content:
            modify_list.append(filename)
            # 替换文件内容
            file = par.sub(want_it, content)
            f.write(file)

if modify_list:
    print('可修改的文件名为：\n', modify_list)
    print('修改完毕！')
else:
    print('没有可以修改的文件！')

os.chdir(current_path)  # 改回程序运行前的工作目录
sys.stdin.flush()  # 刷新

```

