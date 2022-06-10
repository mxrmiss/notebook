---
title: python批量修改文件名
date: 2022/1/23
categories: python
tags: [python, script]
index_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_41.jpg
banner_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_41.jpg
---

- 脚本如下：

```python
"""
1）这个脚本的用处是批量修改文件的名称，如果存在已经修改过的名字的文件(加过前缀)就会跳过
2）这个脚本中修改的是图片，如果需要修改其他文件，可以单独创建一个变量，将代码中的 ‘.jpg’更换为变量
3）脚本符合三种情况，不含后缀名，前缀名需要修改，后缀名需要修改
4）这个脚本中还存在一些问题，如果缺失了某个图片，使得图片中数字码不连贯，这个脚本不会自动排序，需要手动排序否则
  修改的名称会追加到所有文件的最后面，但可以重命名新的前缀，再改回来，这也是一种办法，针对这个问题以后会解决的
"""

import os
import sys


# 批量修改文件名
def renameall():
    file_name = input("输入你想修改的文件夹所在的地址：")
    prefix_name = input("输入文件前缀名：")
    modify_list = []  # 可修改文件名的列表
    last_list = []    # 经筛选后再次修改的列表
    num = 1  # 表示文件数量的变量
    cum = 0  # 表示被修改文件数量的变量
    file_List = os.listdir(file_name)   # 待修改文件夹
    print("修改前：\n" + str(file_List))  # 输出文件夹中包含的文件
    current_path = os.getcwd()  # 得到进程当前工作目录
    os.chdir(file_name)  # 将当前工作目录修改为待修改文件夹的位置

    # 将不含有前缀名的文件添加到列表中
    for filename in file_List:
        # 分离文件名和后缀名
        (name, suffix) = os.path.splitext(filename)
        # 若无后缀名添加后缀
        if '.' not in filename:
            modify_list.append(filename)
            # 如果修改后的文件名与列表中的不同，则立即修改，否则加入第二次修改队列中
            if (name + '.jpg') not in file_List:
                os.rename(filename, name + '.jpg')
            else:
                last_list.append(filename)
            cum += 1
        # 若文件前缀名一致,后缀名不匹配‘jpg’，更改后缀名
        elif (prefix_name in filename) and (suffix != '.jpg'):
            modify_list.append(filename)
            if (name + '.jpg') not in file_List:
                os.rename(filename, name + '.jpg')
            else:
                last_list.append(filename)
                cum += 1
        # 若文件的前缀名不一致，添加进总列表中
        elif prefix_name not in filename:
            modify_list.append(filename)
            last_list.append(filename)
            cum += 1
        num += 1
    num -= cum

    print('可以修改的文件名列表：\n{}'.format(modify_list))
    print("---------------------------------------------------")

    # 第二次修改队列
    if modify_list:
        for filename in last_list:
            os.rename(filename, prefix_name + str(num) + '.jpg')
            num += 1
        os.chdir(current_path)  # 改回程序运行前的工作目录
        sys.stdin.flush()  # 刷新
        print("修改后：" + str(os.listdir(file_name)))  # 输出修改后文件夹中包含的文件
    else:
        print("没有可重命名的文件")
        return None
    os.chdir(current_path)  # 改回程序运行前的工作目录
    sys.stdin.flush()  # 刷新


if __name__ == '__main__':
    renameall()
    # try:
    #     renameall()
    # except:
    #     print("出错了!!!!快用999小葵花牌感冒灵 :)")
```

