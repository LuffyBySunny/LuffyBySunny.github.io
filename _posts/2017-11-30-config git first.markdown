---
layout:   post
title:    "第一次使用"git配置
subtitle:		"gitgitgit"
date:		2017-11-30 12:00:00
author:		"Droodsunny"
header-img: "img/in-post/Android.jpg"

tags:
    - GitHub
---
> 第一次配置Git的简单步骤

### 1 安装Git客户端
   github是服务端，要想在自己电脑上使用git我们还需要一个git客户端，下载地址[Git](http://msysgit.github.com/)<br/>
   一路默认即可。
## 2 添加密钥
   1. 首先在本地创建ssh key
ssh-keygen -t rsa -C "email"<br/>
email 为自己的github邮箱，其他的是不是也行，我还没有尝试。<br/>
然后去这个文件夹C:\Users\Sunday\.ssh，当然不同的user不一样，找到一个后缀为pub的文件，用记事本打开。复制文本。然后去自己的github账号的Account Setting添加SSHKEY title随意，然后粘贴下来。
   2. 验证是否成功
   输入
``` java
  ssh -T git@github.com
```   
如果是第一次的会提示是否continue，输入yes就会看到：You’ve successfully authenticated, but GitHub does not provide shell access 。这就表示已成功连上github。
![](/img/in-post/1366089042_1867.png)
3. 接下来我们要做的就是把本地仓库传到github上去，在此之前还需要设置username和email，因为github每次commit都会记录他们。
```java
git config --global user.name "your name"
git config --global user.email "your_email@youremail.com"
```
其中your name 是你的github用户名，另外一个是登录邮箱。
然后进入到需要上传的代码文件夹，使用命令创建本地仓库
``` java
git init
```
然后使用下面命令连接到远程仓库
``` java
git remote add origin git@github.com:yourName/yourRepo.git
```
后面为你的远程地址
4. 上传代码
 git add .<br/>
 git commit -m "description"<br/>
 git push origin master

 5. 大功告成

# 进阶的git操作，未完待续。。。
