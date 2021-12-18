# 博客园，CSDN等平台利用Github绑定域名

---

#### **网上有许多关于这方面的教程，但是大多数由于Github发生了更新导致教程出现了断层，并且在绑定域名后还会有其他问题发生，在这篇博文中，我将详细的演示如何将博客园，CSDN等博客平台利用GitHub绑定自己的域名**

---

- 首先我们需要在GitHub上面建立一个仓库，仓库的格式是 `XXX.github.io` , xxx部分为自己选择的名字

- 接着在仓库中建立两个文件，分别是CNAME 和 index.html

![1](https://mxrblog.cn/main/59f5640b3945dc0d4ea9c2b5a0dfb8b7.png)

- 在CNAME中填入自己的选择绑定的域名

![1.2](https://mxrblog.cn/main/5100502402bd2b3ddd8d1b5f6cd06919.png)

- 在index.html中填入以下内容, **注意`window.location.href="https://social.blog.csdn.net/"` 这一栏中的域名换成自己的博客网站给的域名**

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <style media="screen">
            * {
                margin: 0;
                padding: 0;
            }
        </style>
    </head>
    <body>
    </body>
    <script type="text/javascript">
        window.location.href="https://social.blog.csdn.net/";  //换成自己的博客网站给的域名
    </script>
</html>
```

- ping通自己的GitHub仓库后(如ping csdn.github.io)，复制给出的地址到剪切板中

![2](https://mxrblog.cn/main/67b2789f8d257531056e73024572a484.png)

- 对自己的域名进行解析，添加如下两条记录

1. CNAME记录，用来转发链接的

![3](https://mxrblog.cn/main/78912f28aa8db11a58826004ce5c7def.png)

2. A类型解析，用来开启Github 的 HTTPS（）后面会说到

![4](https://mxrblog.cn/main/dfc71d79bf3d57ddef8b6a52ca0bbe55.png)

- 打开GitHub仓库，在页面中选择setting选项![5](https://mxrblog.cn/main/b0b08af54217716d1b2343d1f6419700.png)

- 然后选择page选项

![6](https://mxrblog.cn/main/3caa05f3f1090625553f15958333ecf9.png)

- 再按下图选择

![7](https://mxrblog.cn/main/58a79afd28b72d08221bd1dbb873ab18.png)

- 点击保存

![8](https://mxrblog.cn/main/cff051ab412420ff1f6d6aa739ac87e2.png)

- 等待加载完成

![9](https://mxrblog.cn/main/bdf2811aaf967c3c4d2258dcfd80452b.png)

- **刷新界面**后点击 Enforces HTTPS 强制选择HTTPS，不选也行

![10](https://mxrblog.cn/main/eff76f1988a212a2ec816425b5690e51.png)

- 其实做到这一步已经可以利用我们的域名访问我们的博客了，但是我们访问博客的时候，浏览器会警告我们，我们的域名是不安全的链接，解决这个问题的方法是给我们的域名添加SSL证书，如果已经添加的证书的话就不会出现这种情况

![11](https://mxrblog.cn/main/8020667ca17e449b5b9e5fbb1626beeb.png)

- 本人是在阿里云注册的域名，每年附送20张免费的SSL证书，当然像腾讯云这些域名提供商月会有这项免费服务，申请完免费的证书后，我将接着演示如何访问自己绑定的域名时显示我们的域名是安全的

1. 申请一个免费的证书（其实就是在20张证书中分配给你一个证书）

![12](https://mxrblog.cn/main/4c7abc3d9f02a00fb1bbc46bed3111af.png)

2. 等待几秒刷新后会显示已经绑定成功

![13](https://mxrblog.cn/main/3c7175fa3cd1f0ff585e7a6d8d274256.png)

3. 再刷新刚才警告的页面就能直接进去了

![14](https://mxrblog.cn/main/b280c5a57dd82d7ccd602484f7377dc4.png)

- 另外，我还想吐槽一下gitee，虽然也可以绑定域名，但是要备案的域名才可以，可是穷，买不起服务器，使用GitHub虽然实现了，但是访问会慢点，强迫症码农心里暗暗不爽 :(
- 国外的Mediun，Blogger可以直接绑定自己的域名，使用自己的域名访问博客的时候，网址栏上显示的是自己的域名，但是像我们这种使用GitHub转发的方式，点击自己的域名后网址栏上会跳转到博客平台的URL，（强迫症码农心里又暗暗不爽）
- 不爽归不爽，还是国内香，支持国内的博客平台，（CSDN，博客园听了暗暗高兴）但不能骄傲，毕竟有不足，还得继续发展 