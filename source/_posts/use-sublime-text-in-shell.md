title: 在shell中使用Sublime Text
date: 2015-09-17 14:45:03
tags:
- 工具
- Sublime
- 基础
- 编辑器
categories:
- 基础知识
- 编辑器
---

## 在Shell中配置Sublime Text

Sublime Text是一种常见的编辑器软件，并且被很多人喜欢。而鉴于多平台的原因，Sublime Text在不同的平台中需要使用的不同的配置方式。

- Linux:

```bash
# 在~下的目录下的.bashrc或者.zshrc文件中添加
alias subl="/usr/local/bin/subl"
```

- windows:

>由于习惯使用Git Bash作为命令行讲解工具，故配置同Linux下相同

```bash
# 在~下的目录下的.bashrc或者.zshrc文件中添加
alias subl="/d/bin/Sublime\ Text\ 3/subl.exe"
# 同理，再配置一个chrome的Shell命令
alias 
```

- Mac:

```bash
alias subl='/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl'
```

PS: 具体程序名称等需要按照对应的修改下配置内容

## subl命令相关参数

```bash
subl -h
```

在Shell中运行以上的命令后，会看到提示的相关帮助。
```bash
$ subl -h
Sublime Text build 3083

Usage: subl [arguments] [files]         *打开指定的文件[ subl app.js]*
   or: subl [arguments] [directories]   *打开制定的文件夹[ subl ./]*

Arguments:
  --project <project>: Load the given project *载入指定的project*
  --command <command>: Run the given command *运行指定命令*
  -n or --new-window:  Open a new window *打开新的窗口*
  -a or --add:         Add folders to the current window *添加文件夹到当前窗口*
  -w or --wait:        Wait for the files to be closed before returning *返回前等待文件关*
  -b or --background:  Don't activate the application *后台运行*
  -s or --stay:        *在关闭文件后，保持存活*
  -h or --help:        Show help (this message) and exit *显示帮助*
  -v or --version:     Show version and exit *显示版本号*
```
