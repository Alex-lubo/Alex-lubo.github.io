---
title: opencv部分函数公式摘要
date: 2018-08-01 17:06:21
tags:
---
- 图像融合
  ```
  void cvAddWeighted(
    const CVArr* src1,
    double alpha,
    const CVArr* src2,
    beta, double gamma,
    CvArr* dst
  );
  ```
  写入dst的结果采用如下公式得出：
  
  <a href="https://www.codecogs.com/eqnedit.php?latex=dst_{x,y}=\alpha&space;\cdot&space;src1_{x,y}&plus;\beta&space;\cdot&space;src2_{x,y}&space;&plus;&space;\gamma" target="_blank"><img src="https://latex.codecogs.com/gif.latex?dst_{x,y}=\alpha&space;\cdot&space;src1_{x,y}&plus;\beta&space;\cdot&space;src2_{x,y}&space;&plus;&space;\gamma" title="dst_{x,y}=\alpha \cdot src1_{x,y}+\beta \cdot src2_{x,y} + \gamma" /></a>
  
  图像可以是任意类型的像素，但需属于同一类型。图像可以时不同尺寸，但是ROI必须尺寸统一， 否则OpenCV就会产生错误。参数alpha是src1的融合强度，beta是src2的融合强度。当alpha在0到1之间取值，beta=1-alpha，gamma=0时，公式就为alpha融合。

- 颜色空间转换
  ```
  void cvCvtColor(
    const CvArr* src,
    CvArr* dst,
    int code
  );
  ```
  该函数将图像从一个颜色空间转换到另一个。具体转换操作由code来指定。
  色彩空间有如下约定:
    - 8位图像值范围是0~256
    - 16位图像值范围是0~65536
    - 浮点数的取值范围为0.0~1.0
    - 灰度图像转换成彩色图像时，最终图像的所有通道的值都是相同的
    - 彩色图像转灰度图像时，灰度值的计算使用权值公式：<a href="https://www.codecogs.com/eqnedit.php?latex=Y=(0.299)R&plus;(0.587)G&plus;()0.114)B" target="_blank"><img src="https://latex.codecogs.com/gif.latex?Y=(0.299)R&plus;(0.587)G&plus;()0.114)B" title="Y=(0.299)R+(0.587)G+()0.114)B" /></a>

- 图像旋转
  ```
  void cvFlip(
    const CvArr* src,
    CvArr* dst = NULL,
    int flip_mode
  );
  ```
  将图像绕X轴或者Y轴或者同时绕X、Y旋转。flip_mode为0时，图像只会绕X轴旋转，flip_mode设置为正值时（+1），图像绕Y轴旋转；如果设置为负值，则同时绕X和Y轴旋转。比较常用的时坐标原点在左上角和左下角的变换。

- 绘图
  - 直线cvLine(src, startPt, endPt, color, thickness, connectivity)
  - 矩形cvRectangle(src, startPt, endPt, color, thickness)
  - 圆形cvCircle(src, center, radius, color, thickness, connectivity)
  - 椭圆cvEllipse(src,center,axes,angle,startAg,endAg,color,thickness,linetype)
  - 多边形cvFillPoly(src, pts, npts, contours,color,lineType)
  - 字体和文字cvPutText(src, text,origin, font,color)

- 数据存储
  YAML或XML格式来存储序列化的数据。
  - 矩阵数据的存储cvSave/cvLoad
  - cvOpenFileStorage