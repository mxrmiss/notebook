---
title: 博客搭建
date: 2021/12/12
tags: [blog, hexo]
categories: blog
index_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_1.jpg
banner_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_1.jpg
---



## 1. 准备工作

- 博客框架：Hexo
    - 博客主题：Fluid, Ayer, butterfly
- 主机：阿里云轻量级应用服务器
    - 配置：2核2G 60GB
- 图床：阿里云OSS
    - 软件：oss-browser(阿里云OSS适配)
- 写作工具：Typora
    - 图床软件：PicGo
- 博客备份：Github，Gitee，Onedrive



### 1.1 博客框架选择

- 我采用的是[Hexo]([Hexo](https://hexo.io/zh-cn/index.html))作为我的博客框架，理由是：响应快速、内存占用小、文章格式统一（都使用markdown格式）、迁移能力强

![image-20211210001116683](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211210001116683.png)

- [文档 | Hexo](https://hexo.io/zh-cn/docs/)

![image-20211210003013846](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211210003013846.png)



### 1.2 博客主题选择

#### 1.2.1 [butterfly](https://github.com/jerryc127/hexo-theme-butterfly)

- butterfly是一款功能齐全的hexo主题，界面优美，更新频率比其他主题更多，并且对手机端浏览较为友好
- 缺点是在服务器上生成页面速度相对于其他主题很慢，并且在web上打开页面也比其他主题稍慢，但是可以接受

![image-20211212004527099](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211212004527099.png)



![image-20211212004708018](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211212004708018.png)





#### 1.2.2 其他主题介绍

#####  [Fluid](https://github.com/fluid-dev/hexo-theme-fluid)

- [Fluid](https://github.com/fluid-dev/hexo-theme-fluid)是一款响应速度很快的主题，界面简洁美观，支持文章自动摘要（这部分做的很不错），博客应有的基础功能都具备了

![image-20211210001443531](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211210001443531.png)



![image-20211210002224502](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211210002224502.png)



- [Fluid](https://github.com/fluid-dev/hexo-theme-fluid) 具有很全面的使用文档，就像是一个配置了说明书的产品，可以很轻松的使用这个主题
- [配置指南 | Hexo Fluid (fluid-dev.com)](https://hexo.fluid-dev.com/docs/guide/#关于指南)

![image-20211210003126170](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211210003126170.png)



#####   [Ayer](https://github.com/shen-yu/hexo-theme-ayer)

- [Ayer](https://github.com/shen-yu/hexo-theme-ayer) 是一款简洁的主题，响应速度快，但不如Fluid，选择该主题的原因是：界面优雅，菜单栏设计在侧面，主题配置十分简单，很快就能配置好文件，博客所需的基本都具备

![image-20211210003534928](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211210003534928.png)



![image-20211210004003823](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211210004003823.png)



- Ayer相对于某些主题而言，对于国内用户提供了Gitee 的镜像下载
- [hexo-theme-ayer: 一个安静且优雅的Hexo主题-Ayer (gitee.com)](https://gitee.com/shen-yu/hexo-theme-ayer)

![image-20211210004232211](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211210004232211.png)



#####  主题对比

- 两个主题都有共同的优点，那就是响应速度快，简洁明了，配置简单
- Fluid缺点：使用移动端进行网页浏览时，菜单栏点击展开后会占据半个屏幕，让人很不适应；没有Gitee的镜像，对于大陆内地的用户不是很友好
- Ayer缺点：主题有一段时间没有更新了，对于文章的摘要过于简单，只有自动摘要只能截取标题

#####  [Bamboo](https://github.com/yuang01/hexo-theme-bamboo)

- 如果对上面我使用的主题不太满意，觉得版式太过简单或者可定制程度低，你可以使用主题 [bamboo](https://github.com/yuang01/hexo-theme-bamboo)
-  [yuang01/hexo-theme-bamboo: Hexo博客主题，功能强大，简洁 (github.com)](https://github.com/yuang01/hexo-theme-bamboo)

![image-20211210005805743](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211210005805743.png)



- 优点：美观，速度快，具有很多可以使用的功能，可定制程度较高，具有Gitee镜像，最近一直在更新（现在是2021-12-10）

[hexo-theme-bamboo: hexo博客主题--bamboo https://yuang01.gitee.io](https://gitee.com/yuang01/hexo-theme-bamboo)

![image-20211210010233209](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211210010233209.png)

- 缺点：与其他hexo主题相比十分臃肿，尽管作者对主题的配置有了很详细的说明和解释，但是复杂的主题配置文件需要花费很长时间去定制，并且由于主题迭代速度快，导致了主题配置文件需要根据不同版本进行细微的调整，另外，使用该主题文件上传hexo博客文件夹到GitHub时，仓库可能会提示代码危险，可能是主题的作者代码不规范，毕竟这只是一个人维护的主题
- 建议：这个主题有他自己的优点，主要取决于使用者是否喜欢花里胡哨的界面，因此我不推荐使用该主题，毕竟没必要将大量的时间浪费到博客主题的配置上，除非你想向你的好友炫耀你的主题



### 1.3 服务器

**这里不给相关链接，因为不想被理解是打广告的，服务器可以买，也可以不买，不买的话使用 Github page 或者Gitee page 都可搭建自己的博客，这里不再多述**

- 我使用的是阿里云的轻量级应用服务器，打折后3年内99元每年，2核 2G内存使用起来是绰绰有余的，无论是使用hexo还是wordpress

![image-20211210011640954](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211210011640954.png)



- 轻量级应用服务器的特点是可以很便捷的搭建应用，就像我可以很方便的在上面搭建主题，因为阿里云提供了相关软件的安装

![image-20211210012248797](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211210012248797.png)



## 2. 开始搭建

### 2.1 镜像选择

- 在轻量级应用服务器上安装 Node.js ，最好一开始购买机器初始化时就选择这个镜像，否则重置系统后再次安装，可能会无法通过阿里云网页服务器控制平台查看到内存的使用情况，按照阿里云的文档重新下载内存监视软件可能会不成功

![image-20211210012946246](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211210012946246.png)



### 2.2 安装Hexo

- 连接服务器，安装Hexo
- 所有必备的应用程序安装完成后，即可使用 npm 安装 Hexo。

```bash
npm install -g hexo-cli
```

- 安装以后，可以使用以下两种方式执行 Hexo：
    1. `npx hexo <command>`
    2. 将 Hexo 所在的目录下的 `node_modules` 添加到环境变量之中即可直接使用 `hexo <command>`：

```bash
echo 'PATH="$PATH:./node_modules/.bin"' >> ~/.profile
```

### 2.3 创建文件夹

#### 2.3.1 建立仓库

- 安装完 Hexo 后，我们需要新建一个文件夹来容纳我们的博客所需的一切文件，在此之前，我们需要先建立一个GitHub或Gitee仓库，并且将其克隆到我们的服务器上
- 克隆仓库到服务器上的目的是因为可以很方便的将我们的hexo整体给搬到仓库中，可以更好的备份以及以后的迁移，并且当我们可以将包含Hexo的仓库克隆到我们的计算机上，然后编辑里面的文件，结束后我们于服务器上同步内容

- 如我的仓库：

![image-20211210014541894-16390719449702](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211210014541894-16390719449702.png)

#### 2.3.2 创建文件夹

- 切换到已克隆的仓库内，使用如下命令创建文件夹：

```bash
hexo init <folder>  //folder为你所创建文件夹的名称，是博客的根文件夹，许多操作都是在这个文件夹中进行操作的
```

- 创建文件夹的过程实际上是从GitHub上面克隆仓库并且重新命名的过程，所以加载速度会比较慢，网上有其他方法，通过切换源的方法来加速文件夹创建过程，这里不再多述
- 文件夹建立后，在文件夹内生成了许多文件，在这里我将介绍几个比较重要的
    - _config.yml	是整个Hexo框架的配置文件
    - config.xxx.yml	xxx代表了hexo主题的名称，这是主题的配置文件，在主题内部也有一个名为 _config.yml 的文件，他的功能和 _config.xxx.yml 文件一样，但是位于hexo根目录下的 _config.xxx.yml 文件 具有更高的优先级，当加载主题时会优先调用 _config.xxx.yml 文件
    - source	这个文件夹中包含了文章，分类，标签的页面和文件夹，当然最开始时这个文件夹中几乎是空的，在后来会慢慢变充实
    - themes    主题文件夹，里面存放了你下载的主题
    - public      这是生成的网页文件， nginx 指向的就是这个文件

###  2.4 主题(butterfly)

#### 2.4.1 安装主题

- 安装主题有两种方式
    - `npm i hexo-theme-butterfly` 
        - 此方法只支持Hexo在5.0.0版本以上 **通过 npm 安装并不会在 themes 里生成主题文件夹，而是在 node_modules 里生成** 修改hexo根目录下(*hexo init 命令所创建的文件夹下*)的站点配置文件`_config.yml`
    - git 克隆主题到themes文件夹中，推荐使用这个方法，容易管理
- 下图是我的主题文件夹内容：

![image-20211210021620985](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211210021620985.png)

- 为了集中管理主题，可以将主题的配置文件复制改名到博客的根目录下，改名为 _config.xxx.yml (xxx是主题名字)，这样只需要配置博客根目录下的配置文件，而不用配置主题目录下的文件

#### 2.4.2 主题配置

#### 2.5 安装插件

- 如果你没有pug 以及stylus 的渲染器，请下载安装：

```
npm install hexo-renderer-pug hexo-renderer-stylus --save
```

- 字数统计

```
npm install hexo-wordcount --save
```

- RSS订阅

```
npm install hexo-generator-feed --save
```

- 全局搜索

```
npm install hexo-generator-search --save
```

> 在站点配置文件 _config.yml 当中写入如下配置，即可为全站的文档生成索引文件
>
> ```css
> search:
> path: search.xml
> field: post  //将post改为all即为搜索全部页面
> ```

- 为网站使用到的所有外链添加rel="noopener external nofollow noreferrer", 可以有效地加强网站SEO和防止权重流失

```
npm i hexo-filter-nofollow --save
```

- 安装seo优化插件

```bash
// sitemap.xml适合提交给谷歌搜素引擎
npm install hexo-generator-sitemap --save 
// aidusitemap.xml适合提交百度搜索引擎
npm install hexo-generator-baidu-sitemap --save 
```

- 中文链接转拼音

```
npm i hexo-permalink-pinyin --save
```

##### 文章设置

| 写法                    | 解释                                                         |
| ----------------------- | ------------------------------------------------------------ |
| title                   | 【必需】文章标题                                             |
| date                    | 【必需】文章创建日期                                         |
| updated                 | 【可选】文章更新日期                                         |
| tags                    | 【可选】文章标籤                                             |
| categories              | 【可选】文章分类                                             |
| keywords                | 【可选】文章关键字                                           |
| description             | 【可选】文章描述                                             |
| top\_img                | 【可选】文章顶部图片                                         |
| banner_img                   | 【可选】文章缩略图(如果没有设置top\_img,文章页顶部将显示缩略图，可设为false/图片地址/留空) |
| comments                | 【可选】显示文章评论模块(默认 true)                          |
| toc                     | 【可选】显示文章TOC(默认为设置中toc的enable配置)             |
| toc\_number             | 【可选】显示toc\_number(默认为设置中toc的number配置)         |
| copyright               | 【可选】显示文章版权模块(默认为设置中post\_copyright的enable配置) |
| copyright\_author       | 【可选】文章版权模块的`文章作者`                             |
| copyright\_author\_href | 【可选】文章版权模块的`文章作者`链接                         |
| copyright\_url          | 【可选】文章版权模块的`文章连结`链接                         |
| copyright\_info         | 【可选】文章版权模块的`版权声明`文字                         |
| mathjax                 | 【可选】显示mathjax(当设置mathjax的per\_page: false时，才需要配置，默认 false) |
| katex                   | 【可选】显示katex(当设置katex的per\_page: false时，才需要配置，默认 false) |
| aplayer                 | 【可选】在需要的页面加载aplayer的js和css,请参考文章下面的`音乐` 配置 |
| highlight\_shrink       | 【可选】配置代码框是否展开(true/false)(默认为设置中highlight\_shrink的配置) |
| aside                   | 【可选】显示侧边栏 (默认 true)                               |

- 常用文章设置

```
---
title:
date:
tags:
categories:
index_img:
banner_img:
---
```

- 分类和标签

只有文章支持分类和标签，您可以在 Front-matter 中设置。在其他系统中，分类和标签听起来很接近，但是在 Hexo 中两者有着明显的差别：分类具有顺序性和层次性，也就是说 `Foo, Bar` 不等于 `Bar, Foo`；而标签没有顺序和层次。

```
categories:
- Diary
tags:
- PS3
- Games
```

> 分类方法的分歧
>
> 如果您有过使用 WordPress 的经验，就很容易误解 Hexo 的分类方式。WordPress 支持对一篇文章设置多个分类，而且这些分类可以是同级的，也可以是父子分类。但是 Hexo 不支持指定多个同级分类。下面的指定方法：
>
> ```
> categories:
>   - Diary
>   - Life
> ```
>
> 会使分类`Life`成为`Diary`的子分类，而不是并列分类。因此，有必要为您的文章选择尽可能准确的分类。
>
> 如果你需要为文章添加多个分类，可以尝试以下 list 中的方法。
>
> ```
> categories:
> - [Diary, PlayStation]
> - [Diary, Games]
> - [Life]
> ```
>
> 此时这篇文章同时包括三个分类： `PlayStation` 和 `Games` 分别都是父分类 `Diary` 的子分类，同时 `Life` 是一个没有子分类的分类。

### 2.6 配置 _config.yml

- 接下来配置Hexo的配置文件 _config.yml ，这个文件在博客文件夹的根目录下
- 修改语言为 zh_CN，修改主题名，注意，主题名一定要与themes文件夹中的主题名成一样，否则之后的操作会报错

![image-20211210022926430](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211210022926430.png)

![image-20211210023002784](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211210023002784.png)

- 其他的可以按照个人喜好修改，如网站的标题，副标题等
- 这里不教学Hexo+github page的使用，有兴趣的可以在网上搜索



### 2.7 安装 Nginx

1. Nginx安装

```
yum install -y nginx
```

2. 配置Nginx

```
nginx -t
```

3. 使用vim打开nginx.conf文件

- 注意：有时该配置文件会不同于下面显示的，请参考网上其他教程从官网下载

```
vim /etc/nginx/nginx.conf
```

- 修改用户：

![image-20211210114418269](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211210114418269.png)

- 修改 http 部分（默认页面加载部分）

![屏幕截图 2021-12-10 114501](https://herozql.oss-cn-beijing.aliyuncs.com/main/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202021-12-10%20114501.jpg)

- 修改 https 部分 （需要先下载SSL证书）

![屏幕截图 2021-12-10 115453](https://herozql.oss-cn-beijing.aliyuncs.com/main/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202021-12-10%20115453.jpg)

- 自动跳转至https

>在网上直接搜索域名是不会使用 https 的，除非你的浏览器强制使用 https 的方式浏览网页，否则直接输入域名会使用 http 的方式访问网站，并且显示不安全的字样，可以配置 nginx配置文件来自动跳转到https链接
>
>- 在http项中添加 `rewrite ^(.*)$ https://$host$1 permanent;`

![image-20220115094852682](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20220115094852682.png)

- 以下是我的nginx配置文件

```nginx
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user root;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;

    server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  oldstory.cn;
	rewrite ^(.*)$ https://$host$1 permanent;
        root         xxxxxxx;  //填你的网站页面文件目录

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }

# Settings for a TLS enabled server.

    server {
        listen       443 ssl http2 default_server;
        listen       [::]:443 ssl http2 default_server;
        server_name  oldstory.cn;
        root         xxxxxxxxxx;   //填你的网站页面文件目录

        ssl_certificate "xxxxxxxxxx";   //ssl公钥地址
        ssl_certificate_key "xxxxxxxxxxx";  //s
        ssl_session_cache shared:SSL:1m;
        ssl_session_timeout  10m;
        ssl_ciphers PROFILE=SYSTEM;
        ssl_prefer_server_ciphers on;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }

}
```

- 启动Nginx

```
systemctl start nginx.service
```

- 重启Nginx

```
systemctl restart nginx.service
```

- 注意，有时候加载网页发现出现nginx 404 not found 是因为：
    - 更改了nginx配置文件内容但是没有刷新，需要重启nginx
    - nginx配置文件使用了错误的网页页面文件夹地址，在hexo中，地址为博客根目录下的 public 文件夹位置
    - 给博客文件夹赋权 `chmod 777 folder`

### 2.8 生成网页

- 在博客的根目录下执行命令

```
hexo clean	//该命令执行后清除public文件夹

hexo g -d  //该命令首先生成静态文件，然后立即部署
```

- 如果想先预览一下网页，可以使用 `hexo s` 命令，默认访问端口是4000，这时候我们需要开启服务器的4000端口否则无法访问

- 打开阿里云轻量级应用服务器，配置防火墙用来开放端口

![image-20211210122039919](https://herozql.oss-cn-beijing.aliyuncs.com/main/image-20211210122039919.png)

- 注意：无论是使用 `hexo g -d` 还是 `hexo s` 首先都最好先执行`hexo clean`



## 3. 其他配置要求

### 3.1 关于git

- 在某些服务器中预先不会有git命令，或者重装系统后需要重新安装git命令

#### 3.1.1Git安装

- 我使用的云主机是centos8系统的，之前也用过ubuntu18系统，但是感觉效果不怎的，可能试的次数比较少吧
- 先检查是否有git

```
git --version
```

- 没有则安装，旧的卸载重装

```
＃卸载旧的git
yum remove git
# 安装依赖库
yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel
# 将git下载安装到/usr/local/src目录
cd /usr/local/src
wget http://ftp.ntu.edu.tw/software/scm/git/git-2.26.0.tar.gz
tar -zvxf git-2.26.0.tar.gz
# 进入目录
cd git-2.26.0
# 编译执行
make prefix=/usr/local/git all
# 安装 git 到 /usr/local/git 目录下
make prefix=/usr/local/git install

```

#### 3.1.2配置Git环境变量

- 打开环境变量配置文件进行配置

```
vim /etc/profile

#添加以下内容
PATH=$PATH:/usr/local/git/bin   # git 的目录
export PATH

# 使配置的环境变量生效
source /etc/profile
```

- 验证git安装成功

```
git --version
```

#### 3.1.3云主机与GitHub交换SSH密匙

- Git初始化

```
# 设置github昵称
git config --global user.name 'mxrmiss'
# 设置github邮箱
git config --global user.email 'heroli520@outlook.com'
```

- git status 无法显示中文

```
git config --global core.quotepath false
```

- 创建ssh密匙

```
ssh-keygen -t rsa -C "heroli520@outlook.com"
```

- 输出 id_rsa.pub 内容并复制到GitHub中的setting页面上

```
cat id_rsa.pub
```

![syONLT.jpg](https://herozql.oss-cn-beijing.aliyuncs.com/main/syONLT.jpg)

- ssh -T [git@github.com](mailto:git@github.com) 输入此命令，显示successful表示成功

#### 3.1.4博客与github page挂钩

1. 在github上新建一个仓库，仓库名为自己的 github名.github.io

2. 在自己的博客目录下的站点配置文件_config.yml中进行配置，在文件的末尾找到并进行修改：

   ```
   deploy:
   	type: git
   	repo: //填仓库地址
   	branch： master  //填写上传页面的分支
   ```

3. 安装git部署插件

   ```
   npm install hexo-deployer-git --save
   ```



### 3.2 关于node.js

- 对于某些非轻量级应用服务器需要对node.js进行更详细的设置

1. node.js下载

```
cd /opt
wget https://npm.taobao.org/mirrors/node/latest-v15.x/node-v15.0.1-linux-x64.tar.xz  # 下载二进制安装包
```

2. node.js部署

```
cd /opt
tar xf node-v10.16.0-linux-x64.tar.xz 
ln -s node-v10.16.0-linux-x64 nodejs
nodejs -> node-v10.16.0-linux-x64
```

3. 版本信息

```
cd /opt/nodejs/bin
./node -v
会看到 v10.16.0
```

4. 创建软连接

```
ln -s /opt/nodejs/bin/node /usr/local/bin/node
ln -s /opt/nodejs/bin/npm  /usr/local/bin/npm

# 验证
node -v
```

### 3.3 关于 hexo

- 有些服务器安装hexo需要进行更多的配置

#### 3.3.1 安装hexo

```
npm install hexo-cli -g 
```

- 采用软连接的方式将hexo添加入全局变量

```
ln -s /opt/nodejs/lib/node_modules/hexo-cli/bin/hexo /usr/local/bin/hexo
```

#### 3.3.2 当安装hexo报错

- 解决错误：

Missing write access to /usr/local/lib/node_modules是没有写权限, npm官方给出的解决方案是新建一个有权限的文件夹, 在这个新文件夹中安装npm包. 这个方法不适用于Microsoft Windows系统.
在用户的根目录创建文件夹(名字不一定要是.npm-global, 可以自己起):

```
mkdir ~/.npm-global
```

- 设置npm全局包的安装路径:

```
npm config set prefix '~/.npm-global'
```

- 在用户的根目录下查看有没有.profile文件, 如果没有就创建, 然后用文本编辑器打开, 加上以下一行, 保存:

```
export PATH=~/.npm-global/bin:$PATH
```

- 回到终端运行以下命令, 让配置生效:

```
source /etc/profile
```

- 然后全局安装npm包就可以了.

[参考链接](https://blog.csdn.net/zhangxuekang/article/details/89075039)


2. 将 Hexo 所在的目录下的 node_modules 添加到环境变量之中即可直接使用 hexo <command>：

   echo 'PATH="$PATH:./node_modules/.bin"' >> ~/.profile

###  3.3.3 一些错误解决

- **hexo发生error:spawn failed错误的解决方法**

  - [原文链接](https://blog.csdn.net/HTL2018/article/details/106876940)

  - 问题原因：由于git进行push或者hexo d 的时候改变了一些 .deploy_git 文件下的内容


```
# 出现错误
error:spawn failed...
#解决方法
1. 删除blog文件夹中的 .deploy_git 文件夹
2. 输入 git config --global core.autocrlf false
3. 
	hexo clean
	hexo g
	hexo d

```

- **git add 博客文件提示博客主题是仓库**

  - 当我们对博客进行同步的时候，我们会使用git add 命令来添加同步文件，但是会提示hexo主题文件是一个仓库，不允许仓库里面套仓库上传，除非将里面的仓库也一起同步

  - 解决办法：按照提示，清除缓存，但是不要将主题仓库设置为同步


- **加载网页只显示文字**

  - 当我们加载网页的时候，我们可能会发现网页没有任何背景以及图片，只显示文字，这时候我们就应该在博客配置文件或者主题配置文件中关闭 **强制使用HTTPS** 这一选项，使用

  - GitHub page也是如此


##  4. 相关链接(有用)

| 名称                                                         | 网址                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [Waline评论](https://waline.js.org/)                         | https://waline.js.org                                        |
|                                                              |                                                              |
| [Hexo官网](https://hexo.io/zh-cn/index.html)                 | [hexo-theme-ayer:  gitee](https://gitee.com/shen-yu/hexo-theme-ayer) |
| [Hexo文档](https://hexo.io/zh-cn/docs/)                      | [hexo-theme-bamboo: gitee](https://gitee.com/yuang01/hexo-theme-bamboo) |
| [Hexo主题](https://hexo.io/themes/)                          | [hexo-theme-fluid: gitee](https://gitee.com/mxrmiss/hexo-theme-fluid) |
| [Hexo 部署到 GitHub Pages](https://hexo.io/zh-cn/docs/github-pages) | [hexo-theme-butterfly: gitee](https://gitee.com/immyw/hexo-theme-butterfly) |
|                                                              | [hexo-theme-ayer: github](https://github.com/shen-yu/hexo-theme-ayer) |
| [博客园，CSDN等平台利用Github绑定域名](https://social.blog.csdn.net/article/details/121484520) | [hexo-theme-bamboo: github](https://github.com/yuang01/hexo-theme-bamboo) |
| [typora+阿里云图床+印象笔记+OneDrive](https://blog.csdn.net/missmxr/article/details/121443438?spm=1001.2014.3001.5501) | [hexo-theme-fluid: github](https://github.com/fluid-dev/hexo-theme-fluid) |
| [图标网站：Font Awesome](https://fontawesome.com/)           | [hexo-theme-butterfly:github](https://github.com/jerryc127/hexo-theme-butterfly) |
|                                                              |                                                              |



