---
title: git基本命令操作
date: 2020-08-19 10:40:35
tags: git基本命令操作
categories: git
description: 版本控制工具(linux命令在其中 git bash 同样适用 )
---



# git基本操作

## git初始化仓库

###  用来存放数据的仓库

```git
git init #初始化仓库会创建 .git文件夹
git init [指定文件夹] #在指定文件夹里面初始化仓库
```

如图：

{%asset_img git_init.jpg 初始化仓库%}

---



## 设置仓库签名

**区分开发人员身份**

**注意这里** ：设置签名跟远程库(代码托管中心Github)的账号密码没有任何关系

**形式：** 

+ 用户名： “tom”
+ email :    “xxxx@xx.com”

**级别：**

+ 项目级别（仓库级别）：仅仅在当前本地库范围内有效

  -  git config  user.name tom_pro
  - git config  user.eamil  xxxxx@xxx.com

+ 系统用户级别： 登录当前操作系统的用户范围

  -  git config **--global**   user.name tom_pro
  -  git config **--global**   user.eamil  xxxxx@xxx.com

  - 就进原则：如果项目级别和系统用户级别都设置了有先使用项目级别，如果只设置了系统级别那么就用系统级别
  - 二者都没有就不允许操作

```git
cat  .git/config #查看配置文件检查刚刚配置的签名
```





这个是项目级别的：

 {%asset_img set_design.jpg 查看项目级别签名%}   

```git
cat .gitconfig# 查看全局配置文件里面的系统用户级别签名
```





这个是系统级别的：

{%asset_img global_design.jpg 查看系统级别签名%}

---



## 查看仓库状态

`git status #可以用来查看仓库中文件状态`

{%asset_img git_status.jpg  查看仓库状态%}

---



## 在暂存区中添加文件

**我们可以创建一个a.txt文件**

```git
git add a.txt #将a.txt文件添加到暂存区 但是只是放到了暂存区
git add  --all #将添加所有文件到暂存区
```

**提交之后的状态使用git status查看**

{%asset_img git_status_add01.jpg 添加文件后的状态%}





**我们可以将暂存区的文件给撤销提交状态**

```git
git rm  --cached a.txt
```

**撤销后查看状态**

{%asset_img git_status_rm01.jpg 撤销文件后的状态%}

---



## 提交文件到仓库

**注意：我们要提交一个文件前应该先添加到暂存区 **

**注意：如果你没有描述文件信息message** 

那 么会进入到vim编辑器里面 使用 `set nu`可以查看行号

我们可以在第一行添加message 也可以直接wq退出 

{%asset_img commit01.jpg 提交文件时出现vim编辑器%}

```git
git commit  [ -m "message"]  a.txt  #提交文件到仓库中
git commit [ -m "message"] --all #提交缓存区所有文件到仓库中
```

再次查看状态可以发现提示信息显示没有可提交的文件

---



## 更新文件内容文件

**我们修改a.txt的内容后再次查看状态**

{%asset_img modified.jpg 修改文件内容后的状态%}

  **可以注意到我们可以下面提示我们可以add or commit 也就是我们可以直接提交，但是如果你直接commit修改的文件没有放到暂存区当你查看状态时会发现这个时候还是会有提示提示你！**

  **所以我们可以按照刚刚提交文件的方式先add再commit这样就不会提示你！**

{%asset_img modified02.jpg 提交修改后的文件%}



----

## 大致的流程

{%asset_img process.jpg 流程图}



---

##  查看日志

```git
git log #查看日志会显示之前的操作和版本号,最完整 （空格翻页）
git log --pretty=oneline  #日志按照一条一条显示，完整的hashcode
git log  --oneline  #显示部分的hashcode
git reflog #部分hashcode还包含HEAD@{恢复这个版本需要多少步}
```

{%asset_img log01.jpg git_log01}

{%asset_img log02.jpg git_log02}

{%asset_img log03.jpg git_log03}

{%asset_img log04.jpg git_reflog}

---

## 版本前进后退

**本质：通过一个指针进行前进后退**

**三种方式：**

+ 基于索引值操作【推荐】

​      {%asset_img  reset01.jpg  基于索引值%}

+ 使用^符号（只能后退不能前进）

  {%asset_img  reset02.jpg  基于^%}

+ 使用~符号（只能后退不能前进）

  {%asset_img  reset02.jpg  基于~%}

```git
git reset --hard 索引值 #部分hashcod(部分索引值)
git reset --hard HEAD^ #^回退一下 ^^回退两下
git reset --hard HEAD~N #回退N步
```



### reset 命令的三个参数的对比

  + soft 参数(了解)

    - 仅仅在本地库移动head指针

    实际上就是只有本地库改变了

    {%asset_img soft.jpg soft%}

  + mixed参数（了解）

    - 在本地库移动head指针
    - 重置缓存区

    本地库和暂存区都发生了改变

    {%asset_img mixed.jpg mixed%}

  + hard参数（常用的是hard）
    - 在本地库移动head指针
    - 重置缓存区
    - 重置工作区（就是能看见的文件目录，本地库【保存操作的版本】）



---

## 删除文件后找回

**文件并不是真正删除了只是换了一种方式保存**

**删除文件的命令:**

 ```git
rm  完整文件名 #会将本地库的文件删除 暂存区需要添加这个操作并commit
 ```

{%asset_img delete_file.jpg 删除文件%}

**找回文件（回退版本到没有删除的版本既可以找回文件,通过索引值找回）**

{%asset_img delete_reset.jpg 查看版本%}

```git
git  reset --hard  部分hashcode(索引值) #回退版本
```

{%asset_img find_file 找回文件%}

**如果我们已经rm 文件了工作区里面已经找不到这个文件了但是我们还没有add和提交delete操作**

{%asset_img find_file02.jpg 没有添加删除操作%}

```git
git reset --hard  HEAD  #前面介绍hard会刷新暂存区和工作区 没有加^ 和~ 代表刷新当前版本
```

{%asset_img find_file03.jpg 刷新暂存区工作区%}

---

## 比较文件差异

 ```git
git diff  文件名 #将工作区中的文件和暂存区进行比较 默认是比较上一个版本
git diff [本地库中历史版本] 文件名 #将工作区中的文件和本地库历史进行比较
 ```

{%asset_img diff_file01.jpg 比较上个版本文件%}

{%asset_img diff_file02.jpg 比较指定版本文件%}

---

## 提交文件的忽略

​    **描述当我们想要提交代码但是一部分代码我并不需要提交上去我们需要忽略一些不必要的文件**

​    **例如：我们在eclipse中提交文件时其实里面很多文件我们并不需要，里面包含了大量的eclipse项目部署的配置文件**

忽略文件分为两种：

+ 系统级别忽略

具体操作：在系统盘的用户目录下会有一个.gitconfig 我们只需要创建一个以.ignore指定结尾的文件进行配置

内容如下 ：*代表所有字符0~多个通配符匹配 #表示注释

```git
# Compiled class file
*.class

# Log file
*.log

# BlueJ files
*.ctxt

# Mobile Tools for Java (J2ME)
.mtj.tmp/

# Package Files #
*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar

# virtual machine crash logs, see http://www.java.com/en/download/help/error_hotspot.xml
hs_err_pid*

.classpath
.project
.settings
target 
```

把上述文件的绝对路径赋值下来配置到下面里面去

{%asset_img sys_ignore.jpg 系统级别忽略文件%}

+ 项目级别忽略

​      在.gti的本地仓库里面<font color='cornflowerblue'>他们会默认识别下面.ignore文件</font>我们只需要在里面配置需要忽略的文件

```git
z321/.deploy_git
z321/.gitignore
z321/node_modules
z321/public
z321/themes
```



{%asset_img pro_ignore.jpg 项目级别忽略文件%}