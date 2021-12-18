---
title: Brainfuck
date: 2021-10-14
swiper: false # 将改文章放入轮播图中
swiperImg: '/medias/1.jpg' # 该文章在轮播图中的图片，可以是本地目录下图片也可以是http://xxx图片
img: 'http://cdn.mxrblog.cn/ct_5.jpg' # 该文章图片，可以是本地目录下图片也可以是http://xxx图片
categories: walkthrough
tags: [渗透测试, 靶机]
top: false
---



# brainfuck

- **这是hackthebox的一台难度为hard的机器**

###  信息收集

- nmap进行初步的端口和漏洞扫描

![image-20211010014845041](http://cdn.mxrblog.cn/image-20211010014845041.png)

- 发现443端口是开启的，输入ip地址但是无法正常访问网页

- 将ip与之前搜索到的域名填入`/etc/hosts`文件中

![image-20211011200123477](http://cdn.mxrblog.cn/image-20211011200123477.png)

- brainfuck.htb与www.brainfuck.htb结果是相同的，网站上说了有关于SMTP的事 **（伏笔）**，而sup3rs3crt.brainfuck.htb指向的是另一个网站

![image-20211011200454715](http://cdn.mxrblog.cn/image-20211011200454715.png)

![image-20211011200422596](http://cdn.mxrblog.cn/image-20211011200422596.png)

- 使用工具whatweb发现cms是wordpress
- 使用dirub没结果，太慢

![image-20211010124008980](http://cdn.mxrblog.cn/image-20211010124008980.png)

- 已经知道了网站的cms是WordPress，可以使用wpscan扫描网站的漏洞

- 首先使用`wpscan --url https://brainfuck.htb --disable-tls-checks -e p`查看该网站使用了什么插件

![image-20211011203357620](http://cdn.mxrblog.cn/image-20211011203357620.png)

![image-20211011203503105](http://cdn.mxrblog.cn/image-20211011203503105.png)

- 只有这一个插件，然后看看这个插件是否有什么漏洞

`wpscan --url https://brainfuck.htb --disable-tls-checks -e vp --api-token 64wEY6smCuQulgcBgg0cgstQ8urNAueZHIXAizHlNqM`

- disable-tls-checks    不使用tls检查，因为网站使用了https协议
- -e vp    进行插件的漏洞·扫描
- --api-token    令牌，注册wpscan后可以免费使用，只有使用它才能够查看到插件漏洞，注意首先需要建立一个`.wpscan`目录，在其中创建名为scan.yml的文件，文件中写入`api_token: xxxxxxxxxxxxxxxxxx`

- 共有6个**插件漏洞**

![image-20211011204423426](http://cdn.mxrblog.cn/image-20211011204423426.png)

![image-20211011204610352](http://cdn.mxrblog.cn/image-20211011204610352.png)

- 其他都无法使用最终就只有一个特权提升的漏洞可以使用

- 当然，如果使用wpscan发现插件的漏洞太多难以排查，可以使用searchsploit查看插件的漏洞

![image-20211011205238092](http://cdn.mxrblog.cn/image-20211011205238092.png)

- 点开漏洞页面给的链接，复制其中的HTML代码段

![image-20211011205454176](http://cdn.mxrblog.cn/image-20211011205454176.png)

- 更改代码段之中的一些内容，网址以及登录用户名
- 其中网站的用户名可以使用wpscan查看

![image-20211011205939655](http://cdn.mxrblog.cn/image-20211011205939655.png)

- 用户名有两个：admin和administrator，暂且用admin  **(其实administretor没用处，后面提到)**

![image-20211011205712400](http://cdn.mxrblog.cn/image-20211011205712400.png)

- 从浏览器中打开刚编辑的HTML文件，点击login，之后会显示一片空白，将原来的brainfuck.htb网站刷新，就会发现我们已经有了登录面板的权限

![image-20211011210611251](http://cdn.mxrblog.cn/image-20211011210611251.png)

![image-20211011211050936](http://cdn.mxrblog.cn/image-20211011211050936.png)

- 点击控制面板

![image-20211011213633557](http://cdn.mxrblog.cn/image-20211011213633557.png)

- 从设置中找到SMTP服务

![image-20211011214035584](http://cdn.mxrblog.cn/image-20211011214035584.png)

- 查看密码：kHGuERB29DNiNE
- 若密码不显示，可以尝试将type=“password”的参数password改为text

![image-20211011214354879](http://cdn.mxrblog.cn/image-20211011214354879.png)

- 下载邮件客户端软件evolution，并进行配置：

- 配置过程我不写忽略，里面的选项如端口等都选最常规的就行
- 进去后会发现收件箱里面有两封邮件，依次查看，得到账号密码

```
username: orestis
password: kIEnnfEKJ#9UmdO
```

- 由其中的描述“secret forum”可以知道是另一个域名为sup3rs3crt.brainfuck.htb的网站

![image-20211011215244548](http://cdn.mxrblog.cn/image-20211011215244548.png)

- 在网站进行登录，并查看里面的内容
- 可以发现讨论模块有三部分：key， ssh Access和development

![image-20211011215712120](http://cdn.mxrblog.cn/image-20211011215712120.png)

- 分别查看三部分的内容

![image-20211011215939189](http://cdn.mxrblog.cn/image-20211011215939189.png)

![image-20211011220102546](http://cdn.mxrblog.cn/image-20211011220102546.png)

![image-20211011220136045](http://cdn.mxrblog.cn/image-20211011220136045.png)

- 我们会发现key中的貌似是英文乱码，并且联系ssh access部分可以留意到用户orestis每段说完后都有一个签名：Orestis - Hacking for fun and profit  ，于是可以猜想这可能是将可能是将key部分的对话给加密了，我们不知道是什么加密方式，下面网站可以识别并进行解密：

```
https://www.boxentriq.com/code-breaking/cipher-identifier
```

![image-20211011221627906](http://cdn.mxrblog.cn/image-20211011221627906.png)

- 将加密过后的密文进行识别，发现是vigenere加密

![image-20211011221900328](http://cdn.mxrblog.cn/image-20211011221900328.png)

- 将签名原文当做key（解密需要一个key但是我们没有，将原文充当key）

```
同密文-同原文=key
同密文-key != 同原文    //很奇怪，但是我不知道原因
其他密文-key=原文
```

- 去掉原文和加密后的密文的空白及符号部分，进行解码，得到：

`BrainfuCkmybrainfuckmybrainfu`

- 可以知道key是fuckmybrain，使用可以对每段加密文进行解码，可以得到一个下载链接：

```
https://10.10.10.17/8ba5aa10e915218697d1c658cdee0bb8/orestis/id_rsa
```

![image-20211011222919169](http://cdn.mxrblog.cn/image-20211011222919169.png)

- 下载文件，发现是ssh密匙（通过我们之前查看的对话也能知道这个文件里面装的是ssh密匙）

![image-20211011223818793](http://cdn.mxrblog.cn/image-20211011223818793.png)

- 使用john将这个密匙解密，但是我们首先要将这个密匙转化为john能够识别的文件格式
- 使用ssh2john.python转化格式

![image-20211011224906545](http://cdn.mxrblog.cn/image-20211011224906545.png)

- 使用john进行解密，密码是：3poulakia!

![image-20211011224932772](http://cdn.mxrblog.cn/image-20211011224932772.png)

- ssh登录

![image-20211011225203633](http://cdn.mxrblog.cn/image-20211011225203633.png)

- ls得user.txt

![image-20211011225250695](http://cdn.mxrblog.cn/image-20211011225250695.png)

- 通过查看，在encrypt.sage文件中找到了有关于root.txt文件的线索
- 读取root.txt文件的内容，并对其进行16进制编码赋值给m，取随机数p，q，两数相乘赋值给n，在对两数操作赋值给e，通过pow()函数操作后赋值给c，将c字符串化后写入output.txt文件，p, q, e，写入debug.txt文件里

![image-20211011225356963](http://cdn.mxrblog.cn/image-20211011225356963.png)

- 网上有关于这个过程求解的代码，直接“拿来主义”就好了

```python
#!/usr/bin/env python3

import functools
import math

# -----[ The following functions were taken from James K Polk's reply  ]-----#
# -----[ to the following Stack Overflow query: https://bit.ly/2Pun615 ]-----#


def egcd(a, b):
    x0, x1, y0, y1 = 1, 0, 0, 1
    while b != 0:
        q, a, b = a // b, b, a % b
        x0, x1 = x1, x0 - q * x1
        y0, y1 = y1, y0 - q * y1
    return a, x0, y0


def inverse(a, n):
    d, a_inv, n_inv = egcd(a, n)
    if d != 1:
        raise ZeroDivisionError("{} is not coprime to {}".format(a, n))
    else:
        return a_inv % n


def lcm(*x):
    if not x or len(x) < 2:
        raise ValueError("at least two arguments must be supplied to lcm")
    lcm_of_2 = lambda x, y: (x * y) // math.gcd(x, y)
    return functools.reduce(lcm_of_2, x)


def carmichael_pp(p, e):
    phi = pow(p, e - 1) * (p - 1)
    if (p % 2 == 1) or (e >= 2):
        return phi
    else:
        return phi // 2


def carmichael_lambda(pp):
    return lcm(*[carmichael_pp(p, e) for p, e in pp])


# -----[ The following code takes the resultant text files from Orestis' ]-----#
# -----[ encrypt.sage code and uses the above functions to calculate the ]-----#
# -----[ original plaintext of /root/root.txt.                           ]-----#


with open("output.txt", "r") as f:
    y = int(f.read().strip().split()[2])

with open("debug.txt", "r") as f:
    lines = [int(line.strip()) for line in f.readlines()]

p, q, b = lines

c = p * q

# Now we have y, b, and c. We need to calculate x, which is the original root
# flag in integer format.
lam = carmichael_lambda([(p, 1), (q, 1)])
z = inverse(b, lam)
x = pow(y, z, c)

# Now that we have x, we need to transform it back into plaintext. The
# following line was provided by Erik Aronesty in the following Stack Overflow
# conversation: https://bit.ly/2PsZpGl

root_flag = x.to_bytes(((x.bit_length() + 7) // 8), "big").decode()

print(f"Root flag: {root_flag}")

```

- 得到root.txt

![image-20211011233319185](http://cdn.mxrblog.cn/image-20211011233319185.png)
