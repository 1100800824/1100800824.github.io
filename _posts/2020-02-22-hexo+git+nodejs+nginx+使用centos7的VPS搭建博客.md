---
layout:     post
title:      hexo+git+nodejs+nginx+使用centos7的VPS搭建博客
subtitle:   搭建博客
date:       2020-02-22
author:     yao-wen-chao
header-img: img/post-bg-ios9-web.jpg
catalog: true
categories: 博客
tags:
    - hexo
    - nodejs
    - nginx
---
废话不说，直接上干货。

## 本机（个人电脑）上的操作（以win10 64位系统为例）

### 安装nodejs

在官网下载软件，一路默认安装即可。

### 安装Git(不要使用最新版本，2.10左右即可)

#### 安装

在[淘宝镜像](https://npm.taobao.org/mirrors/git-for-windows/)上下载合适的版本，一路默认安装即可（在安装时记得勾选add to path）。

####  生成ssh密钥

按下面的步骤进行：

1.打开 `C:\Users\<用户名>\.ssh`文件夹，如果没有就新建
		2.在空白处单击右键，选择`Git Bash Here`打开终端
		3.设置git用户名<br/>

```
git config --global user.email "email@example.com"
git config --global user.name "username"
```

4.生成ssh密钥

```
ssh-keygen -t rsa -C "email@example.com"
```

### 创建网站目录

在电脑的任意位置创建一个文件夹（例如`E:\hexo`，下文以此代替），作为网站目录。

### 安装 `hexo`

打开 `powershell`, 通过`cd`命令进入`E:\hexo`, 执行如下命令：

```
npm install -g hexo-cli
hexo init
npm install
hexo d -fg
hexo serve
```

打开[http://localhost:4000](http://localhost:4000/) 即可看到你的站点（当然还没有发布到网络）。

# 在安装有centos7 的VPS中的操作

关于如何购买VPS，购买域名，备案，设置DNS等操作在此就不赘述了，请自行百度。

此处为`centos`在`root`用户下的操作，关于连接VPS，Windows 用户请使用[Putty](http://the.earth.li/~sgtatham/putty/latest/x86/putty.exe)（提示： Putty 中使用粘贴仅需鼠标右键）。关于vi操作，按下i键进入编辑模式，编辑结束后按`esc`键退出，这时按`：` ，并输入 `wq` ，即可。

## 安装git-core

我使用的centos7 64位系统自带git, 如果没有的话，使用如下命令：

```
yum update && apt-get upgrade -y #更新内核
yum install git-core
```

## 安装nodejs(不是必要的，只是折腾到这里了，顺便记录一下)

这里我推荐使用nvs安装nodejs，具体操作详见<https://github.com/jasongin/nvs/> ,简要步骤如下：

```
export NVS_HOME="$HOME/.nvs" #指定安装路径
git clone https://github.com/jasongin/nvs "$NVS_HOME" #克隆副本
. "$NVS_HOME/nvs.sh" install #安装nvs
nvs add lts #安装最新的LTS版本
nvs use lts #使用该版本
nvs link lts #将其永久加入到path中
```

以上操作完成后，VPS中应该安装好了nodejs 和npm。

## 安装 nginx

参考[[Zhanming's blog](https://qizhanming.com/blog/2018/08/06/how-to-install-nginx-on-centos-7)的文章，需要先添加源，然后使用`yum install`命令安装。具体命令如下：

```
sudo rpm -ivh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
sudo yum install nginx
sudo systemctl enable nginx
sudo systemctl start nginx
```

VPS需要打开一些端口，比如80 ，443等，常见命令如下：

```
firewall-cmd --zone=public --add-port=80/tcp --permanent  #永久打开80端口
firewall-cmd --reload #防火墙重载
firewall-cmd --zone=public --list-ports #查看所有打开的端口
```

### 新建git用户添加sudo权限

```
adduser git
chmod 740 /etc/sudoers
vim /etc/sudoers
```

在vi编辑中找到如下内容：

```
## Allow root to run any commands anywhere
root    ALL=(ALL)     ALL
```

在下面添加一行

```
git   ALL=(ALL)     ALL
```

保存并退出后执行

```
chmod 440 /etc/sudoers
```

### 创建git仓库，并配置ssh登录

```
su git
cd ~
mkdir .ssh && cd .ssh
touch authorized_keys
vi authorized_keys//在这个文件中粘贴进刚刚申请的key（在id_rsa.pub文件中）
cd ~ 
mkdir hexo.git && cd hexo.git
git init --bare
```

测试一下，如果在`git bash`中输入`ssh git@VPS的IP地址`,能够远程登录的话，则表示设置成功了。

这里我一直没成功，一直显示`Are you sure you want to continue connecting (yes/no/[fingerprint])?`输入`yes`后让输入密码，可是我开始就没设置密码，最后参照[回风](https://huifeng.me/2015/08/16/deploy-hexo-to-vps/)的博客，看到了这个：

如果出问题了,请试试看看运行 `ll -a /home/git/`, 看看`.ssh`目录的拥有者是否是`git:git`,实在不行就运行:

```
chown -R git:git .ssh
chmod 700 .ssh
chmod 600 .ssh/authorized_keys
```

如果还不行，试试在本机电脑.ssh文件夹下（也就是生成公钥的文件夹）创建config文件，输入如下内容：

```
# alex's git server
Host VPS的IP
HostName VPS的IP
User git
Port SSH端口
IdentityFile ~/.ssh/id_rsa
```

### 创建网站目录并赋予git对网站目录的所有权

```
cd /var/www
mkdir hexo
chown git:git -R /var/www/hexo
```

### 配置git hooks

```
su git
cd /home/git/hexo.git/hooks
vim post-receive
```

输入如下内容后保存退出，

```
#!/bin/bash
GIT_REPO=/home/git/hexo.git #git仓库
TMP_GIT_CLONE=/tmp/hexo
PUBLIC_WWW=/var/www/hexo #网站目录
rm -rf ${TMP_GIT_CLONE}
git clone $GIT_REPO $TMP_GIT_CLONE
rm -rf ${PUBLIC_WWW}/*
cp -rf ${TMP_GIT_CLONE}/* ${PUBLIC_WWW}
```

然后赋予脚本的执行权限

```
chmod +x post-receive
```

### 配置Nginx

```
vim /etc/nginx/conf.d/hexo.conf

插入如下代码：
server {
    listen         80 ;
    root /var/www/hexo;//这里可以改成你的网站目录地址，我将网站放在/var/www/hexo
    server_name example.com www.example.com;//这里输入你的域名或IP地址
    access_log  /var/log/nginx/hexo_access.log;
    error_log   /var/log/nginx/hexo_error.log;
    location ~* ^.+\.(ico|gif|jpg|jpeg|png)$ {
            root /var/www/hexo;
            access_log   off;
            expires      1d;
    }
    location ~* ^.+\.(css|js|txt|xml|swf|wav)$ {
        root /var/www/hexo;
        access_log   off;
        expires      10m;
    }
    location / {
        root /var/www/hexo;//这里可以改成你的网站目录地址，我将网站放在/var/www/hexo
        if (-f $request_filename) {
            rewrite ^/(.*)$  /$1 break;
        }
    }
}
```

重启Nginx

```
service nginx restart
```

## 本机的最后配置

### 配置hexo配置文件

位于`hexo`文件夹下，`_config.yml`,修改`deploy`选项

```
deploy:
  type: git
  message: update
  repo:
    s1: git@VPS的ip地址或域名:git仓库地址,master
##例如
deploy:
      type: git
      message: update
      repo:
    		s1: git@45.63.45.446:hexo.git,master
```

接着在`hexo`文件夹内，按住`shift`右击，选择`在此处打开命令窗口`（当然你也可以用`cd`命令），运行`hexo g hexo d`，如果一切正常，静态文件已经被成功的push到了blog的仓库里。以上，博客已经完全建好了。

#### 这里遇到的坑

`hexo g` 报错，安装hexo-delopyer-git后解决。



## 更新博客

使用一款 MarkDown 编辑器写 Blog 。写完后将文件以 *.md 的格式保存在本地`[网站目录]\source\_posts`中。文件编码必须为 UTF-8，这一点仅 Windows 用户需注意。

每篇 Blog 都有固定的参数必须填写，参数如下，注意每个参数的 : 后都有一个空格。

```
title: title
date: yyyy-mm-dd
categories: category  
tags: tag
#多标签请这样写：  
#tags: [tag1,tag2,tag3]
#或者这样写： 
#tags:
#- tag1 
#- tag2  
#- tag3  
---  
正文  
```

编写完后，只需要在hexo文件夹下执行`hexo g && hexo d`，博客即可更新。

