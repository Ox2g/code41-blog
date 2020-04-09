title: GIT的Rebase和Merge的区别
date: 2017-09-03 12:36:18
tags:
- 基础
- 工具
- git
categories: 
- 基础知识
---
## GIT的Rebase和Merge的区别

### 目的
区分GIT中Rebase和Merge的区别

### 详解
0. 模拟当前本地分支和远程分支的提交点情况
    - ![](http://img3.0x29.cn/markdown/1504415696066.png)
1. Merge是对分支的合并操作，按照提交的顺序排列全部Commits
    - ![](http://img3.0x29.cn/markdown/1504415729898.png)
    - 创建一个新的Commit，按照提交时间顺序规整全部Commit提交点
2. Rebase也是对分支的合并操作，但是远程Commit的全部提交点会排列在本地的为提交点顺序之前
    - ![](http://img3.0x29.cn/markdown/1504415756949.png)
    - 保持远程的提交顺序，重新复制创建出本地的全部提交点备份，并排列备份提交到远程顺序之后
    - 优点
        + 保证全部GIT的提交链的顺序准确性

### Rebase本地冲突处理方式
1. 出现冲突后，处理冲突
2. 处理冲突时，如果修改好冲突文件后，使用**git add -u**进行冲突文件的提交操作
3. 之后使用**git rebase --continue** 继续Rebase操作
3. 在使用**git rebase --continue**时，提示__applying no changes__时，确认冲突处理完全后，使用**git Rebase--skip**继续执行
4. 出现冲突后，打算恢复成Rebase操作前的样子，使用**git rebase --abort**恢复

