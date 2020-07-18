---
title: 目标跟踪之TLD第一篇
date: 2017-08-19 21:53:10
categories:
- 技术
tags:
- 图像处理
- 目标跟踪
- OpenCV
- TLD
comments: true
---

# 目标跟踪之TLD（Tracking-Learning-Detection）--第一篇

之前总结了目标跟踪领域的一些传统方法，光流、meanshift、camshift等，这些大概都是2010之前比较流行的方法。都是比较纯粹的跟踪方法，有很多需要完善的东西，比如camshift虽然对跟踪目标的形变不敏感，但是如果在目标与背景颜色相差不大的情况，跟踪效果很差；传统方法在跟踪目标消失或者被完全遮挡后，再出现无法进行准确的重新跟踪（其实也可以，就是依靠相关的目标检测方法啦）。

今天要介绍的方法就是在2012年Kalal的TLD方法，主要是长时间跟踪的一种方法，把检测、跟踪、学习融合在一起。
OpenCV3.2.0已经加入了TLD算法，但是我目前的版本是2.4,所以今天这个TLD的实现是Github上面的大神实现的。稍后也会加上OpenCV上面的TLD效果。
<!--more-->
## 安装
如果要了解一个算法最好直观感性的看看效果嘛，在Github上有一堆TLD的实现，但是大都没有经过严格的测试。所以坑了我们这些小白。我用的是[alantrrs](https://github.com/alantrrs/OpenTLD)安装，在这里我主要介绍OpenCV3.0.0之后版本在编译TLD的步骤方法。

1、 安装OpenCV，具体步骤本文不详述。在这里[CSDN 宋洋鹏（youngpan1101）](http://blog.csdn.net/youngpan1101/article/details/58027049).我的OpenCV3.2.0默认安装在'/usr/local/'下面。

2、
```
git clone https://github.com/alantrrs/OpenTLD.git
cd OpenTLD
mkdir build
cd build
```
3、一般进行到这里没有问题，下面就是设置编译选项、编译了
```
cmake ../src/
make
```
这里可能会出现如下错误
+ PatchGenerator 在CV空间中不是一个类型名字，这是因为在OpenCV3以后就把legacy这个文件夹都删除了，而PatchGenerator的类定义和实现就在这个文件夹里面。
+ vector 没有定义什么的，这是因为在头文件中没有包含vector,这个错误好像被修复了
+ 好像就没有什么错误了，要是再有错误就是OpenCV没有装好，或者装的位置不在默认的'/usr/local'下面

**解决方法**
1. 重写PatchGenerator类
下面将给出类的定义与实现，只需要将OpenCV2中对应实现复制过来就可以。'.h'头文件加入OpenTLD的include文件夹，'.cpp'源文件加入OpenTLD的src文件夹。
+ 牢记在需要PatchGenerator类的地方加入'PatchGenerator类'头文件。【在TLD.h中添加即可】
+ 牢记必须在**CMakeList.txt**中合适位置添加编译选项
```
#libraries
add_library(PatchGenerator PatchGenerator.cpp)
...
#link the library
target_link_libraries(run_tld tld PatchGenerator ...)
```
其中 PatchGenerator.h 如下：
其中 PatchGenerator.cpp 如下：

2. 在需要的头文件中加入'#include <vector>' 【好像是在tld_utils.cpp中添加】
3. 找到安装的位置， 在OpenTLD目录'src'找到**CMakeList.txt**,在文件中指定OpenCV安装路径即可
```
#OpenCV
set(OpenCV your_path)
find_package(OpenCV REQUIRED)
```

至此应该所有问题都将迎刃而解……

如果上面这么多依然解决不了你的问题， 直接git clone 我个人修改过的repo[]()（前提你是用的OpenCV3，以及安装位置正确）

