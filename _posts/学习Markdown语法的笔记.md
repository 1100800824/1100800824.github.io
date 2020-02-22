---
layout:     post
title:      note of learning markdown
subtitle:   Hello Markdown
date:       2020-02-22
author:     yao-wen-chao
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - markdown
---

# 学习Markdown语法的笔记

## 字体加粗与斜体

下面展示如何加粗字体，**加粗字体**是这样的，接下来展示如何使用斜体，*斜体*是这样的。然后是加粗斜体，答案当然很简单，那就是**加粗**和*斜体*的组合啦，是***这样***的。  ~~删除线~~是这样的，<u>下划线</u>是这样的。
***
***
## 超链接
好的，我们继续往下学习。
[github](https://github.com/1100800824)是一个指向我的**github账户**的链接，[知乎](https://www.zhihu.com/people/yao-wen-chao)是指向我**知乎账户**的链接。
***
***
## 表格
学习了链接之后，我们继续，让我们一起来试试表格。
|姓名|技能|排行|行规|
|--:|:--:|:--|--|
|刘备|哭|大哥|哭天|
|关羽|打|二哥|喊地|
|张飞|骂|三哥|飞天|
***
***
## 代码块
好的，目前看起来学习效果不错，我们继续往下进行吧
单行代码是这样的：`import os`
多行代码是这样的：
```
import os

def creat_dir(name)
​		if not os.path.exist(name):
​				mkdir(name)
```
****
***
## 列表
那么，接下来我们试试列表吧
​	无序列表是这样的：
​		-  这是第一行
​		-  这是第二行
​    	-  这是第三行
​	有序列表是这样的：
​		1. 这是第一行
​		2. 这是第二行
​		3. 这是第三行
***
***
## 引用
真棒，下面我们来试试引用是如何工作的<br/>引用时只需要在引用的文字前加>即可。
> 这是一个引用
> > 这也是一个引用
> > > 这还是一个引用
> > > > 这依然是一个引用
> > > >
> > > > > 好吧，我要疯了，这还是一个引用
***
***
## 插入图片
好的，下面我们来试试如何插入图片<br/>![yao-wen-chao](https://github.com/1100800824/hello-world/blob/master/about-me.jpg "yao-wen-chao")
