---
title: 代码上传下载远程git仓库
date: 2020-08-19 17:28:54
tags: 代码上传下载git仓库
categories: git
description: 远程机器代码上传和下载
---

# 远程git仓库的操作

## 上传代码

**我们现在远程的机器上面初始化一个git仓库**

注意我们最好使用bare参数来初始化远程仓库（这个是看了其他人博客）

```git
git init --bare 仓库名称
```

 {%asset_img init_repositry.jpg 初始化远程仓库 %}

**查看是否关联远程的git仓库地址**

```git
 git remote -v #查看远程关联的仓库
```



{%asset_img associate_repositry.jpg  查看关联的仓库%}

**如何清空关联的仓库**

```git
git remote rm origin #清除关联的远程仓库
```



**关联一个远程的git仓库地址**

```git
git remote add origin git@ip:/home/git/blog_me (仓库路径)
```



**注意：直接 git push会报  current branch and set the remote as upstream **

**表示我们需要push时指定一个分支，输入下面命令就会直接上传代码**

```git
git push --set-upstream origin master #我们默认推送在master分支
```

**这里我们还需要输入密码，也就是远程机器的密码这个密码与你远程计算机用户名的密码对应**

---

## 下载git仓库代码

**打开控制台 git bash here输入一下命令**

```
 git clone git@ip:/home/git/blog_me  下载的文件夹名称
```



**也需要输入密码和上面push一样，上传下载都需要输入机器的登录密码**

