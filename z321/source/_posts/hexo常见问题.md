---
title: hexo常见问题
date: 2020-08-18 10:37:56
tags: 博客
categories: 博客
---

在编写博客时总会遇到很多问题，例如默认评论是全部打开的等等...

# 博客编写遇到的问题

<!--more-->

## 首页会默认将所有内容展示

### 如何设置阅读全文的功能当点击阅读全文时再出现所有的内容

+ 方式一： 在文章中使用`< !--more-->` 手动进行截断这种方法可以根据文章的内容，自己在合适的位置添加 `< !--more-->` 标签，使用灵活，也是Hexo推荐的方法。 亲测十分有效



+ 方式二： 在文章中的`front-matter`中添加description，并提供文章摘录这种方式只会在首页列表中显示文章的摘要内容，进入文章详情后不会再显示。 



+ 方式三：查看了一篇关于这个设置全文的博客 但是没有亲测

  > 因为next新本升级以前老版本的配置有可能生效不了了所有需
  >
  > 看了一篇博客上说这个是需要下载插件的
  >
  > 第一步： 在博客目录 npm install hexo-excerpt --save 
  >
  > 第二步：配置站点配置文件 _config.yml

  ```yml
  excerpt:
   depth: 10
   excerpt_excludes: [ ]
   more_excludes: [ ]
   hideWholePostExcerpts: true
  ```




## 如何添加评论功能

> 关于hexo的评论功能分别是依赖其他的评论系统 例如：gitalk 依赖的是github的评论系统 valine依赖的就是valine这个评论系统我们需要先点击快速开始：会出现 {%asset_img valine_register01.jpg  valine点击注册%}



> 首先我们先注册登录一下然后创建一个属于自己的应用记得点击开发版，开发版是不需要收费的{%asset_img create_app01.jpg 创建应用%}
>
> 点击右上角的设置出现如下样式：
>
> {%asset_img app_setting01.jpg 应用设置%}
>
> 将 **AppID**  和 **AppKey**  分别复制到站点配置文件中（这个是一个全新的写法是将主题配置文件中的内容cpoy出来然后`theme_config:`配置在站点配置文件中这样方便与更新和维护）
>
> {%asset_img  comments_config01.jpg  配置站点配置文件%}

## 如何修改默认评论全部打开

{% asset_img comment.jpg comment%}

如何将这个功能只在特定的页面展示呢！

> 首先这个功能是默认打开的所以我们不用去管我们只需要在我们不想要的地方进行配置：
>
> instances: 分类 关于 还有标签等地方我们不希望出现评论模块我们只需要在文章开头加上comments : false 这样就会关闭

{% asset_img comment_false.jpg comment%}



## 如何在博客中插入图片能正常显示

desc： 因为在写博客并且上传到云端时会出现想要在文字旁边添加图片的时候，但是发现普通的markdown语法写出的`![]()`

不管是在里面填写相对路径还是绝对路径都不管用那么我们应该如何解决这一问题



* 第一步`首先我们需要下载一个图片处理的插件在npm中我们没有去处理图片的插件所以不能去显示图片在博客目录下输入：npm install hexo-asset-image --save安装好这个插件之后我们就可以使用插件的语法：{% asset_img 图片名称 图片描述 %}`  [点击前往github]( https://github.com/xcodebuild/hexo-asset-image )
* 第二步我们修改全局配置文件_config.yml `post_asset_folder: true`默认为false
* 第三步 当我 使用hexo n titile 时就会在出现同名的文件夹  {% asset_img newFrold.jpg newFrold%}
* 第四步重新 hexo g  、hexo  s 测试效果 没有问题之后再hexo d部署到云端

---

## 新建文章时如何格式化建文档

```nodejs
hexo new [layout] 文章名称   #新建文章格式
```



  **当我们执行了这个代码之后会在博客目录的source目录下生成.md文件，这个代表着文章是识别MarkDown语法所以我们可以使用Typora编辑器来编写博客**

​    **<font color='cornflowerblue'>布局（layout）:<br/> Hexo 有三种默认布局：`post`、`page` 和 `draft`。在创建者三种不同类型的文件时，它们将会被保存到不同的路径；而您自定义的其他布局和 `post` 相同，都将储存到 `source/_posts` 文件夹。 </font>**

​    **<font color='cornflowerblue'>文件名称：<br/> Hexo 默认以标题做为文件名称，但您可编辑 `new_post_name` 参数来改变默认的文件名称，举例来说，设为 `:year-:month-:day-:title.md` 可让您更方便的通过日期来管理文章。 </font>**

|   变量    |                描述                 |
| :-------: | :---------------------------------: |
|  ：title  | 标题（小写，空格将会被替换为短杠）  |
|  ：year   |      建立的年份，比如， `2020`      |
|  ：month  | 建立的月份（有前导零），比如， `04` |
| ：i_month | 建立的月份（无前导零），比如， `4`  |
|   :day    | 建立的日期（有前导零），比如， `07` |
|  :i_day   | 建立的日期（无前导零），比如， `7`  |





