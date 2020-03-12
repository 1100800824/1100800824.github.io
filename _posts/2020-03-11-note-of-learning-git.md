---
layout:     post
title:      note of learning git
categories: git
subtitle:   
date:       2020-03-11
author:     yao-wen-chao
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - git
    - github
---

# Git使用笔记

## 创建仓库

如果是在Windows下进行操作，首先使用选择或者新建一个文件夹（目录的路径最好没有中文），然后在cmd或者powershell中使用cd命令进入这个目录。使用`git init`命令初始化一个仓库。此时需要注意的是，文件夹的名称就是这个仓库的名称。

## 向仓库添加文件

仓库创建好了之后，就可以在这个目录下面写东西了。需要注意的是，所有的版本控制系统只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等，Git也不例外。版本控制系统可以告诉你每次的改动，比如在第5行加了一个单词“Linux”，在第8行删了一个单词“Windows”。而图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化。不幸的是，Microsoft的Word格式是二进制格式，因此，版本控制系统是没法跟踪Word文件的改动的。所以我学习了markdown语法，使用Typora来编辑文本文件。

使用typora编写好文本文件后，将文件保存为`.md`格式的文档。然后使用`git add xxxx.md`命令向仓库添加文件。`git add`命令的意思是将文件添加到git仓库中。这一点需要注意，如果需要git管理仓库中的文件，需要做两件事情：1.将文件添加到仓库目录里；2.使用`git add`命令添加文件。如果是在仓库目录中修改已有的文件，那么每次修改过后都需要使用`git add`命令添加一次该文件。

修改文件后使用`git status`命令查看状态：

```
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   _posts/2020-03-11-note-of-learning-git.md

no changes added to commit (use "git add" and/or "git commit -a")
```

使用`git add xxxx.md`命令后再使用`git status`查看状态：

```
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   _posts/2020-03-11-note-of-learning-git.md
```



## 提交修改

提交修改使用`git commit -m ‘xxxxxx’`命令；此次使用`git commit -m ‘添加note of learning git'`命令提交修改，输出如下：

```
[master a990a74] 添加note of learning git
 1 file changed, 33 insertions(+), 1 deletion(-)
```

然后再使用`git status`命令查看状态：