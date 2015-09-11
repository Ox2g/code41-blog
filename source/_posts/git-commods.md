title: "GIT基础知识和命令"
date: 2015-06-25 22:40:36
tags:
- 基础
- 工具
- git
categories: 
- 基础知识
---

## GIT 常用命令及基本操作

0. 查看本地文件的git的状态
```
git status
```
1. 本地工程初始化
```
git init
```
2. 为本地仓库绑定远程仓库地址
```
git remote add origin URL
```

<!-- more -->

3. 常用基本操作
```
# 添加
git add FILE        #添加文件
git add *.FILE      #通配符添加文件
git add .           #添加全部修改的文件

# 还原add前的文件，就是将文件中修改的内容都revet
git checkout -- FILE

# 还原add后，commit前的文件到add前的状态
git reset HEAD FILE

# 查看修改文件的差异
git diff
git diff FILE

# 提交
git commit -m "要提交的说明Comment" FILE   # 提交 单个文件
git commit -m "要提交的说明Comment" -a     # 提交 所有修改文件
git commit -C head -a -amend              # 增补提交，不会产生新的提交历史

# 查看提交日志
git log
git log --pretty=oneline        # 一行查看提交日志

# 还原commit后的操作，还原节点后的提交日志不能查看了
git reset --hard COMMIT_ID      # 按照COMMIT_ID还原文件，COMMIT_ID可以只写前几位
git reset --hard HEAD^          # 还原上次的文件，如果^^，则还原上上次

# 查看GIT操作命令历史记录
git reflog

# 删除文件
git rm FILE     # 删除文件
git checkout -- FILE # 删错找回

# 推送本地文件
git push

# 拉取远程文件
git pull

# 克隆远程仓库
git clone URL
```

    #### *附：GIT工作区与暂存区*
    > 修改前   =>  添加内容&修改文件   =>    git add     =>  git commit
    > 无状态   =>  文件在工作区       =>    文件在暂存区  =>  文件在本地分支

    #### *附：SSH操作*
    > 为了减少git提交时反复输入用户名和密码的操作，故大部分采取SSH连接的方式进行提交，而通过本机的公钥文件配置在git服务器上就可以完成不用密码的提交操作。

    - 查看本地是否存在SSH keys
    ```
    ls -al ~/.ssh
    ```
    + ssh-keygen生成新的id_rsa和id_rsa.pub文件（*rsa为加密方式*）
    ```
    ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
    ```
    + 配置保存公钥和私钥的文件位置和文件名
    ```
    Enter file in which to save the key (/Users/you/.ssh/id_rsa): [Press enter]
    ```
    + SSH密钥使用密码（可不配置）
    + 在git仓库中配置(*如果将id_rsa.pub放到另外机器的 ~/.ssh.authorized_keys中，可以通过ssh和scp无密码登录访问*)


## GIT Config 配置相关
0. 修改git请求URL中的协议头
```
git config --global url."https://".insteadOf git://
```
 > __[解决问题]__bower ECMDERR Failed to execute “git ls-remote ... exit code of #128

1. 查看当前配置信息
```
git config --list
```
2. 配置操作
    - 添加配置
    ```
    // 添加 'git add .' 的全局别名
    git config --global alias.a 'add .'
    ```
    - 删除配置
    ```
    // 删除别名
    git config --unset alias.s
    ```
3. 根据git仓库配置不同的用户名和密码
    - 根据不同的用户名生成不同的SSH keys
    - 去除全局的user.name和user.email的设置
    ```
    git config --global --unset user.name
    git config --global --unset user.email
    ```
    - 修改用户下的config文件（*\~/.ssh/config*）
    ```
    #Default Git
    Host github.com
        HostName github.com
        User code41
        PreferredAuthentications publickey
        IdentityFile ~/.ssh/id_rsa.code41.pub

    Host github.com
        HostName github.com
        User naruto900814
        PreferredAuthentications publickey
        IdentityFile ~/.ssh/id_rsa.naruto900814.pub

    Host demo.com
        HostName demo.com
        User shaw
        PreferredAuthentications publickey
        IdentityFile ~/.ssh/id_rsa.shaw.pub
    ```
    > 以上配置，访问github时，根据不同的用户名，使用不同的公钥文件；通过在git的repo中配置不同的user就可以使用以上配置按不同用户提交；当git提交到不同的GIT仓库的时候就会使用不同配置，从而使用不同的账户和公钥文件。
    - 删除历史信任记录
    ```
    rm ~/.ssh/known_hosts
    ```
    - 禁用伪终端分配
    ```
    ssh -T shaw@demo.com
    ```
