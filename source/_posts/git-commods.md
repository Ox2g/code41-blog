title: "GIT基础知识和命令（待补全）"
date: 2015-06-25 22:40:36
tags:
- 基础
- 工具
- git
categories: 
- 基础知识
---

## GIT 常用命令

0. 查看本地文件的git的状态  *git status*
1. 本地工程初始化 => *git init*
2. 为本地仓库绑定远程仓库地址 => *git remote add origin* __URL__

## GIT 分支操作
0. 查看远程分支 => *git branch -a*
1. 查看本地分支 => *git branch*
2. 推送分支到远程分支 => *git push origin* __BRANCH_NAME__
3. 切换分支 => *git checkout* __BRANCH_NAME__

## GIT Config 配置相关
0. 修改git请求URL中的协议头 => *git config --global url."https://".insteadOf git://*
> __[解决问题]__bower ECMDERR Failed to execute “git ls-remote ... exit code of #128

1. 查看当前配置信息  => *git config --list*
2. 根据git仓库配置不同的用户名和密码

## GIT 的忽略设置 __.gitignore__

## GIT的高级应用
### GIT的服务器搭建
### GIT的权限————gitolite