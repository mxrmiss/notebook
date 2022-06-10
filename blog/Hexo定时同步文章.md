---
title: Hexo定时同步文章
date: 2022/1/13
categories: blog
tags: [linux, blog, script]
index_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_34.jpg
banner_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_34.jpg
---

- 使用hexo 搭建博客有许多好处，但是也有许多麻烦，不过可以一一解决，接下来分享一下我的hexo博客自动化定时同步脚本

- 使用情景：

>1. hexo 部署在 Linux系统上
>2. 最好使用 Linux 服务器

- 脚本使用 Linux bash shell 编写，可以自行修改：

# 1. nginx部署单个网站

```bash
#!/bin/bash
# 该脚本的是调用博客文件夹中 hexo.sh
# hexo.sh 的作用是发布博客
# 该脚本位于/etc/cron.daily目录，作用是每天更新博客,每周末清除日志内容，包括错误和正确的日志

echo "loading..."

if [ $(date +%u) -eq 7 ]
then
    echo "" > /root/story/blog_daily.log
    echo "" > /root/story/blog_daily_error.log	
fi

exec 2>>/root/story/blog_daily_error.log
echo "=============****错误日志****===============" >&2 
date >&2; echo "" >&2

exec 3>&1
exec 1>>/root/story/blog_daily.log
echo "=============*****运行日志*****================"
date;echo ""

cd /root/story
git pull
cd /root/story/blog
hexo clean
hexo g -d

exec 1>&3
echo "loaded successfully!"
```

 

- 解释一下脚本内容：

>- **运行脚本的前提：**
>
>1. 首先创建一个GitHub或Gitee类型的仓库，并将仓库克隆到Linux服务器中，在上面的脚本中，我的仓库名称就是 story
>2. 然后再被克隆的仓库中创建博客文件夹，在上面的脚本中，我的博客文件夹就是 blog
>3. 接着在仓库文件夹中创建两个日志文件，一个是记录正确信息的日志`blog_daily.log`，一个是记录错误信息的日志blog_daily_error.log
>4. 将创建的文件放入 /etc/cron.daily , 如果没有这个文件夹，自己查询自己的Linux系统中cron程序文件夹在哪里，或者你需要更加精确的定时运行脚本，可以在 /etc/crontab 中修改参数
>
>- **脚本内容：**
>
>1. 每天同步仓库，并更新博客，记录正确和错误的输出到日志中
>2. 每周末清除日志文件中的内容

# 2. 部署多个网站

- 这里以宝塔面板部署多个站点为例：

```bash
#!/bin/bash
# 该脚本的是调用博客文件夹中 hexo.sh
# hexo.sh 的作用是发布博客
# 该脚本位于/etc/cron.daily目录，作用是每天更新博客,每周末清除日志内容，包括错误和正确的日志

echo "loading..."

if [ $(date +%u) -eq 7 ]
then
    echo "" > /root/story/blog_daily.log
    echo "" > /root/story/blog_daily_error.log	
fi

exec 2>>/root/story/blog_daily_error.log
echo "=============****错误日志****===============" >&2 
date >&2; echo "" >&2

exec 3>&1
exec 1>>/root/story/blog_daily.log
echo "=============*****运行日志*****================"
date;echo ""


cd /root/story
git pull
cd /root/story/blog
hexo clean
hexo g -d
cd /www/wwwroot/oldstory.cn/public/

for i in `ls -a`
do 
    if [ $i != .user.ini ] && [ $i != . ] && [ $i != .. ];then
        rm -rf $i
    fi
done

cp -r /root/story/blog/public/* ./


cd /root/story/life
hexo clean
hexo g -d
cd /www/wwwroot/life.oldstory.cn/public/

for i in `ls -a`
do 
    if [ $i != .user.ini ] && [ $i != . ] && [ $i != .. ];then
        rm -rf $i
    fi
done

cp -r /root/story/life/public/* ./
exec 1>&3
echo "loading successfully!"
```

