---
title: learn python - excellent materials.md
date: 2017-11-21 22:51:35
tags:
---
You can get a lot of learing materials about python. The list belowing contains the resource that pull over me a lot, so i note them for mem.

- CS231n课程笔记翻译：Python Numpy教程: https://zhuanlan.zhihu.com/p/20878530


按理说运行下面一句就可以安装了
`pip install matplotlib`
但是对于我的redhat7.2+python2.7.5，报了下面的错误

>_posixsubprocess.c:3:20: 致命错误：Python.h：没有那个文件或目录

于是需要先安装python-devel

`sudo yum install python-devel`