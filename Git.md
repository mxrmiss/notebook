# Git & Github

- git init

- Git 初始化

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

- git 无法推送新文件到github仓库原因

  1. 可能仓库内已经有东西了，而且使用的推送命令不对
  2. 或是原来的仓库内的东西是通过网页进行上传的， 而不还是通过命令行进行上传的
  3. 强制使用命令  “git commit 文件 +mster” 可能会使原仓库内的东西丢失，网上的教程都他妈的是骗人的

- 查看git简单配置

  ```
  git config --list
  ```

- 是git无密向github传输文件

  1. 终端输入ssh-keygen, 之后不断按回车
  2. 会产生一对公密和私密，存储在家目录下的 .ssh 目录中
  3. cat ～/.ssh/id_rsa.pub ，复制输出的公密内容至 github个人主页setting中的ssh中，并保存
  4. 以后clone仓库地址时就可以直接使用
  
- 删除github的文件

  1. github上可以删除仓库，但是不可以删除文件或文件夹

  2. 使用命令行方式删除个github上的文件

     ```
     # 将远程仓库的项目拉下来
     git pull url master
     # 删除文件夹
     git rm -r --cached dir
     # 提交，添加操作说明
     git commit -m '删除dir'
     # 更改本次更新
     git push -u url master
     ```

     
