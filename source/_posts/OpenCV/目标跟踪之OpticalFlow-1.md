---
title: 目标跟踪之OpticalFlow
date: 2017-08-19 21:52:43
categories:
- 技术
tags:
- 图像处理
- 目标跟踪
- OpenCV
comments: true
---

## OpticalFlow 光流法
在视频序列中，把视频帧中每个像素与速度相关联，或者与同一像素点两帧之间的位移相关联，那么我们就可以得到稠密光流。[Horm-Schunck方法]()是计算光流的速度场。OpenCV是计算两帧之间对像素领域匹配，也叫块匹配；两种方法都是稠密跟踪。但是计算量都不小，所以不重点介绍。

## Lucas—Kanada光流法
相比上面的稠密光流，伟大的前人搞出了稀疏光流法，就是找出一些合适跟踪的特征点来进行跟踪匹配，即减少了运算量，效果还不差。稀疏光流法中当属LK光流法最有名。

### LK算法原理
<!--more-->
三个假设：
1. 亮度恒定，跟踪目标像素亮度在帧间不发生变化；
2. 时间连续或者微小运动
3. 空间一致， 这样同一空间的相邻点具有相似运动；
基于这三个假设就可以解出像素的速度场(在短时间内等于位移估计？？？这个点我不确定)，这样就可以得到下一帧的运动估计了。
然而实际运用中，采用的低帧率摄像机，而大而不连贯的运动普遍存在，如果要实现跟踪就要求大的跟踪窗口，就不满足第二假设。图像金字塔正好解决这个问题，从图像金字塔最高层开始向最底层跟踪。

## OpenCV实现
+ goodFeaturesToTrack()函数
goodFeaturesToTrack()函数主要是寻找Shi-Tomasi特征点
```
C++: void goodFeaturesToTrack(InputArray image, OutputArray corners, int maxCorners, double qualityLevel, double minDistance, InputArray mask=noArray(), int blockSize=3, bool useHarrisDetector=false, double k=0.04 )
参数解释：
image：输入单通道图像
corners：输出检测到的Shi-Tomasi角点
maxCorners：最多角点数量
qualityLevel：图像角点质量
minDistance：角点最小间距 
mask：ROI模板 
blockSize：邻域大小
useHarrisDetector：是否用Harris角点
```
+ calcOpticalFlowPyrLK()函数
calcOpticalFlowPyrLK()函数是计算金字塔LK算法的主要函数，通过指定合理的特征点，计算得到下一帧中这些特征点的位置，从而完成跟踪。
```
C++: void calcOpticalFlowPyrLK(InputArray prevImg, InputArray nextImg, InputArray prevPts,
InputOutputArray nextPts, OutputArray status, OutputArray err,
Size winSize=Size(21,21), int maxLevel=3, TermCriteria criteria=TermCriteria(TermCriteria::COUNT+TermCriteria::EPS, 30, 0.01),
int flags=0, double minEigThreshold=1e-4 )
参数说明：
prevImg：第一张输入单通道图像或者图像金字塔
nextImg：第二张
prevPts：需要光流跟踪的点集输入，必须是单精度浮点型
nextPts：输出光流跟踪点集结果
status：输出状态，如果跟踪成功就置1，反之置0
err：输出跟踪误差，如果没有跟踪成功，则error没有定义，这种情况status可以看出
winSize：每级金字塔搜索窗口尺寸
maxLevel：设置金字塔层数 0：不使用金字塔 1： 2层金字塔
criteria: 迭代终止条件
flags：各种标志位
– OPTFLOW_USE_INITIAL_FLOW：
– OPTFLOW_LK_GET_MIN_EIGENVALS：
minEigThreshold：
```

```
// 程序说明： 从摄像头获取图像，根据光流法得到跟踪点的向量
#include <opencv2\opencv.hpp>
#include <iostream>

using namespace cv;
using namespace std;
Mat srcImage1, srcImage2;
Mat gray1, gray2;
vector<Point2f> preCorners;
vector<Point2f> nextCorners;
vector<uchar> trackStatus;
vector<float> trackError;
int maxCorners = 200;
double qualityLevel = 0.08;
double minDistance = 20;
Scalar colorGreen(0,255,0);   //green
Scalar colorRed(0,0,255);  //red
bool gotBB = false;
Point2f up,down;    //鼠标位置
vector<Point2f> initial;  //记录跟踪框中的角点
bool rectFlag = false;
bool downFlag = false; 
Rect rect;
Rect box;
Mat mouseImg;
static void onMouse( int event, int x, int y, int, void* )
{
	
	// CV_EVENT_MOUSEMOVE   CV_EVENT_LBUTTONDOWN    CV_EVENT_RBUTTONDOWN    CV_EVENT_MBUTTONDOWN
   // CV_EVENT_LBUTTONUP    CV_EVENT_RBUTTONUP     CV_EVENT_MBUTTONUP      CV_EVENT_LBUTTONDBLCLK
   // CV_EVENT_RBUTTONDBLCLK    CV_EVENT_MBUTTONDBLCLK 
	
	
	switch(event)
	{
	case CV_EVENT_LBUTTONDOWN:
		down = Point(x,y);	
		downFlag = true;
		break;
	case CV_EVENT_LBUTTONUP:
		up = Point(x,y);
		rect = Rect(down,up);
		downFlag = false;
		gotBB = true;		
		break;
	case CV_EVENT_MOUSEMOVE:
		if(downFlag)
		{
			rectangle(mouseImg, Rect(down,Point(x,y)), Scalar(0,0,255));
			imshow("src", mouseImg);
		}		
		break;
	default:
		break;
	}
}

int main()
{
	Mat frame;
	VideoCapture capture(0);
	capture >> frame;
	namedWindow("src");
	//setMouseCallback("src", onMouse, 0);
	while(0)
	{
		imshow("src", frame);
		capture >> frame;
		frame.copyTo(mouseImg);
		waitKey(10);
	}

	frame.copyTo(srcImage1);
	cvtColor(srcImage1, gray1, CV_RGB2GRAY);
	goodFeaturesToTrack(gray1, preCorners, maxCorners,  qualityLevel, minDistance);
	for(int i = 0; i < preCorners.size(); i++)
	{
		circle( srcImage1,preCorners[i], 3, colorRed, -1, 8);
	}
	
	while(!frame.empty())
	{
		capture >> frame;
		frame.copyTo(srcImage2);		
		cvtColor(srcImage2, gray2, CV_RGB2GRAY);		
		calcOpticalFlowPyrLK(gray1, gray2, preCorners, nextCorners,  trackStatus, trackError);
		int realNums = 0;  //真正成功跟踪的点数量
		for(int i = 0; i < nextCorners.size(); i++)
		{
			if(trackStatus[i] && ((abs(preCorners[i].x - nextCorners[i].x) + abs(preCorners[i].y - nextCorners[i].y)) > 2))
			{
				nextCorners[realNums++] = nextCorners[i];  //把成功跟踪的点提前  
				line(srcImage2,preCorners[i],nextCorners[i],colorGreen,1,8,0);
				circle( srcImage2,nextCorners[i], 3, colorRed, -1, 8);
			}
		}		
		imshow("src",srcImage2);
		nextCorners.resize(realNums);
		if(realNums <= 1)  //如果被成功跟踪的点不到10个，加入新的特征点		
		{
			vector<Point2f> reTrack;
			goodFeaturesToTrack(gray2, reTrack, 100,  0.05, minDistance); 		
			nextCorners.insert(nextCorners.end(), reTrack.begin(), reTrack.end());
		}
		swap(gray1, gray2);
		swap(preCorners, nextCorners);
		waitKey(10);
	}
	return 0;
}
```
