title: "GIT进阶知识和命令"
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

# 删除远程分支
git push origin :BRANCH_NAME
```
__PS:__如果需要reset远程分支状态的时候，可以先删除远程分支再进行重新push

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

## GIT 的忽略设置 __.gitignore__
>__忽略规则__
>* 以斜杠“/”开头表示目录
>* 以星号“*”通配多个字符
>* 以问号“?”通配单个字符
>* 以方括号“[]”包含单个字符的匹配列表
>* 以叹号“!”表示不忽略(跟踪)匹配到的文件或目录
>* .gitignore配置文件是按行从上到下进行规则匹配的，意味着如果前面的规则匹配的范围更大，则后面的规则将不会生效

```
# 配置全局的忽略配置
git config --global core.excludesfile ~/.gitignore_global 
```

__PS:__有一些常用的.gitignore配置可以参考[GITHUB的忽略配置Demo](https://github.com/github/gitignore/ "GITHUB的忽略配置Demo") 

## GIT的TAG
> GIT的打标签操作，主要是针对GIT的提交节点进行打标操作，同时可以为标签进行签名操作

```
git tag TAG_NAME            #添加标签
git tag                     #查看本地标签
git show TAG_NAME           #显示标签的信息
git tag TAG_NAME COMMIT_ID  #根据提交id创建标签
git tag -a TAG_NAME -m "message" COMMIT_ID  #根据提交id创建标签，同时添加说明
git tag -s TAG_NAME -m "message" COMMIT_ID #根据提交id创建标签，同时添加签名采用PGP签名
git tag -d TAG_NAME         #删除标签
git push origin TAG_NAME    #推送标签到远程
git push origin --tags      #推送全部标签
git push origin :refs/tags/TAG_NAME     #删除远程标签          
```

## GIT的服务器搭建
0. 服务器的搭建
搭建步骤
- 安装GIT
- 创建GIT用户，用于运行GIT服务
- 将需要登录用户的公钥放入  */home/git/.ssh/authorized_keys*
- 初始化Git仓库
- 禁用shell登录
- 克隆远程仓库

1. GIT的权限
- 小团队人员权限，将每个人的ssh公钥放到GIT服务器的 */home/git/.ssh/authorized_keys* 中
- 要方便管理公钥，用Gitosis；
- 要更全面控制权限，用Gitolite。 
