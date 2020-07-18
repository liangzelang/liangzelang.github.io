---
title: Markdown用法总结
date: 2017-06-14 23:12:17
categories:
- 技术
tags:
- Markdown
comments: true
---

+ Markdown是时下程序员圈子最火的文档格式，它的宗旨就是<b>易写易读</b>
+ 目前很多论坛、博客都支持Markdown，Github、Stack overflow等程序猿熟知的社区都支持Markdown。Markdown也兼容HTML，所以前端对这个几乎没有学习成本。而且我本人博客也是使用的Markdown格式，因此有必要把一些常用的Markdown语法做一个简要的介绍，技多不压身嘛。

+ 以下介绍常用的Markdown语法
+ [<b>标题</b>](#1)
+ [<b>字体</b>](#2)
+ [<b>代码与代码块</b>](#3)
+ [<b>分割线</b>](#4)
+ [<b>列表</b>](#5)
+ [<b>图片&链接</b>](#6)
+ [<b>视频</b>](#7)

<!--more-->

<h3 id='1'>标题</h3> 
```
标题格式： #(个数自己定)+空格
# 一级标题
## 二级标题
### 三级标题
```
+ 效果
# 一级标题
## 二级标题
### 三级标题



<h3 id='2'>字体</h3> 
字体不是什么宋体、楷体，是粗体和斜体……一些情况下需要区分，这个就很好用。
```
粗体格式： **+内容+**
**我好想找个女朋友**
```
+ 效果
**我好想找个女朋友**
```
斜体格式： *+内容+*
*女朋友你在哪里*
```
+ 效果
*女朋友你在哪里*

<h3 id='3'>代码与代码块</h3> 
程序猿接触最多的就是代码了，所以这个部分必然是要学会的，
```
代码格式： ```+内容+```  或者 `+内容+`
\```
#include <iostream>
using namespace std;
\```
我们测试一个`嵌入式代码`
```
+ 效果
```
#include <iostream>
using namespace std;
```
`嵌入式代码`

<h3 id='4'>分割线</h3> 

时常需要搞一个醒目的分割线以示尊重。。。
```
分割线格式： *** 或 --- 或 ___  必须单独一行
就是三个以上的星号、减号、底线就OK，也可以在之间加空格
我
***
---
___
* * *
- - -
_ _ _
女朋友
```
+ 效果
# 我
***
# 我
---
# 我
___
# 我
* * *
# 我
- - -
# 我
_ _ _
# 女朋友





<h3 id='5'>列表</h3> 

写文章阐述多个观点时肯定需要列表符号，列表有无序列表和有序列表：
```
无序列表格式： *+空格
* 你
* 我
* 她
```
+ 效果
* 你
* 我
* 她
```
有序列表格式： 数字+点+空格
1. 白菜
2. 青菜
3. 韭菜
```
+ 效果
1. 白菜
2. 青菜
3. 韭菜


<h3 id='6'>图片&链接</h3> 

在文章中插入图片和链接很常见，也使得文本不那么单调嘛。
```
插入图片格式： ![pic_title](pic_link)
![deep learning](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1498051527&di=502ad10591850ae9ae1211d0ed3cd587&imgtype=jpg&er=1&src=http%3A%2F%2Fs11.sinaimg.cn%2Fmw690%2F001tzeulgy6HMd3vpqida%26amp%3B690)
```
+ 效果
![deep learning](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1498051527&di=502ad10591850ae9ae1211d0ed3cd587&imgtype=jpg&er=1&src=http%3A%2F%2Fs11.sinaimg.cn%2Fmw690%2F001tzeulgy6HMd3vpqida%26amp%3B690)
```
插入链接格式：[link_title](link)
[newbird](newbird.cn)
```
+ 效果
[newbird](newbird.cn)


<h3 id='7'>视频</h3> 
这里插入视频就是直接一个视频播放框，不是链接！！！这个时代不插入个视频都不好意思演示自己的demo。当然Markdwn本身是没有办法插入的，前面也说了兼容HTML，所以用HTML的语句
```
插入视频格式：使用iframe，指定视频框长宽，源地址等就OK了。 
<iframe 
		height=498 
		width=510 
		src="http://player.youku.com/embed/XNjcyMDU4Njg0" 
		frameborder=0 
		allowfullscreen>
</iframe>
```
+ 效果
<div align=center>
<iframe 
		height=498 
		width=510 
		src="http://player.youku.com/embed/XNjcyMDU4Njg0" 
		frameborder=0 
		allowfullscreen>
</iframe>
</div>


## 引用
有时需要引用其他地方的东西，如果不指出也可以，但是指出的话看起来更加有逻辑，处女座的必须要学起来呀……
```
引用格式： >+空格
> 世界是我们的，也是你们的，但终究是你们的。
```
+ 效果
> 世界是我们的，也是你们的，但终究是你们的。


差不多就是这些了…………
