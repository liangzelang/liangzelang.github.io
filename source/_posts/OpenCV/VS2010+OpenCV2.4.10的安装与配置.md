---
title: VS2010+OpenCV2.4.10的安装与配置
date: 2016-09-25 06:56:32
tag: 
 - OpenCV基础
 - VS2010
categories: 
 - 技术 
---
由于本人最近要做图像处理的一些工作，因此将相应的流程记录备份，方便自己，也为他人提供一个思路吧。<br/>
平台操作系统： Win7 32位 旗舰版<br/>
## 1、VS2010的安装
直接点击 AutoRun.exe或者Setup.exe按照提示一步步安装即可。需要选择安装那些模块，按自己的开发需要选择，如果实在不懂，全部安装。<br/>
## 2、OpenCV的安装
我用的是OpenCV2.4.10，安装非常简单。只要选择安装位置就可以自动完成，如图1。需要记住该安装地址（本文假设安装在D盘根目录下 D:\）。
![图1](http://oapeb119y.bkt.clouddn.com/1.png)
![图2](http://oapeb119y.bkt.clouddn.com/2.png)
## 3、OpenCV的配置
<!--more-->
### 3.1、系统环境变量的配置
OpenCV其实就是开源视觉库，要让开发环境找到并使用这个库，需要将OpenCV的路径添加到系统的PATH环境变量。具体方法：<b>我的电脑——>属性——>高级——>环境变量——PATH变量<b><br/>
编辑PATH环境变量，在最后加入 Opencv的路径(本文假设安装在D:\下面，需要根据自己安装路径更改)
```
;D:\opencv\build\x86\vc10\bin
```
<b>不要遗忘分号 ；<b/>
## 3.2、VS2010工程的相关配置
工程要使用OpenCV库，就需要在工程中把库的相关路径包含进去。
有两种方法，一种是在某个工程下包含相应路径，但是这样每建一个目录就需要重新包含一次，非常繁琐。<b>本文采用第二种方法,包含一次，以后建立工程就不用反复添加<b/>：
* 新建项目：<b>文件-->新建-->项目-->选择Win32控制台程序-->确定<b/>（本文项目名叫做test，后文要用）
![图3](http://oapeb119y.bkt.clouddn.com/3.png)
![图4](http://oapeb119y.bkt.clouddn.com/4.png)
* 包含目录和库目录配置: 在新建的工程页面 <b>菜单栏-->视图-->属性管理器-->Debug|Microsoft.cpp.win32.user(右键属性)，弹出属性页-->VC++目录<b/>，分别修改包含目录和库目录。
![图5](http://oapeb119y.bkt.clouddn.com/5.png)
![图6](http://oapeb119y.bkt.clouddn.com/6.png)
![图7](http://oapeb119y.bkt.clouddn.com/7.png)
![图8](http://oapeb119y.bkt.clouddn.com/8.png)
* 包含目录配置<>
添加
```
D:\opencv\build\include
D:\opencv\build\include\opencv
D:\opencv\build\include\opencv2 
```
这三个路径，如图九所示。
注意，根据自己不同的路径修改。
![图9](http://oapeb119y.bkt.clouddn.com/9.png)
* <b>库目录配置
添加
```
D:\opencv\build\x86\vc10\lib
```
这个路径。
* 链接器配置
在<b>属性页-->链接器-->输入-->附加依赖项<b>添加
```
opencv_calib3d2410.lib
opencv_contrib2410.lib
opencv_core2410.lib
opencv_features2d2410.lib
opencv_flann2410.lib
opencv_gpu2410.lib
opencv_highgui2410.lib
opencv_imgproc2410.lib
opencv_legacy2410.lib
opencv_ml2410.lib
opencv_nonfree2410.lib
opencv_objdetect2410.lib
opencv_ocl2410.lib
opencv_photo2410.lib
opencv_stitching2410.lib
opencv_superres2410.lib
opencv_ts2410.lib
opencv_video2410.lib
opencv_videostab2410.lib
opencv_calib3d2410d.lib
opencv_contrib2410d.lib
opencv_core2410d.lib
opencv_features2d2410d.lib
opencv_flann2410d.lib
opencv_gpu2410d.lib
opencv_highgui2410d.lib
opencv_imgproc2410d.lib
opencv_legacy2410d.lib
opencv_ml2410d.lib
opencv_nonfree2410d.lib
opencv_objdetect2410d.lib
opencv_ocl2410d.lib
opencv_photo2410d.lib
opencv_stitching2410d.lib
opencv_superres2410d.lib
opencv_ts2410d.lib
opencv_video2410d.lib
opencv_videostab2410d.lib
```
即可。至此所有配置都完成。
![图10](http://oapeb119y.bkt.clouddn.com/10.png)
## 测试安装和配置是否成功
在之前创建test项目，打开test.cpp,编写如下测试程序：

```
/***********************************************************************
* OpenCV 2.4.10 测试例程
* 梁泽浪提供
***********************************************************************/
#include"stdafx.h"
#include <opencv2/opencv.hpp>
using namespace std;

using namespace cv;

int main(int argc, char*argv[])
{
	IplImage* pImg;  

    if ((pImg = cvLoadImage("C:\\Users\\liangzelang\\Desktop\\lena.jpg", 1)) != 0)  
    {  
        cvNamedWindow ("Image", 1);  
        cvMoveWindow ("Image", 0, 0);  
        cvShowImage ("Image", pImg);  
        cvWaitKey (0);  
        cvDestroyWindow ("Image");  
        cvReleaseImage (&pImg);  
    }
}
```
需要注意的是cvLoadImage("C:\\Users\\liangzelang\\Desktop\\lena.jpg",1)中的图片地址的写法。<br/>
![图11](http://oapeb119y.bkt.clouddn.com/11.png)
![图12](http://oapeb119y.bkt.clouddn.com/12.png)
最后,一切OK。尽情享受OpenCV吧。
