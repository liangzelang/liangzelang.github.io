---
title: HigtGUI的初识
date: 2017-03-01 06:48:02
categories:
- 技术
tags:
- OpenCV基础
- C/C++
comments: true
---

1. <b>imread()函数</b>
	Mat imread(const string & filename, int flags = 1);
其中第一个参数表示要读取的图像文件名， const string &类型的filename。值得注意的是图像的路径。支持多种类型的图像载入，主流的有bmp\jpg\jpeg\png\ppm\pgm\ras\tif等类型。
第二个参数表示载入标识，就是需要以何种方式把原图像呈现，默认参数为1。可以在OpenCV的图像标识枚举中选取：

```
enum
{
/* 8bit, color or not */
//新的版本中已经取消，忽略，原通道读取
    CV_LOAD_IMAGE_UNCHANGED  =-1, 
/* 8bit, gray */
//原图像转换为灰度图像输出
    CV_LOAD_IMAGE_GRAYSCALE  =0,
/* ?, color */
//原图像转换为彩色图像输出，如果原图像是灰度图像，输出还是灰度
    CV_LOAD_IMAGE_COLOR      =1,
/* any depth, ? */
//如果图像深度为16或者32位，则输出相应深度的图像，否则转换为8位深度图像
    CV_LOAD_IMAGE_ANYDEPTH   =2,
/* ?, any color */
//原图像色彩保持不变
    CV_LOAD_IMAGE_ANYCOLOR   =4
};
```
但是OpenCV3中这些宏都失效，故采用数字。
```
Mat img = imread("pic.jpg",2|4)  //载入无损的源图像
```
<!--more-->
2. <b>imshow()函数</b>
void imshow(const string & winname, InputArray mat);
const string & winname: 显示到哪个窗口；
InputArray mat: 图像数据，InputArray类型简单（笼统）理解成Mat类型吧

3. <b>namedWindow()函数</b>
void namedWindow(const string & winname, int flags = WINDOW_AUTOSIZE);
const string & winname: 创建这个窗口名字；
int flags: 窗口属性
```
WINDOW_NORMAL: 可以改变窗口大小
WINDOW_AUTOSIZW: 根据图像大小自动调整，用户不能调整
WINDOW_OPENGL: 窗口支持OpenGL
```

4. <b>imwrite()函数</b>
bool imwrite(const string & filename, InputArray img, const vector<int>& params = vector<int>());
const string & filename: 图片另存为 什么格式，什么路径
InputArray img: 要保存的图像数据
const vector<int>& params: 就是图像编码标识（这是一个int类型的vector,默认为空）
+ 对于JPEG格式图像：表示保存的图像质量（CV_IMWRITE_JPEG_QUALITY） 0-100,越大质量越高，默认95；
+ 对于PNG： 表示图像压缩级别（CV_IMWRITE_PNG_COMPRESSION） 0-9，越大压缩程度越大， 默认3；
+ 对于PPM，PGM，PBM：表示二进制格式表示（CV_IMWRITE_PXM_BINARY），0或1，默认1，这个参数没有太懂是什么意思；

5. <b>createTrackbar()函数</b>
int createTrackbar( const string & trackbarname, const string & winname, int * value, int count,  TrackbarCallback onChange=0, void * userdata=0);
const

6. </b>getTrackbarPos()函数</b>
int getTrackbarPos(const string & trackbarname, cosnt string & winname);
就是获取当前滑动条的位置。
const string & trackbarname: 滑动条名字
const string & winname: 窗口名字。

7. <b>setMouseCallBack()函数</b>
void setMouseCallBack(const string & winname, MouseCallback onMouse, void * userdata = 0)
Mousecallback onMouse: 鼠标事件的回调函数，这个函数的格式为 void func(int event, intx, int y, int flags, void * param),其中event是指鼠标事件（是单击左键，右键等），x和y表示鼠标在图像坐标系（不是窗口坐标系）的坐标。



8. <b>BUG警示</b>
在键入一个保存图像的程序，在Debug模式下运行，编译、链接都成功，运行阶段程序报错：Openc Error ：Assertion failed <size.width>0 && size.height>0> in cv::imshow file ..\opencv\modules\highgui\src\window.cpp line 261
![程序报错](http://oapeb119y.bkt.clouddn.com/Highgui%E4%B9%8B%E9%93%BE%E6%8E%A5%E5%BA%93%E9%94%99%E8%AF%AF%EF%BD%82.png)

```
#include<opencv2/opencv.hpp>
#include <opencv2/highgui/highgui.hpp>
using namespace cv;
using namespace std;

int main( )
{
	//显示图片
	try{
		//imwrite("透明Alpha值图.PNG", mat,compression_params);
		Mat a = imread("learn.jpg",1);
		namedWindow("nima",WINDOW_AUTOSIZE);
		imshow("nima",a);
		waitKey(0);
	}
	catch(runtime_error& ex) {
		fprintf(stderr,"发生错误：%s\n", ex.what());
		return 1;
	}

	return 0;
}
```
原因： 这是Opencv 2.4.1版本以来的一个BUG，编译器将自动优先选择依赖项（就是lib库）。在【属性管理器】->【Debug|Win32】->Microsof.cpp.win32.user【属性】->【链接器】->【输入】->【附加依赖库】中，在安装配置时，添加了两种依赖库，一种是名称后面不带d的（是release模式下链接库），一种是带的（是Debug下的链接库）。但是编译器自动选择放置在前面的链接库作为默认支持的调试方式。我是把不带d的lib库放在前面，导致Debug模式下的各种错误（因为字符串读取问题引起的如图片载入不了、报指针越界、内存错误等）。

解决方法：由于我常在Debug模式下调试，故将带d的lib库放在不带d的lib库前面，重新生成解决方案后，Debug模式下程序正常。但是想在Release模式下怎么办呢？工程切换至Relea模式，【项目】->【属性】->【链接器】->【输入】->【附加依赖库】添加不带d的lib链接库，重新生成解决方案，完美解决。值得欣慰的是，在OpenCV3中这个ＢＵＧ已经修复了。

<b>总结</b>
主要是熟悉OpenCV简单的图像操作，以及滑动条以及鼠标等交互方式。

