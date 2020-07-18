---
title: 目标跟踪之meanshift/camshift
date: 2017-08-19 21:52:08
categories:
- 技术
tags:
- 图像处理
- 目标跟踪
- OpenCV
comments: true
---

# 目标跟踪--meanshift和CamShift
之前讲过最简单的目标跟踪方法：模板匹配（其实是目标检测范畴），这个方法有很多局限。因此又产生了很多其他的目标跟踪方法，例如应用广泛的meanshift。 
meanshift是一种在数据密度分布中寻找局部极值的稳定的方法，对数据的密度直方图引用爬山算法即可。1998年的时候，人们开始把这种数据寻优的算法应用于图像的目标跟踪领域。
OpenCV也实现了这个算法，大致过程就是：
1. 选择跟踪的区域
2. 计算跟踪区域直方图的概率分布(这里可以选择颜色+纹理的特征进行反向投影)
3. 计算新一帧图像的概率分布
4. 运用meanshift算法计算目标新中心

## 跟踪效果
<!--more-->
## 晦涩的原理
虽然meanshift算法已经算目标跟踪领域简单级别的算法了，但是其数学描述和推导过程依然十分艰难。
MeanShift算法正是属于核密度估计法，它不需要任何先验知识而完全依靠特征空间中样本点的计算其密度函数值。
对于一组采样数据，直方图法通常把数据的值域分成若干相等的区间，数据按区间分成若干组，每组数据的个数与总参数个数的比率就是每个单元的概率值；
核密度估计法的原理相似于直方图法，只是多了一个用于平滑数据的核函数。
采用核函数估计法，在采样充分的情况下，能够渐进地收敛于任意的密度函数，即可以对服从任何分布的数据进行密度估计。

## OpenCV实现及源码分享

要实现OpenCV中使用meanShift目标跟踪，需要几个常用的函数，下面一一介绍
+ inRange()函数
inRange()函数可实现二值化功能（这点类似threshold()函数），更关键的是可以同时针对多通道进行操作，使用起来非常方便！
```
C++: void inRange(InputArray src, InputArray lowerb, InputArray upperb, OutputArray dst)
Parameters
src：输入数据
lowerb：下界
upperb：上界  在上下界之间的为1，否则为0
dst：输出数据，大小和输入数据一样而且是单通道
```
+ mixChannels()函数
mixChannels()主要就是把输入的矩阵（或矩阵数组）的某些通道拆分复制给对应的输出矩阵（或矩阵数组）的某些通道中，其中的对应关系就由fromTo参数制定.
```
C++: void mixChannels(const Mat* src, size_t nsrcs, Mat* dst, size_t ndsts, const int* fromTo, size_t
npairs)
参数解释：
src：输入数据，可以多个但是大小深度需要一样
nsrcs：输入数据个数
dst：输出数据
ndsts：输出数据个数
fromTo：通道索引对列表，从哪里复制到哪里 如果这个不是很明白，就看代码一下就懂
npairs： 通道索引对对数
这个函数也提供了一个打乱图片通道顺序的机制，懂！！！
```

+ calcHist()函数
calcHist()函数主要用于计算图像直方图。
```
void calcHist(const Mat* arrays, int narrays, const int* channels, InputArray mask, OutputArray
hist, int dims, const int* histSize, const float** ranges, bool uniform=true, bool accumulate=
false )；
参数解释：
arrays:输入的图像的指针，可以是多幅图像，所有的图像必须有同样的深度（CV_8U or CV_32F）。同时一副图像可以有多个channes。
narrays:输入的图像的个数。
channels:用来计算直方图的channes的数组。比如输入是2副图像，第一副图像有0，1，2共三个channel，第二幅图像只有0一个channel，
那么输入就一共有4个channes，如果int channels[3] = {3, 2, 0}，那么就表示是使用第二副图像的第一个通道和第一副图像的第2和第0个通道来计
算直方图。
mask:掩码,如果mask不为空，那么它必须是一个8位（CV_8U）的数组，并且它的大小的和arrays[i]的大小相同，值为1的点将用来计算
直方图。
hist:计算出来的直方图
dims:计算出来的直方图的维数。
histSize:在每一维上直方图的个数。简单把直方图看作一个一个的竖条的话，就是每一维上竖条的个数。
ranges:用来进行统计的范围。比如
float rang1[] = {0, 20};
float rang2[] = {30, 40};
const float *rangs[] = {rang1, rang2};那么就是对0，20和30，40范围的值进行统计。
uniform。每一个竖条的宽度是否相等。
```

	
+ calcBackProject()函数
calcBackProject()函数主要用于根据直方图计算图像的反向投影图。
```
C++: void calcBackProject(const Mat* images, int nimages, const int* channels, const SparseMat& hist,
OutputArray backProject, const float** ranges, double scale=1, bool uniform=true )
参数解释：
images：输入源图像（可以是多张），但是需要要同深度同尺寸，同类型 CV_8U or CV_32F ，可以有任意通道
nimages：源图像数量
channels：需要反向投影的通道列表。通道数必须和直方图维度相匹配，通道数第一个图片指定通道时范围0~images[0].channels()-1，第二张图片通道范围images[0].channels()~images[0].channels() + images[1].channels()-1
hist：输入直方图
backProject：输出反向投影图，单通道，深度大小和原图一样
ranges：每个维度直方图bin的范围
scale：缩放因子
uniform：直方图是否归一化的标志位
```

+ meanshift()函数
meanShift()函数利用meanshift算法根据概率密度计算局部极值，因此多应用与图像分割（但是用的函数不一样）、目标跟踪等。
```
C++: int meanShift(InputArray probImage, Rect& window, TermCriteria criteria)
参数解释：
probImage：根据目标直方图得到的反向投影图，具体见calcBackProject() .
window：初始搜索窗口， 这个变量也是输出窗口
criteria：停止迭代的准则
返回收敛时迭代次数(Returns Number of iterations CAMSHIFT took to converge).
```
+ CamShift()函数
CamShift()函数主要用于目标跟踪，使用的方法还是meanshift算法。这个就是调用的meanShift计算目标局部极值点
```
C++: RotatedRect CamShift(InputArray probImage, Rect& window, TermCriteria criteria)
参数解释：
同meanShif()
返回包含目标位置的旋转矩阵。
```
使用CamShift(meanShift)进行目标跟踪需要的重要的函数都大抵介绍完毕。

### 具体实现步骤
这里我们是选择颜色作为我们跟踪的特征，所以我们提取的是目标HSV颜色空间H通道(色度)的数据。
1. 使用鼠标选取需要跟踪区域
2. 提取跟踪区域HSV空间H通道的颜色直方图
3. 根据得到的颜色直方图对图像进行反向投影，相当于得到概率密度图（这个懂吧）
4. 有了概率密度图，利用meanshift算法迭代找到局部极值，更新跟踪区域
5. 重复3

### 代码
[我的github](https://github.com/liangzelang/)
