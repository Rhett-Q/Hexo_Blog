---
title: Markdown初识
date: 2018-03-07 19:33:03
tags: Markdown
---
最近用`Hexo`快速的搭建了自己的博客，希望以此记录下自己的学习记录。  
`Hexo`使用`Markdown`解析文章,一种轻量级的标记语言,它可以在几秒内，利用绚丽的主题生成静态网页。
## 用法
### 标题
	# 一级标题
	## 二级标题
	### 三级标题
	#### 四级标题
	##### 五级标题
	###### 六级标题
总共有六级标题，标题与#号之间要有空格
#### 效果
![标题效果图](https://i.imgur.com/q5Qanq7.png)
### 换行

在文本中的换行会从最生成的结果中删除，浏览器会根据可用空间自动换行，如果想强迫换行，可以再行末插入至少两个空格### 列表
	* 无序列表1
	* 无序列表2
	* 无序列表3
	1. 有序列表1
	2. 有序列表2
	3. 有序列表3
文本前的星号可以用加号或者减号代替
#### 效果
![列表效果图](https://i.imgur.com/3T0bR9X.png)
### 链接
	[a test] (http://www.test.com)
	[a test] (http://www.tesy.com "title")
[] ()之间无需空格,title为鼠标覆盖时产生的链接标题
#### 效果
![链接效果图](https://i.imgur.com/S7Cv2Fd.png)
### 块引用
	> 第一层
	> > 第二层
	> > > 第三层
#### 效果
![块引用效果图](https://i.imgur.com/Olzsvs3.png)
### 代码块
	制表符或者四个空格#include<stdio.h>
	制表符或者四个空格int main() {
	制表符或者四个空格	   printf("Hello Markdown");
	制表符或者四个空格}
#### 效果
![代码块效果图](https://i.imgur.com/ITS9dub.png)
## 结语
初步学习了Markdown的用法，以后再深入学习巩固
> 本文标题：Markdown初识  
> 发布时间：2018年3月7日  
> 更新时间：2018年3月8日
> 许可协议：[署名-非商用-相同方式共享4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)
