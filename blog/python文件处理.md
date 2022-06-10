---
title: python文件处理
date: 2022/1/22
categories: python
tags: python
index_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_39.jpg
banner_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_39.jpg
---

# 1. 读与写

- 读文件：

```python
with open('data.txt', 'r') as f:
    data = f.read()
    print('context: {}'.format(data))
```

- 写文件：

```python
with open('data.txt', 'w') as f:
    data = 'some data to be written to the file'
    f.write(data)
```

| 读模式   | r    | 打开不存在的文件会报错、不能写；如不指定模式则默认是r        |
| -------- | ---- | ------------------------------------------------------------ |
| 写模式   | w    | 打开不存在的文件会,会新建一个文件；打开存在的文件会先清空后覆盖原有文件；不能读 |
| 追加模式 | a    | 打开不存在的文件会,会新建一个文件；不能读                    |

 

| 读写模式   | r+   | 能写，打开不存在的文件会报错                                 |
| ---------- | ---- | ------------------------------------------------------------ |
| 写读模式   | w+   | 能读，但是读不到内容，因为w先把文件内容清空了                |
| 追加读模式 | a+   | 能读，但读不到内容，因为文件指针默认在最后一行，可用seek移动文件指针位置 |

>在 Python 中使用文件的关键函数是 open() 函数。
>
>open() 函数有两个参数：文件名和模式。
>
>有四种打开文件的不同方法（模式）：
>
>- "r" - 读取 - 默认值。打开文件进行读取，如果文件不存在则报错。
>- "a" - 追加 - 打开供追加的文件，如果不存在则创建该文件。
>- "w" - 写入 - 打开文件进行写入，如果文件不存在则创建该文件。
>- "x" - 创建 - 创建指定的文件，如果文件存在则返回错误。
>
>此外，您可以指定文件是应该作为二进制还是文本模式进行处理。
>
>- "t" - 文本 - 默认值。文本模式。
>- "b" - 二进制 - 二进制模式（例如图像）。

# 2. 删除

## 2.1 删除文件

- 如需删除文件，必须导入 OS 模块，并运行其 os.remove() 函数：

```python
import os
os.remove("demofile.txt")
```

## 2.2 删除文件夹

- 使用 os.rmdir() 方法

```python
import os
os.rmdir("myfolder")
```

# 3. 创建

## 3.1 创建文件

- 使用open函数的 ‘x' 参数创建文件，如果文件存在则报错

## 3.2 创建文件夹

- 要创建单个目录，把目录路径作为参数传给 `os.mkdir()` :

```python
import os

os.mkdir('example_directory')
```

- 如果目录已存在会抛出异常： `FileExistsError`

```python
from os import mkdir

p = Path('example_directory')
try:
    p.mkdir()
except FileExistsError as e:
    print(e)
```

- 创建多个目录使用： `os.makedirs()`

# 4. 重命名

- Python包含用于重命名文件和目录的 `os.rename(src，dst)`：

```python
import os
os.rename('first.zip', 'first_01.zip')
```

# 5. 移动

- 要将文件或目录移动到其他位置，请使用 `shutil.move(src，dst)` 。

- `src` 是要移动的文件或目录，`dst` 是目标：

```python
import shutil
dst = shutil.move('dir_1/', 'backup/')
print(dst)  # 'backup'
```

- 和Linux中的mv命令相似，如果 `backup/` 存在，则 `shutil.move('dir_1/'，'backup/')` 将 `dir_1/` 移动到 `backup/` 。 如果 `backup/` 不存在，则 `dir_1/` 将重命名为 `backup` 。

# 6. 列出当前目录文件名

```python
import os

basepath = 'my_directory'
with os.scandir(basepath) as entries:   # 返回一个迭代器
    for entry in entries:
        if entry.is_file():
            print(entry.name)
            
# 或者
from pathlib import Path

basepath = Path('my_directory')     # 返回一个迭代器
for entry in basepath.iterdir():
    if entry.is_file():
        print(entry.name)
```

- 使用 `pathlib` 比起使用 `os` 中的函数更加有效。

# 7. 文件名模式匹配

>`startswith()` 和`endswith()`  字符串方法，匹配前缀和后缀
>
>`fnmatch.fnmatch()`
>
>`glob.glob()`
>
>`pathlib.Path.glob()`

## 7.1 `startswith()` 和`endswith()`  字符串方法

```python
import os

for f_name in os.listdir('some_directory'):
    if f_name.endswith('.txt'):
        print(f_name)
```

上述代码找到 `some_directory` 中的所有文件，遍历并使用 `.endswith()` 来打印所有扩展名为 `.txt` 的文件名。

## 7.2 `fnmatch.fnmatch()`

- 这是一个支持使用 `*` 和 `?` 等通配符的函数。例如，使用 `fnmatch` 查找目录中所有 `.txt` 文件，你可以这样做:

```python
import os
import fnmatch

for f_name in os.listdir('some_directory'):
    if fnmatch.fnmatch(f_name, '*.txt'):
        print(f_name)
```

- 或者：

```python
import os
import fnmatch

for f_name in os.listdir('some_directory'):
    if fnmatch.fnmatch(f_name, 'data_*_backup.txt'):
        print(f_name)
```

## 7.3 `glob.glob()`

- `glob.glob('*.py')` 搜索当前目录中具有 `.py` 扩展名的文件，并且将它们以**列表**的形式返回。

```python
import glob

print(glob.glob('*.py'))
```

- `glob` 也很容易在子目录中递归的搜索文件:

```python
import glob

for name in glob.iglob('**/*.py', recursive=True):
    print(name)
```

- 传递 `recursive=True` 作为 `.iglob()` 的参数使其搜索当前目录和子目录中的 `.py` 文件。`glob.glob()` 和 `glob.iglob()` 不同之处在于，`iglob()` 返回一个**迭代器**而不是一个列表。

# 8. 判断文件目录是否存在

## 8.1 os模块

判断文件是否存在

```python
os.path.isfile(path)
```

判断目录是否存在

```python
os.path.isdir(path)
```

判断路径是否存在

```python
# 使用 path 模块
os.path.exists(path)
```

## 8.2 pathlib 模块

```python
import pathlib

path = pathlib.Path(‘path/to/file’)
```

- 判断文件是否存在

```
path.is_file()
```

- 判断目录是否存在

```
path.is_dir()
```

- 判断路径是否存在

```
path.exists()
```

- 也可以采用

```python
from pathlib import Path

my_file = Path("/path//file")
if my_file.is_file():  # 指定的文件存在
if my_file.is_dir():   # 指定的目录存在
if my_file.exists():   # 指定的文件或目录存在
```

# 9. 遍历目录和处理文件

- 使用内置的Python函数 `os.walk()`

- `os.walk()` 在每个循环中返回三个值：

>1. 当前文件夹的名称
>2. 当前文件夹中子文件夹的列表
>3. 当前文件夹中文件的列表

- 默认是从上到下遍历目录:

```
Found directory: .
test1.txt
test2.txt
Found directory: ./folder_1
file1.py
file3.py
file2.py
Found directory: ./folder_2
file4.py
file5.py
file6.py
```

- 要以自下而上的方式遍历目录树，则将 `topdown=False` 关键字参数传递给 `os.walk()` ：

```python
for dirpath, dirnames, files in os.walk('.', topdown=False):
    print(f'Found directory: {dirpath}')
    for file_name in files:
        print(file_name)
```

- 传递 `topdown=False` 参数将使 `os.walk()` 首先打印出它在子目录中找到的文件:

```
Found directory: ./folder_1
file1.py
file3.py
file2.py
Found directory: ./folder_2
file4.py
file5.py
file6.py
Found directory: .
test1.txt
test2.txt
```



# 参考文献

- [参考链接1](https://www.w3school.com.cn/python/python_file_handling.asp)
- [参考链接2](https://blog.csdn.net/vip_lvkang/article/details/76906718)
- [参考链接3](https://zhuanlan.zhihu.com/p/56909212)
- [参考文献4](https://blog.csdn.net/qq_21240643/article/details/99412409)