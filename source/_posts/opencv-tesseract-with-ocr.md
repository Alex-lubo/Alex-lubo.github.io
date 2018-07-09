---
title: 'opencv: tesseract with ocr'
date: 2018-07-09 11:36:28
tags:
---

# tesseract
项目地址：https://github.com/tesseract-ocr/tesseract

Tesseract是HP实验室开发,后来HP放弃了OCR项目，该项目也就束之高阁了，后来hp想着放着也是放着，不如把它开源，于是2005年惠普在与拉斯维加斯Nevada大学合作项目中将它开源。2006年该项目由google接手继续开发。

Tesseract在version4中采用了基于LSTM的识别引擎。

## ubuntu14.04 安装tesseract
```
sudo add-apt-repository ppa:alex-p/tesseract-ocr
sudo apt-get update
sudo apt install tesseract-ocr
sudo apt install libtesseract-dev
sudo pip install pytesseract //如果要在python中使用，需这一步
```
如果要在ubuntu的C++开发环境中使用tesseract，需要在pkg-config中配置一下，以便编译时计算机可以帮助查询已安装的库的使用接口。配置方法如下：
```
$ cd /usr/lib/pkgconfig
# touch tesseract.pc
# vim tesseract.pc

prefix=/usr
exec_prefix=${prefix}
libdir=${exec_prefix}/lib/
includedir=${prefix}/include/tesseract/

Name:Tesseract
Description:Tesseract C++
URL:https://code.google.com/p/tesseract-ocr/
Version:3.03~20130725
Libs:-L${libdir} -ltesseract
Cflags:-I${includedir}

#
```
检测是否正确：
```
$ pkg-config --cflag --libs tesseract
-I/usr/include/tesseract/  -L/usr/lib/ -ltesseract
```

## tesseract使用
There are four OCR Engine Mode (oem) available（version 4）
     
    OEM_TESSERACT_ONLY             Legacy engine only.
    OEM_LSTM_ONLY                  Neural nets LSTM engine only.
    OEM_TESSERACT_LSTM_COMBINED    Legacy + LSTM engines.
    OEM_DEFAULT                    Default, based on what is available.

使用tesseract+opencv的简单例子：
```
//simple.cpp

#include <string>
#include <opencv2/opencv.hpp>
#include <tesseract/baseapi.h>
#include <leptonica/allheaders.h>

using namespace cv;

int main(int argc, char* argv[])
{
  string outText;

  tesseract::TessBaseAPI *ocr = new tesseract::TessBaseAPI();
  if(ocr->Init(NULL, "eng", tesseract::OEM_DEFAULT)) {
    printf("Could not initialize tesseract.\n");
    return EXIT_FAILURE;
  };
  ocr->SetPageSegMode(tesseract::PSM_AUTO);
  
  Mat m = imread(argv[1], 1);
  if(m.empty()) {
    printf("No image data \n");
    return EXIT_FAILURE;
  }
  ocr->SetImage(m.data, m.cols, m.rows, 3, m.step);
  outText = string(ocr->GetUTF8Text());
  std::cout<<outText<<std::endl;
  ocr->End();
  return EXIT_SUCCESS;
}
```

编译：
```
makefile：
ocr: ./OCR/simple.cpp
	gcc -g -Wall simple.cpp `pkg-config --cflags --libs opencv tesseract ` -o ocr-simple
```
