---
layout:     post
title:      Android手机/平板安装使用Linux Deploy安装Centos
categories: 废旧手机利用
subtitle:   
date:       2020-03-20
author:     yao-wen-chao
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - Android手机/平板
		- LinuxDeploy
		- Centos
---

# Android手机/平板安装使用Linux Deploy安装CentOS

手中有一个2016年底买的华为荣耀平板2，最近从一堆电子产品中把它翻了出来，本来想咸鱼卖了算了，结果二手回收估价，才估值150块钱，气得我肝疼。想了想，还是想办法废物利用一下吧，于是想到了把它改装成一个微型服务器。在网上搜了好多教程之后，自己又利用闲余时间鼓捣了三四天，最后终于成功了，在此记录一下。

## 准备工作

- 手机/平板已经root
- 需要用到的软件：Linux Depoly、busybox、JuiceSSH
## 操作步骤

### 安装busybox

网上在github网站下载一个busybox安装包，安装即可。（最好是安装ru.meefik版本的，与linux deploy是同一个版本）

### 安装Linux Deploy

同样在github网站下载一个Linux Deploy安装包，安装即可。

### Linux Deploy设置

首先点开左侧菜单，点击“配置文件”，点击编辑改个你喜欢的名字。然后点击“设置”，【锁定wifi】，【CPU唤醒】，【屏幕常亮】，【联网更新】勾上，【PATH变量】最重要，点击并填上/system/xbin，这样才能关联Busybox。然后点击下面的【更新环境】。

### TF卡分区

由于准备用这台旧平板做服务器，Linux Deploy安装linux系统有三种方式，文件方式是指安装好的系统都在一个img文件里，向系统里添加文件比较麻烦，我也没弄明白，不推荐；目录方式是设置一个目录，系统所有的文件都在一个目录下，想访问安装目录上级的其他文件需要挂载，而且文件系统不能大于2G，用作服务器感觉比较鸡肋。最后选用了分区方式，在外置TF卡中进行分区，一个32G内存卡可以分成多个分区，31G大小分区用来安装系统都可以，相当灵活。

### 配置文件设置

<img src="https://raw.githubusercontent.com/1100800824/pictures/master/blog/Screenshot_2020-03-25-18-51-10.png" alt="配置文件1" style="zoom:50%;" />

这里需要注意的几件事：

1.架构那一块是需要根据平板的CPU的架构来选择的，平板CPU的架构是`arm64`,但是这里没有这个选项，网上查阅了一些资料，资料显示`arm64`和`aarch64`实际上是同一回事，于是这里选择`aarch64`；

2.源地址可以改成国内的源地址，但是我在使用的时候发现，改成国内的清华、阿里的源地址后安装一直不成功，我也不知道跟这个源地址有没有关系，但是使用默认的源地址就可以安装成功，那么这里就不改了；

3.安装类型上面已经说过了，选择分区类型；

4.安装路径最后的那个`mmcblkXpY`,如果需要安装在内置存储的话，`X=0`；如果安装在外置存储卡上的话，`X=1`。`Y`的值需要根据卡的分区来确定，这次安装的时候，我选择安装在外置存储卡上的第2个分区里，所以`X=1,Y=2`。

<img src="https://raw.githubusercontent.com/1100800824/pictures/master/blog/Screenshot_2020-03-25-18-51-24.png" alt="配置文件2" style="zoom:50%;" />

这里需要注意的几个问题：

1. 用户名，密码根据自己的习惯设置即可；

2. 本地化这里选择`zh_CN.UTF-8`，本地系统可以正常显示中文汉字。

	<img src="https://raw.githubusercontent.com/1100800824/pictures/master/blog/Screenshot_2020-03-25-18-51-35.png" alt="配置文件3" style="zoom:50%;" />

这里需要注意的问题：

1.初始化这个选项，是用来设置启动安装好的linux系统时随着系统开启自动加载的服务的。如果需要设置开机自启的服务的话，需要勾选上。当然如果安装的时候没有勾选上的话，系统安装完成后使用的过程中可以随时勾选，更改了选择之后回到软件主界面，在右上角点击竖着的三个点，点击配置即可更新配置。

2.挂载这个选项，因为我选择的是分区安装，这个TF卡30G的空间都给linux系统使用了，就不挂载手机内存上的资源了。

3.SSH这个选项是一定要勾选上的，这样linux系统安装完成后，使用JuiceSSH这个软件根据前面设置的账号密码就可以登陆系统了。

<img src="https://raw.githubusercontent.com/1100800824/pictures/master/blog/Screenshot_2020-03-25-18-51-49.png" alt="配置文件4" style="zoom:50%;" />

这里需要注意的问题：

1.声音服务和图形界面根据自己实际情况选择，因为我主要是用来做服务器，不需要图形界面，只要能SSH远程登陆就可以了，所以这两个都没选。

### 安装

根据上面的配置文件配置好安装的参数后，按返回键返回到软件主界面，点击软件主界面右上角的三个竖点，选择安装，就进入到安装过程了。这个过程根据网络情况的不同，耗时也不一样。我大概安装了15分钟左右就结束了。（安装开始的时候软件会要求获取root权限，这个权限必须要给，否则安装无法进行；或者点击安装没有反应的话，需要进入授权软件给linuxdeploy这个软件授权）

开始安装：

<img src="https://raw.githubusercontent.com/1100800824/pictures/master/blog/173507_uB6p_2846946.png" alt="安装过程1" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/1100800824/pictures/master/blog/173521_s3rp_2846946.png" alt="安装过程2" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/1100800824/pictures/master/blog/173556_CeBe_2846946.png" alt="安装过程3" style="zoom:50%;" />

出现`<<<deploy`，说明安装过程已经结束了。

<img src="https://raw.githubusercontent.com/1100800824/pictures/master/blog/173654_bzEb_2846946.png" alt="安装过程4" style="zoom:50%;" />

启动系统之前，先点击一下`停止`。

<img src="https://raw.githubusercontent.com/1100800824/pictures/master/blog/173850_bV9L_2846946.png" alt="安装过程5" style="zoom:50%;" />

然后点击`启动`来启动系统，看到`<<<start`说明系统启动完成。

<img src="https://raw.githubusercontent.com/1100800824/pictures/master/blog/173946_p7FM_2846946.png" alt="系统启动" style="zoom:50%;" />

### 登陆

系统启动完成后，就可以使用JuiceSSH这个软件来登陆系统了。

PS：如果linux系统无法正常安装，启动不成功……等各种问题，先试试停止，然后将linux deploy软件杀后台，最后再次打开软件试试。如果还是不能正常操作的话，那么可以祭上万能大法：重启试试。当然此处的重启是指将手机或者平板重启一下，或许会收到意外的惊喜。