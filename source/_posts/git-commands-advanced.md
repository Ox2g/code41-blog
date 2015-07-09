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

## GIT 分支操作
```
# 查看远程分支
git branch -a

# 查看本地分支
git branch

# 推送分支到远程分支
git push origin BRANCH_NAME

# 获取远程分支
git checkout -b BRANCH_NAME origin/BRANCH_NAME

# 建立本地分支和远程分支的关联
git branch --set-upstream BRANCH_NAME origin/BRANCH_NAME

# 切换分支
git checkout BRANCH_NAME
# 创建分支
git branch BRANCH_NAME

# 创建并切换分支
git checkout -b BRANCH_NAME

# 合并分支(在需要合并代码的分支中，选择目的分支进行合并)
git merge BASE_BRANCHE_NAME
git merge BASE_BRANCHE_NAME --no-ff     #保留日志方式合并（推荐）

# 删除分支
git branch -d BRANCH_NAME
git branch -D BRANCH_NAME   #强制删除分支

# 
```

### 分支中工作现场
> 当存在需要修理的bug时，而手边工作为修改完成的情况下，可以保存现有的工作现场，而在bug修正后进行工作现场恢复，继续之前的开发工作

```
# 保存工作现场
git stash

# 查看工作现场列表
git stash list

# 恢复工作现场
git stash pop   #恢复上一个工作现场
git stash apply stash@{0}
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