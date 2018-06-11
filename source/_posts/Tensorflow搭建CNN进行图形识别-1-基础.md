---
title: 基于Tensorflow搭建CNN进行图形识别(1)-基础
date: 2018-06-04 16:18:47
tags:
---

自从2012年的ImageNet举行的大规模视觉识别挑战赛（ILSVRC）以来，CNN（卷积神经网络，不是指某新闻资讯频道）在计算机视觉领域取得的空前准确性使之一直为人们所持续关注。我想自己做点图像识别方向的小东西，目前在为实现积攒技术，比如学习tf、opencv等。本篇文章记录下采用tf搭建CNN的过程，记录自己的实际操作过程。有些理论的东西就直接搬过来了，以对抗遗忘。

个人写博客，就是写给自己，目前水平有限，不敢说指导别人了，纯粹就当自己的读书笔记了。

好了，言归正传，要搭建CNN，首先要了解什么是CNN。CNN（卷积神经网络）是一种至少包含一个卷积层的神经网络，该层使计算输入f与一组可配置的卷积核g的卷积。[知乎上红色石头的机器学习之路](https://zhuanlan.zhihu.com/p/31554961)已经对卷积神经网络的基础做出了图文并茂的解释，所以这部分内容可以参考，如果E文比较好的话直接上coursera上看Andrw Ng的课程。就不在炒冷饭了（写的可能还没它的好）。

**CNN的组成:**
+ 图像输入
+ 卷积层
  + conv2d
  + depthwise_conv2d
  + separable_conv2d
  + conv2d_transpose
+ 激活函数:
  + relu
  + sigmoid[0.0, 1.0]
  + tanh[-1.0, 1.0]
  + dropout
+ 池化层
  + max_pool
  + avg_pool
+ 全连接层
  + fully_connected


**图像操作：**
+ 裁剪
  + tf.image.central_crop()
  + tf.image.crop_to_bounding_box()
  + tf.slice()
+ 边界填充
  + tf.image.pad_to_bounding_box()
  + tf.image.resize_image_with_crip_or_pad()
+ 翻转
  + tf.image.flip_left_rigth()
  + tf.image.flip_up_down()
  + tf.image.random_flip_left_right()
  + tf.image.random_flip_up_down()
+ 饱和平衡
  + tf.image.adjust_brightness()
  + tf.image.adjust_contrast()
  + tf.image.adjust_saturation()

**颜色转换**
+ 灰度
  + tf.image.rgb_to_grayscale()
  > 每个像素的所有颜色值取平均，将其作为灰度值来实现。
+ HSV(HSB)空间
  + tf.image.rgb_to_hsv()
+ RGB
  + tf.image.grayscale_to_rgb()
  > 意义不大。只是将灰度值作为RGB来填充得到。
  + tf.image.hsv_to_rgb()
+ LAB空间