---
title: OpenCV Execise 1
date: 2018-08-03 13:47:04
tags:
---

3.2与3.3题代码 创建拥有三个通道的二维矩阵，字节类型，大小为100*100，并所有值赋为0，在图中画一个圆形。
通过函数cvPtr2D，在图中的通道2中画一个以（20,5）与（40,20）为定点的绿色长方形。

```c++
#include <iostream>
#include <string>
#include <opencv2/opencv.hpp>

using namespace std;
using namespace cv;

int main(int argc, char* argv[]) {
  Mat img(100, 100, CV_8UC3, Scalar(0, 0,0));
  IplImage* img2 = cvCreateImage(cvSize(100, 100), IPL_DEPTH_8U, 3);
  cvZero(img2);
  // 3.2题
  circle(img, Point(50, 50), 10, 135, 2,8, 0);
  namedWindow("tet", CV_WINDOW_AUTOSIZE);
  imshow("tet", img);
  int top = 20,left=5,bottom=40, right=20;
  // draw plant 3.4题
  // for(int row = top;row <bottom;row++){
  //   for(int col = left;col <right;col++){
  //     *(cvPtr2D(img2, row, col)+1)=255;
  //   }
  // }
  
  // draw rectangle 3.3题
  for(int col = left;col <right;col++){
    *(cvPtr2D(img2, top, col)+1)=255;
    *(cvPtr2D(img2, bottom, col)+1)=255;
  }
  for(int row = top;row <bottom;row++){
    *(cvPtr2D(img2, row, left)+1)=255;
    *(cvPtr2D(img2, row, right)+1)=255;
  }
  namedWindow("tet1", CV_WINDOW_AUTOSIZE);
  cvShowImage("tet1", img2);

  cvWaitKey(0);
  return EXIT_SUCCESS;
}
```