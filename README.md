# zMatrix

zMatrix 是一个使用C++编写的图像处理库，它的基础是_Matrix矩阵模板类。
<br><br>

# 特色
* zMatrix使用类模板来支持各种类型<br>
* 函数接口尽量和`matlab`和`opencv`类似，zMatrix类可以和openCV中的Mat类转换<br>
* 使用引用计数来达到自动内存管理的目的，不需要为内存问题担心

<br><br>

# 已经实现功能

## 矩阵的基本功能
* 没有实现求秩和求逆

## 图像处理相关
### 基础
* 实现jpeg类图片的读取和写入，以及彩色图片的显示。**(需要其他功能需要使用openCV的函数)**
* 颜色空间转换，BRG->RGB, GRAY

### 线性滤波
* 方框滤波函数`boxFilter()`
* 均值滤波函数`blur()`
* 高斯滤波函数`GassionBlur()`

### 非线性滤波
* 中值滤波函数`medianBlur()`

### 形态学滤波
* 腐蚀和膨胀函数`erode()/dilate()`
* 开运算、闭运算、顶帽、黑帽、形态学梯度运算，函数`morpEX()`

### 图像变换
* sobel算子
* 简单的canny边缘检测（sobel算子），彩色和灰度均可，效果不好

## Kinect 4 Windows v2 （Events）
* Kinect获取深度、彩色和红外图像，返回Mat类型的数据

<br><br>

# BUG日志

* 2016-09-07：将迹的返回类型同一改为double，防止结果溢出
* 2016-09-08：注意不要用Matrix的引用作参数返回值，这个问题需要好好解决
* blur(), 内核大小为（5， 5，runtime(openCV:27ms):4277ms -> 2964ms -> 728ms;<br>// 受不了openCV的运行效率了，太高了(这是在DEBUG模式下，在Release下要快的多，94ms，opencv 7ms)
* medianFilter(), 内核大小为（5， 5），runtime(openCV:334ms):21822ms - > 17594ms
* GassionBlur(), 内核大小为（5， 5），runtime(openCV:116ms):4245ms
* FFT卷积？？
* erode()/dilate(), 内核大小为（5， 5）,runtime(openCV:10ms):17555ms -> 920ms(release:140ms, 2ms)
* 2016-09-16：修改sobel局部分量的估计没有乘尺度因子
* 2016-09-16：修改zMatrix中conv上次更改后出现数据类型的错误
* 2016-09-17: 修改KinectSensor析构函数中释放指针时未检查是否为空的BUG
* 2016-09-22: 修改_Scalar类定义构造函数和复制函数的错误
* 2016-09-29：字符集引起的血案，将编码格式改为UTF-8(无BOM)后虽然统一了平台，但是发现在别人的电脑上用VS2015编译会报几百的莫名其妙的错误。既然用的是VS，那么还是乖乖的把编码格式改为GB2312吧。