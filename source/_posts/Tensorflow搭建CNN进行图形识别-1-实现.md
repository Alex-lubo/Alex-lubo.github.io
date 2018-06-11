---
title: 基于Tensorflow搭建CNN进行图形识别(1)-实现
date: 2018-06-06 10:48:43
tags:
---

根据基础的介绍，对CNN的卷积，常见层（非线性，池化，全连接等），图像加载，图像操作和颜色空间等相关知识做了了解。下面就是要把这些知识串联用起来，来实现一个简单的图像目标识别和分类的CNN。CNN的网络架构有很多种，这里实现采用AlexNet（2012年ILSVRC冠军）的简化版。
网络架构：
<div>
  <text>Stanford Dogs DataSet</text>
  <text>Conv Layer1</text>
  <text>Conv Layer2</text>
  <text>Hidden</text>
  <text>pool layer1</text>
  <text>pool layer2</text>
  <text>Dropout</text>
  <text>fc</text>
</div>

AlexNet采用了更多的卷积层，这里做实验，所以只采用了2层卷积。