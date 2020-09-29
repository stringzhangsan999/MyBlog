---
title: JAVASE
date: 2020-09-29 17:57:14
tags: 使用java完成递归去除字符串中重复的内容
categories: JAVASE
description: Java基础
---

# 使用递归完成去除重复字符串

## 需求：

  **现在有两类字符串：**

   **1.外绝缘无裂纹、破损及放电现象，增爬伞裙粘接牢固、无变形，防污涂料完好、无脱落、起皮现象。金属法兰无裂痕，防水胶完好，连接螺栓无锈蚀、松动、脱落。A相外绝缘无裂纹、破损及放电现象，增爬伞裙粘接牢固、无变形，防污涂料完好、无脱落、起皮现象。金属法兰无裂痕，防水胶完好，连接螺栓无锈蚀、松动、脱落。B相外绝缘无裂纹、破损及放电现象，增爬伞裙粘接牢固、无变形，防污涂料完好、无脱落、起皮现象。金属法兰无裂痕，防水胶完好，连接螺栓无锈蚀、松动、脱落。C相**   

   **2.主变油温及油位。北侧油面温度表：\_\_\_℃；   主变油温及油位。北侧绕组温度表：\_\_\_℃；主变油温及油位。南侧油面温度表：\_\_\_℃；主变油温及油位。油位指示：\_\_\_。**

## 目标：

  **我们想要得到的结果是：**

   **1.外绝缘无裂纹、破损及放电现象，增爬伞裙粘接牢固、无变形，防污涂料完好、无脱落、起皮现象。金属法兰无裂痕，防水胶完好，连接螺栓无锈蚀、松动、脱落。**

​      **A相**

​      **B相** 

​      **C相**

   **2.主变油温及油位。**

​      **北侧油面温度表：\_\_\_℃；** 

​      **北侧绕组温度表：\_\_\_℃；**

​       **南侧油面温度表：\_\_\_℃；**

​       **油位指示：\_\_\_。**

## 思路：

   **1.我们不知道字符串内容的情况下如何去掉里面相同的内容** 

   **2.首先我们必须得到那个相同的内容但是如何得到。观察发现这两类字符串中有一个共同点就是相同内容结尾为句号“。”**

   **3.我门用句号截取相同内容，并且把这个内容用一个变量保存起来。截取的内容分为前后两部分 用前部分也就是相同部分去替换掉后部分相同的内容，然后将替换后的字符串保存起来，最后拼接前后部分的内容也就完成了去重**

   **4.我们看见第一类里面相同部分有两个句号，这个时候我们就想我们的逻辑会不会有问题！其实相同部分我们可以看成是两个重复的内容 。完全可以按照我们的逻辑去做。**

   **5.我们可以把前部分保存起来后半部分替换，然后后半部分发现还是有重复的内容我们再次执行我们截取替换的操作把前部分保存后部分替换最后合并。最后相当于我们有多个前部分我们只需要一个个合并就行了。**

**<font color='red'>所以我们需要使用递归</font>**



## 代码：

```java
/**
	 * 合并所有需要合并的字符
	 * @param mergeString
	 * @param content
	 * @return
	 */
	public String mergeString(String content) {	
	     //1.可以发现第一个句号后面都是重复类容
		//2.截取第一个句号前面的字符串
		int indexOf = content.indexOf("。");
		StringBuilder builder = new StringBuilder();
		String subPro = content.substring(0, indexOf+1);//左闭右开所以要+1
		String subFor = content.substring(indexOf+1);
		String mergeString =null;
		String replace =null;
		if(subFor.contains("\r\n"))
	    replace = subFor.replace(subPro, "");
		else	
	    replace = subFor.replace(subPro, "\r\n");
		
		if(replace.indexOf("。")!=-1&&replace.indexOf("。")!=replace.lastIndexOf("。")) { 
			mergeString = mergeString(replace);//递归去掉所有包含。且重复的内容
			//builder.append(subPro+"\r\n");
			builder.append(subPro+"");	
		}
		else {
			mergeString=replace;
			if(subPro.contains("\r\n"))
			builder.append(subPro+"");
			else
			builder.append(subPro+"\r\n");
		}
		builder.append(mergeString);
		return builder.toString();
		
	}
```



