---
title: 安装zsh
date: 2021/12/17
categories: linux
tags: [linux, shell]
top_img: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_7.jpg
cover: https://herozql.oss-cn-beijing.aliyuncs.com/bg_pic/bing_7.jpg
---



## 安装zsh

- Shell是在程序员与服务器间建立一个桥梁，它对外提供一系列命令，让我们得以控制服务器。常用的Bash就是Shell的一种，也是Linux下默认Shell程序。

### 查询shell

- 可以使用以下命令查看当前使用的目录

```
echo $SHELL
```

- 使用以下命令查看系统里面有什么shell

```
cat /etc/shells
```

### 安装zsh

- 使用以下命令安装zsh   【这里仅以debain及其衍生版作为例子】

```
sudo apt install zsh
```


## 安装oh-my-zsh

- Zsh虽然好用，但直接用起来比较麻烦，但是已经有大神配置好的框架：[oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)，专门为Zsh打造的

```shell
sudo apt install git
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
// 国内下载速度缓慢，建议将上面文件所在的仓库克隆到国内的Gitee中下载
```

## 切换为zsh

- 查看zsh的位置

```
which zsh
```

- 切换为zsh

```
chsh -s /usr/bin/zsh    //根据自己的zsh所在地址进行切换
// 然后输入你的账户密码
```

- 切换回bash

```
chsh -s /bin/bash
```

## 配置主题

- 方式一

>oh-my-zsh自带100多个主题，主题的配置文件在 ~/.zshrc文件中，找到这一行：
>
>```
>ZSH_THEME="robbyrussell"
>```
>
>你可以将值改成任意主题，oh-my-zsh的主题可以在[这里](https://github.com/robbyrussell/oh-my-zsh/wiki/themes)找到。保存好文件后，执行 source ~/.zshrc 使配置生效。如果你将值设为空，表示不使用任何主题；你也可以设置为"random"，你每次打开都会随机选择一个主题。
>
>推荐主题： agnoster
>
>由于这个主题使用了一些特殊字符，需要安装字体：
>
>```
>sudo apt-get install fonts-powerline
>```

- 方式二

>在搜索 zsh 资料的同时, 也间接看到了oh-my-zsh, 查看 git, 被安利了一波, 据说用它, 可以配制出非常完美的 zsh, 故, 下载, 安装, 使用
>
>```
>$ cd ~
>$ mkdir .oh-my-zsh
>$ cd .oh-my-zsh
>$ git clone https://github.com/ohmyzsh/ohmyzsh.git ./
>$ cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
>$ source ~/.zshrc
>```
>
>眼前一亮, 感觉世界充满了颜色

## 安装插件

- 安装插件(安装需要的就可以了)

- 语法高亮插件[zsh-syntax-highlighting](https://www.jianshu.com/p/4fde9ae77922) :输入的命令中间有错的时候会自动显示红色

```
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
```

- 自动补全插件 zsh-autosuggestions

```
git clone https://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
```

- 自动跳转插件 autojump

```
git clone git://github.com/joelthelion/autojump.git
cd autojump
./install.py
# 根据安装完成后的提示，在~/.bashrc最后添加下面语句：
vim ~/.bashrc    
[[ -s /home/misfit/.autojump/etc/profile.d/autojump.sh ]] && source /home/misfit/.autojump/etc/profile.d/autojump.sh
```

- 启用插件

```
# 编辑~/.zshrc   
vim ~/.zshrc    
# 找到plugins后括号里添加安装的插件名字
plugins=( git 
            autojump 
            zsh-autosuggestions 
            zsh-syntax-highlighting
            )
# 最后刷新
source ~/.zshrc    
```



## 参考链接

[参考链接一](https://www.cnblogs.com/lfri/p/10798105.html)

[参考链接二](https://www.cnblogs.com/cxl-blog/p/12347993.html)

[参考链接三](https://blog.gomyck.com/posts/zsh/)