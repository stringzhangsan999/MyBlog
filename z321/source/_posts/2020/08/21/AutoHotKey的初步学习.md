---
title: AutoHotKey的初步学习
date: 2020-08-21 17:33:02
tags: 快捷键脚本工具
categories: AutoHotKey快捷键脚本工具
description: 一个可以快速工作在Windows下使用快捷键脚本快速进行操作的语言
---



 # AutoHotKey使用方法

[文档参照]( http://ahkcn.sourceforge.net/docs )

## AutoHotKey简介

​    **是一款免费的、Windows平台下开放源代码的热键脚本语言，是为游戏操纵杆和鼠标创建的热键，是自动按键。也可以通过命令调用系统接口及程序，并创建基于简单语言的图形化界面的执行程序**

---

## 变量使用

### 变量基本介绍

> **1.变量类型：在这个脚本语言中没有明确的变量类型，只包含数字的变量可以进行数学运算和比较**

> **2.变量的作用域和声明: 除了函数中的 局部变量, 其他所有变量都是全局的 ** 

>  **3.变量的名称: 变量名不区分大小写 (例如, *CurrentDate* 等同于 *currentdate*). 变量名可以含有多达 253 个字符, 并且可以由字母, 数字以及后面的标点组成: # _ @ $ ** 

### 变量赋值

#### 给变量赋值有两种方式： 传统方法和表达式方法 

+ **使用“=”为变量赋值**

     ```autohotkey
MyNumber = 123
MyString = This is a literal string.
CopyOfVar = %Var%  ; 和 = 运算符一起使用时, 需要使用百分号来获取变量的内容. 
     ```

**<font color='red'>注意：“；”分号为代码中的注释符号，其中如果如果以这种方式一个变量想要引用另一个变量那么需要加上%%把另一个变量包起来</font>**

+ **使用“:=”为变量赋值**

```autohotkey
MyNumber := 123
MyString := "This is a literal string."
CopyOfVar := Var  ; 和前面段落中与其作用相同的语句不同, 百分号不和 := 运算符一起使用
```

**<font color='red'>注意：这种方式，字符串需要加上引号，百分号不和 := 运算符一起使用</font>**

 **后一种方法由于其更清晰并且与其他许多语言几乎一致的 表达式语法 成为大多数人的首选方法.** 

**清空变量的两种方式：**

```autohotkey
MyVar =
MyVar := ""
```



#### 获取变量的两种方式： 传统方法和表达式方法 

**传统方法需要将变量名包围在百分号中来获取变量的内容. 例如：**

```autohotkey
MsgBox The value in the variable named Var is %Var%.
CopyOfVar = %Var%
```

​    **<font color='red'>注意： 在上面的 MsgBox 这行, 通过使用百分号和空格把参数从传统模式改变为表达式模式. 因为所有的命令默认情况下使用传统模式 (除了另外注明的那些)</font>** 

​    **表达式方法省去了变量名两边的百分号, 但原义的字符串必须包围在双引号中. 所以, 下面的表达式作用等同于上面的例子: **

```autohotkey
MsgBox % "The value in the variable named Var is " . Var . "."  ; 使用句点连接两个字符串.
CopyOfVar := Var
```



### 表达式：

**表达式用来对一系列变量, 原义字符串和/或原义数字执行一个或多个操作**

**关于表达式内容比较多而且与一般的编程语言的内容是大同小异我们只需要注意几个重要的点：**

> **1.表达式中支持 or and 这样 的表达**

> **2.含有表达式的if和传统的if可以通过开括号“(”区分**

​    **<font color='red'>     尽管通常把整个表达式包围在括号中，不过也可以写成这样：`if (x > 0) and (y > 0)`。此外, 如果单词 "if" 后的第一项为 函数调用或类似 "not" 或 "!" 这样的运算符时, 开括号可以完全省略. </font>**

> 3.**关于布尔值**:要计算表达式结果为真还是假时 (例如 IF 语句), 表达式结果为空或零被视为假, 而其他所有结果都视为真. 

> 4.**整数和浮点数**: 在表达式中, 含有小数点的数字被视为浮点数; 否则视为整数. 对于大多数运算符（例如加法和乘法），只要其中的一个输入是浮点数，那么结果也将是浮点数。 

[表达式运算符的优先级点击查看]( http://ahkcn.sourceforge.net/docs/Variables.htm#BuiltIn )

---

## 函数

**<font color='green'>函数就是执行一堆命令返回或者不返回的封装命令的方法</font>**

> 表达格式：方法名（参数）{一堆命令.....[return  期望值]}

```autohotkey
Add(x, y)
{
    return x + y   ; "Return" 期望 表达式.
}
```

### 函数中的参数

这里的参数分为：

+ **常规参数** ：常规参数就像上面的参数一样直接传递即可

+ **可选参数**：可选参数通俗来说就是给参数设置一个默认值如果调用时候没有传递这个参数那么就会使用可选参数

```autohotkey
Add(x, y, z:=0) ; "="和":="效果都一样但是推荐使用":=""
{
    return x + y + z   
}
```



+ **可变参数**:也就是可以传递多个参数参数个数不限制，这个可变参数和java中可变参数不一样, 在最后一个参数后面写一个星号来标记此函数为可变参数的, 这样让它可以接收可变数目的参数: 

```autohotkey
Join(sep, params*) {
    for index,param in params
        str .= param . sep
    return SubStr(str, 1, -StrLen(sep))
}
MsgBox % Join("`n", "one", "two", "three")
```

​    **调用可变参数函数时, 通过保存在函数的最后参数中的对象可以访问剩余的参数. 函数的首个剩余参数在 `params[1]`, 第二个在 `params[2]` 等等. 和所有的标准对象一样, 使用 `params.MaxIndex()` 可以确定最大的索引值 (这里为参数的数目). 但是如果没有参数, MaxIndex 会返回空字符串.** 