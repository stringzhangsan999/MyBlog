---
title: JavaSE面试题
date: 2020-09-11 13:53:11
tags: 关于collection问题
categories: JAVASE面试题
description: 讲解关于list、set、Map
---

# 关于javase中cllection面试会问到的问题

## 面试题1：

### 关于collection里面有哪些 接口，接口之间的区别以及实现类之间的区别？



**答：**{%asset_img outline.jpg  关于cllection图示%}

**List与Set 的区别:**

> 1.list可以允许存入重复的对象，而Set不允许存入重复对象

> 2.list可以插入多个null值，而set只允许插入一个null值

> 3.list是一个有序的集合存入的顺序和取出的顺序一致，而set是一个无序的集合不能保证顺序但是可以自定义排序规则 例如TreeSet通过Comparator 或者Comparable自定义排序规则

**ArrayList与LinkedList区别**

> 1.  每个 `ArrayList`实例都有一个*容量*。该容量是指用来存储列表元素的数组的大小。它总是至少等于列表的大小。随着向  ArrayList 中不断添加元素，其容量也自动增长。
>
> 