---
title: git本地库与远程库交互详解
date: 2020-08-20 13:46:57
tags: git本地库与远程库交互
categories: git
description: git本地库和远程库进行数据交互
---

# 本地库和远程库交互

## 本地库的准备工作

**准备步骤：**

>1.先初始化一下 仓库在需要上传的文件夹下进行 git init 操作

> 2.我们之前在其他库里面设置了项目级别签名但是初始化的库里面没有签名
>
> 我们可以执行：
>
> + git config  user.name 用户名
>
> + git config  user.eamil  邮箱

> 3.准备好远程库 并准备远程库的连接地址 例如github库地址
>
>  https://github.com/stringzhangsan999/test.git
>
> 当我们去复制连接时可以看到HTTP连接和SSH连接这是连接仓库的两种方式

**我们准备好本地库和远程库之后可以进行下面的操作**：

> **1.查看关联远程地址别名**

```git
git remote -v  #查看远程仓库地址别名
```

> **2.添加地址别名  （分HTTP地址连接和SSH地址连接 在代码上传下载那篇是SSH）** github仓库创建时他自己默认的别名就是origin所以我们也可以取一样的避免记得太多   

```git
git remote add 地址别名 地址 
```

{%asset_img address_alias.jpg 设置地址别名%}

> **3.如果在此之前有了别名就要清除别名**

```git
git remote rm 别名 #清除地址别名
```

---

## 推送代码到远程库

> **推送代码 推送代码时要保证本地库中有数据不然会报error: src refspec master does not match any**

```git
git push 地址别名 分支名
```

**注意：远程库如果不是空的需要先pull远程库中的内容把数据先下载到工作区才能进行push不然会报error: failed to push some refs to **

```git
git pull --rebase 地址别名 分支名 #远程库代码下载到本地库使用 --rebase会更好
```



---

## 下载代码到本地库

> 将远程库的代码下载到本地库

```git
git clone 远程库地址  #将远程库的内容下载下来
```

**效果：**

+ 完整把远程库下载到本地
+ 创建origin远程地址别名
+ 初始化本地库

## 邀请成员协同开发

**当你clone了远程的仓库之后如果你不是远程仓库的主人那么你是没有写的权限的**

**注意这里我们只是聊github代码托管的情况**

> 当你执行 git push origin master时输入密码，如果你不是仓库主人那么就需要仓库主人邀请你协同开发

**因为我们这里没有两个账号那么就不演示了**



>   如果你执行git push origin master 没有要你输入密码只能说是系统保存了 在控制面板下打开管理凭证就会看到

{%asset_img identify.jpg 凭证%}

---

## 远程仓库修改的拉取

> pull = fetch + merge
>
> ```git
> git fetch 远程地址别名 远程分支名  #拉取远程库的内容到本地库但是工作区内容还没有改变
> 
> git merge 远程地址别名/远程分支名        #合并远程库的内容更新工作区内容
> 
> git pull 远程地址别名 远程分支名              #先拉取在合并
> ```

{%asset_img fetch01.jpg 拉取%}

{%asset_img merge01.jpg 合并%}

最好两步分开来进行因为我们可以先查看文件的不同再决定要不要合并**

**当我们fetch之后想要查看远程库拉取下来的内容**

```git
git checkout  远程地址别名/远程分支名  #切换到远程拉取的库中

git checkout 本地分支名  #切换回本地的库中
```

{%asset_img checkout01.jpg 查看拉去内容%}

{%asset_img pull01.jpg pull操作%}

---

## 协同开发出现的冲突解决

> 假如小张修改了a文件 并且提交了而小刘也修改了a文件而且修改的是同一个地方
>
> 小张先提交不会有任何问题，当小刘去进行 fetch + merge操作时会出现冲突

{%asset_img conflict01.jpg 冲突%}

解决冲突的步骤：

+ 修改满意的代码去掉git添加的特殊符号
+ 添加到缓存区
+ commit解决冲突（不用加文件名）

> 处理完冲突之后直接推送到远程仓库
>
> `git push 远程地址别名 远程分支名`

{%asset_img resolve_conflict.jpg 解决冲突并推送%}

{%asset_img resolve_conflict02.jpg 查看远程库%}

## 跨团队操作流程（github）

 

 **场景：**小张 和小刘协同开发一个项目但是小刘发现交给他的任务他不会，正好他想请做过这个事情的    小赵来帮忙但是小赵不是他们公司的人。

**小赵想要帮忙必须：**

+ 【fork操作】fork项目到自己的账号下，相当于小赵有了整个项目并且这个项目一个被复制到他自己的远程库中所以他可以进行修改！
+ 修改好代码之后 pull request 发送合并审核给这个项目的主人

​       {%asset_img pullrequest.jpg 发送合并审核%}

+ 小张是这个项目的主人那么点击pull request 将查看合并请求 查看修改的代码
+ 如果审核没错之后点解 Merge pull request合并代码

<font color='red'>**没有那么多账号没有演示**</font>



## SSH免秘钥登录

**操作步骤：**

+ cd ~ 回到根目录
+ ssh-keygen -t rsa -C github注册邮箱   【这里的C是大写】

​       {%asset_img  SSH.jpg 生成秘钥%}

+ 一路回车，检查在C盘下Admin下有一个.ssh文件夹 产生两个文件 一个带pub的是公钥  一个不带pub那个是私钥

  {%asset_img  SSH02.jpg 两个秘钥%}

+ 在远程仓库保存公钥

  {%asset_img  SSH03.jpg 保存钥匙%}

**那么再进行push或者其他需要登录的操作就都不需要登录了**

      ```git
ssh-keygen -t rsa -C github注册邮箱   【这里的C是大写】#生成秘钥
      ```



​                