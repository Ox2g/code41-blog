title: "TODO:GIT进阶知识和命令"
date: 2015-07-09 00:21:50
tags:
- 基础
- 工具
- git
- 进阶
categories: 
- 基础知识
---

## TODO:GIT 分支操作
```
# 查看远程分支
git branch -a

# 查看本地分支
git branch

# 推送分支到远程分支
git push origin BRANCH_NAME

# 切换分支
git checkout BRANCH_NAME
```

## TODO:GIT 的忽略设置 __.gitignore__
>__忽略规则__
>* 以斜杠“/”开头表示目录
>* 以星号“*”通配多个字符
>* 以问号“?”通配单个字符
>* 以方括号“[]”包含单个字符的匹配列表
>* 以叹号“!”表示不忽略(跟踪)匹配到的文件或目录
>* .gitignore配置文件是按行从上到下进行规则匹配的，意味着如果前面的规则匹配的范围更大，则后面的规则将不会生效

## TODO:GIT的TAG

## TODO:GIT的服务器搭建
0. 服务器的搭建
1. GIT的权限————gitolite