---
title: git在Eclipse中的插件操作
date: 2020-08-20 17:10:28
tags: git在Eclipse中的插件
categories: git
description: 关于eclipse中git插件操作
---

## git在eclipse中的插件

## 初始化仓库

**我们可以知道在eclipse中已经内置了git插件所以我们不需要去安装git插件**

{%asset_img git_plugin.jpg eclipse自带的git插件%}

**我们创建一个Maven项目将项目初始化为git仓库**

{%asset_img git_plugin02.jpg 创建git仓库%}

{%asset_img git_plugin03.jpg 创建git仓库%}

**查看项目变化**

{%asset_img git_plugin04.jpg 创建git仓库%}

## 提交代码时忽略特定文件

 <font color='red'> **Navigator 里面包含了项目所有的文件也会当成未提交的文件，里面包含了许多Eclipse为了管理项目的项目配置文件，对于我们来说是不需要的我们需要忽略这些特殊的文件**</font>

  例如：

+ .classespath文件
+ .project文件
+ .settings文件

这些都是eclipse为了管理我们项目所创建的文件，我们并不需要

**为什么要忽略这些文件：**

> 每个人开发用的工具不同所以配置信息不同

> 每个开发工具有不同的配置信息如果不忽略掉当下载时会出现冲突

> 为了解决冲突我们必须忽略掉这些文件

---

## 如何忽略文件

**GitHub官网示例：**

[查看忽略示例文件]( https://github.com/github/gitignore )

[关于java忽略示例文件]( https://github.com/github/gitignore/blob/master/Java.gitignore )

| **Java.gitignore**                                           |
| ------------------------------------------------------------ |
| # Compiled class file<br/>*.class<br/><br/># Log file<br/>*.log<br/><br/># BlueJ files<br/>*.ctxt<br/><br/># Mobile Tools for Java (J2ME)<br/>.mtj.tmp/<br/><br/># Package Files #<br/>*.jar<br/>*.war<br/>*.nar<br/>*.ear<br/>*.zip<br/>*.tar.gz<br/>*.rar<br/><br/># virtual machine crash logs, see http://www.java.com/en/download/help/error_hotspot.xml<br/>hs_err_pid* |

在~/.gitconfig 文件中引入上述文件 

[core]

​    excludesfile=C:/Users/Admin/Java.gitignore<font color='red'>（这里这个路径是可以换的不是固定的而且这里的斜线一定是/）</font>

操作步骤：

+ 创建一个以.gitignore结尾的文件，在里面可以指定以什么结尾的文件可以使用通配符*

+ 在全局配置文件.gitconfig添加配置

  [core]   

  excludesfile=文件路径

+ 重新查看eclipse里面的文件带问号代表需要提交的，会发现navigator指定的文件上面已经没有标问号了，表示忽略了。



{%asset_img ignore01.jpg 忽略文件展示%}

附上：关于eclipse中符号的意义

> 表示项目已具备初始化为git仓库   -----> 会有黄色圆柱形的符号 <font color='red'>（提交后的文件也会变为这个符号）</font>

> 表示以及添加到暂存区   -----> 文件夹上面标有*   文件上面标有+号

> 表示新添加的文件    ------> 文件上面标有？号 <font color='red'> (此时需要添加到暂存区然后再提交)</font>

---

  ## 如何在eclipse中推送到远程库 

**与使用命令推送的方式我们只需要按照图形化的步骤去点击就行**

{%asset_img eclipse_push.jpg 推送到远程仓库%}

**在eclipse中我们也需要指定远程仓库的地址，用户名以及密码**

{%asset_img eclipse_push02.jpg 推送到远程仓库%}

**<font color='red'>都是以/refs/heads/分支名称</font>**

{%asset_img select_branch01.jpg 选择分支%}

**点击finish完成推送**



## eclipse克隆远程库下载项目

**虽然不同版本的eclipse克隆操作不一样但是大同小异这里我们演示的是eclipse2019版本的**

**<font color='cornflowerblue'>import项目选址Git导入的方式</font>**

{%asset_img import_project.jpg  克隆项目%}

{%asset_img clone.jpg  克隆项目%}

{%asset_img clone02.jpg  克隆项目%}

{%asset_img clone_branch.jpg  选择分支%}

{%asset_img select_dir.jpg  选择工作目录%}

{%asset_img cloning.jpg  克隆进行%}

> <font color='red'>实际上这个时候那个项目已经被下载到我们工作区了但是我们需要选择导入方式<br>这里必须解释一下，选择一个已经存在的项目，虽然我们项目已经有了但是在上面推送的时候我们已经把一些eclipse配置相关的目录给忽略了所以eclipse是根本不能识别到这个项目的<br/>我们如果使用选择一个新项目导入那么我们下载的项目就会被当成子目录存在这个新项目中没办法用<br/>所以选择一般项目那么eclipse会把它当成一个普通的java项目导入这个时候我们可以看到整体的项目结构但是eclipse并没有识别是什么项目</font>

**所以我们选择第三项作为一般项目导入**

{%asset_img import_new.jpg  项目导入%}

{%asset_img import_success.jpg 成功导入%}



> **重新识别项目**

{%asset_img convert_project.jpg 重新识别项目%}

{%asset_img convert_project02.jpg 重新识别项目%}

{%asset_img convert_maven.jpg 重新识别项目%}

---

## eclipse中解决代码冲突

**如果在不同的本地库中我们进行修改而且修改的是同一行那么就会出现<font color='red'>冲突</font>**

{%asset_img eclipse_conflict.jpg 操作两个本地库%}

**演示冲突如下：**

{%asset_img eclipse_conflict02.jpg 修改同一行代码%}

**修改同一行代码之后分别进行提交：<font color='red'>注意在提交的时候我们和SVN update一样需要更新一下需要执行pull操作拉取并合并</font>**

> **<font color='cornflowerblue'>因为我们push时必定是有一方是先push的所以在pull拉取合并的时候就不会出现问题，但是当第二个人操作了同一行进行了push操作那么就会出现冲突<font color='red'>（在git中如果发现版本不一样时当你进行push操作时会遭到拒绝我们必须先pull）</font></font>**

{%asset_img eclipse_conflict03.jpg 发生冲突%}

**解决冲突：（eclipse中有一个merge可以用来对比冲突{%asset_img merge_conflict.jpg 合并操作%}）**

+ 修改eclipse中代码到满意程度去掉git添加的区分符号
+ 添加到暂存区
+ 提交到本地库并且push到远程仓库

<font color='cornflowerblue'>按照之前命令操作的步骤一步步操作最后检查远程仓库：</font>

{%asset_img resolve_conflict.jpg 解决冲突%}

---

## eclipse中的分支操作

​    **分支之前的操作是互不影响的，当我们开发一个程序时如果想要去开发一个新功能但是又不确定开发的可行性那么我们可以新建一个分支去开发这个功能，当功能开发完成之后进行分支合并即可**

{%asset_img newBranch.jpg 新建分支%}

{%asset_img changeBranch.jpg 切换分支%}



**进行代码添加：**

{%asset_img editBranch.jpg 修改分支代码%}

**然后在添加暂存区提交本地库最后push到远程仓库中**

{%asset_img newBranch02.jpg 推送远程库并会自动创建新分支%}

{%asset_img newBranch03.jpg 查看分支代码%}



**其他的本地库进行代码拉取：**

**<font color='red'>注意当我们clone时如果不勾选全部分支那么当pull时就不会检测出新分支{%asset_img select_branch.jpg 选择所有分支%}</font>**

{%asset_img pull_branch.jpg  拉取合并检测新分支%}

{%asset_img change_branch.jpg 切换分支%}



**<font color='orange'>切换新分支之后检测内容无误那么切换到主分支进行分支合并最后推送到远程仓库完成分支操作</font>**

{%asset_img merge_branch.jpg 切换分支%}

{%asset_img push_master.jpg 推送到远程仓库%}

{%asset_img merger_push.jpg 合并推送完毕%}

## Gitlab的安装与部署就省略了

   **<font color='cornflowerblue'>  这里介绍一下Gitlab是一个代码托管的服务器，大致操作和github差不多，所以我们在实际工作中在局域网中搭建一个自己的代码托管中心可以避免在网络不好的情况下导致开发的障碍</font>**

