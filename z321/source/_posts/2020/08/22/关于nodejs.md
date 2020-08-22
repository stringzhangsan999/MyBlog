---
title: 关于nodejs
date: 2020-08-22 14:29:58
tags: npm下载依赖
categories: nodejs
description: 关于nodejs记一笔
---

# NodeJs记一笔

## 初始化

```nodejs
npm init #初始化一个项目
```

当执行了初始化时会需要填写一些信息和选项我们大部分都可以不填，初始化完之后在项目目录下会生成一个package.json的文件

## 安装下载依赖

```nodejs
npm install #下载相关依赖
```



**我们常常执行的npm install 其实和Maven中下载包是一个意思**

当执行了npm install 之后它会立马去查看配置package.json

{%asset_img nodejs01.jpg 下载相关包%}

**根据里面的名称版本号去下载指定的版本的包**

