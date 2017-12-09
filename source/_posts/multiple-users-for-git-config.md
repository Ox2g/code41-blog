title: Git的多用户配置
date: 2017-12-09 19:52:18
tags:
- 基础
- 工具
- git
- 多用户
categories: 
- 基础知识
---
## Git的多用户配置

1. 生成多个代码库的SSH-KEY，考虑不同代码库的账户名不同，故生成私钥和公钥的时候考虑使用不同的文件名。
```
ssh-keygen -t rsa -C "XXX@mail.com"
```
2. 加载SSH-KEY
    - 执行ssh-add命令
    ```
    ssh-add id_rsa_github ## id_rsa_github为上一步生成的私钥文件
    ```
    - 可能提示失败，如“could not open a connection to your authentication agent”
    - 出现上述情况为当前账户SSH添加权限问题，使用如下命令解决
    ```
    eval `ssh-agent`
    ssh-add id_rsa_github ## id_rsa_github为上一步生成的私钥文件
    ```
3. 配置多仓库多用户配置
    - 在当前用户的目录的".ssh"文件夹下添加config文件，文件地址如[~/.ssh/config]
    - 在config文件中添加多仓库站点的配置
    ```
    Host github.com
      HostName github.com ##站点地址
      IdentityFile /Users/XXX/.ssh/id_rsa_github ##私钥文件地址
      User XXX1 ##用户名
    Host github.com
      HostName github.com ##站点地址
      IdentityFile /Users/XXX/.ssh/id_rsa_github2 ##私钥文件地址
      User XXX2 ##用户名
    Host git.coding.net
      HostName git.coding.net ##站点地址
      User xxx@mail.com ##用户名
      PreferredAuthentications publickey ##配置登录时用什么权限认证--可设为publickey,password publickey,keyboard-interactive等
      IdentityFile /Users/XXX/.ssh/id_rsa_coding ##私钥文件地址
    ```
    - 配置解读
        + 当前配置支持相同站点不同用户，根据提交代码的用户名自动使用不同的用户权限进行提交
        + 需要在代码所在路径下，配置当前代码使用的用户名
        + 也可以添加多个站点的配置
4. 校验配置，使用“ssh -T”命令，出现成功则代表配置成功
```
ssh -T git@github.com ##校验github
ssh -T git@git.coding.com ##校验coding.net
```







