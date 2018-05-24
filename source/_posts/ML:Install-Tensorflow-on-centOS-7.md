---
title: Install Tensorflow on centOS 7(ML-1)
date: 2018-03-29 22:40:36
tags:
---

准备动手实践机器学习的东西，目前也看了许多机器学习相关的书，感觉书看得再多没有实践的检验，怕自己的理解或许会出问题。
1, 数学太抽象，如果没有实践，你也很容易就忘记了，所以目的一就是通过实践加深理解和记忆；2， 许多书为了让人理解，数据和模型参数都会选的很简单的，真正的工作中数据的维数、数量的复杂度会远远比书上难， 所以需要编程实践区掌握这写算法，让算法落地。‘纸上得来终觉浅，须知此事要躬行’。所以为了提高学习的效率，以[google的机器学习教程](https://developers.google.com/machine-learning/crash-course/prereqs-and-prework?hl=zh-cn)为基础，察缺补漏，提高对机器学习的理解和实践能力。

ML这些文章记录下自己的学习过程。
按照教程的前提条件和准备工作对照了一下，初级代数和基本数学知识还是可以的，编程能力吗之前使用的是JS，C++,C#，python偶尔使用。所以直接开始还是没有问题的。
那么问题来了，要实践，首先需要有环境吧。 so ...
首先，搭建环境。自己的笔记本上是CentOS7系统，直接就在Centos上开始吧。
机器配置有独显，看下配置：
```
～ lspci |grep -i vga
01:00.0 VGA compatible controller: NVIDIA Corporation GT218M [NVS 3100M]

～ nvidia-smi
Thu Mar 29 22:43:19 2018       
+------------------------------------------------------+                       
| NVIDIA-SMI 340.104    Driver Version: 340.104        |                       
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  NVS 3100M           Off  | 0000:01:00.0     N/A |                  N/A |
| N/A   58C    P0    N/A /  N/A |    323MiB /   511MiB |     N/A      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Compute processes:                                               GPU Memory |
|  GPU       PID  Process name                                     Usage      |
|=============================================================================|
|    0            Not Supported                                               |
+-----------------------------------------------------------------------------+

~ glxinfo | grep rendering 
direct rendering: Yes
    GL_NV_parameter_buffer_object2, GL_NV_path_rendering, 
    GL_NV_parameter_buffer_object2, GL_NV_path_rendering,
```
如果有独立显卡，需要安装驱动。关于安装NVIDIA显卡驱动，可以参考我的博文：CentOS 7 安装NVIDIA驱动。
在[NVIDIA上查看自己的GPU型号是否支持CUDA](https://developer.nvidia.com/cuda-gpus).可以看出，我的笔记本的显卡虽然支持cuda，但是配置显卡。可以安装支持GPU运算的Tenorflow。要使用GPU，需要安装CUDA TOOlkit。

1. 安装CUDA
    - 因为我的机器显卡实在是老旧，虽然也可以装cuda，正常的步骤是：下载对应的版本。从NVIDIA的[官网上下载CUDA](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&target_distro=CentOS&target_version=7&target_type=rpmlocal).安装说明也在网页上，按照说明安装。
    ```
    ~ sudo rpm -i cuda-repo-rhel7-9-1-local-9.1.85-1.x86_64.rpm
    ~ sudo yum clean all
    ~ sudo yum install cuda
    ```
    不过安装完后有各种启动问题。所以先不安装cuda。安装完后cuda的配置见步骤2.
2. 配置环境变量。
    因为我是用zsh， so编辑在～/.zshrc中增加如下内容：
    ```
    #gpu driver  
    export CUDA_HOME=/usr/local/cuda-9.1  
    export PATH=/usr/local/cuda-9.1/bin:$PATH  
    export LD_LIBRARY_PATH=/usr/local/cuda-9.1/lib64:$LD_LIBRARY_PATH  
    export LD_LIBRARY_PATH="/usr/local/cuda-9.1/lib:${LD_LIBRARY_PATH}
    ```

3. 安装tensorflow - Installing with Virtualenv（[参考tensorflow官网安装流程安装]()）
    我是python2.7的，所以选择2.7的安装， 并且因为机器原因就不安装支持gpu的版本了。

    ```
    $ sudo apt-get install python-pip python-dev python-virtualenv # for Python 2.7

    $ virtualenv --system-site-packages targetDirectory # for Python 2.7

    $ source ~/tensorflow/bin/activate # bash, sh, ksh, or zsh

    (tensorflow)$ easy_install -U pip

    (tensorflow)$ pip install --upgrade tensorflow  # for Python 2.7
    ```

4. 验证安装
如何验证呢，自然是虚拟环境中运行一段tensorflow的程序啦
因为时在virutalenv环境下，安装完tensorflow后记得要更新以下环境。
```
(tensorflow)$ source ./bin/activate
(tensorflow)$ python

# Enter the following short program inside the python interactive shell:
>>> import tensorflow as tf
>>> hello = tf.constant('Hello, TensorFlow!')
>>> sess = tf.Session()
>>> print(sess.run(hello))

# 输出
Hello, TensorFlow!
```
因为自己机器的cpu型号比较老，所以在import tensorflow的时候报错误：Illegal instruction (core dumped)。
google一番后发现这个问题很多人都碰到了，将级到tensorflow1.5可以运行。那么也降级安装1.5版本的tensorflow吧。

```
$ pip uninstall tensorflow
$ pip install tensorflow==1.5
```
done!
实际上即便是1.5的版本，tensorflow可以执行，但是warnning：
```
Your CPU supports instructions that this TensorFlow binary was not compiled to use: SSE4.1 SSE4.2
```
可以手动编译可以根据自己机器的匹配进行配置，提高tensorflow的效率，后面再仔细研究以下tensorflow的编译吧。目前的任务是学习tensorflow来解决数学问题。
4. 然后就可以开始使用tensorflow进行机器学习的相关开发啦。




补充介绍：
* [virtualenv]python中的第三方包，用来管理python虚拟环境的的方法之一，它通过创建虚拟而独立的python环境，使每个项目的环境与其他项目环境独立开来，保证各自环境的干净，避免包冲突的问题。
```
// create virtual env
$ virtualenv targetDir                              //targetDir 虚拟环境名称
$ virtualenv ---system-site-packages targetDir      //创建时使用系统环境
$ virtualenv -p /usr/bin/python2.7 targetDir        //还可以指定虚拟环境中python的版本
$ cd targetDir                                      //进入虚拟环境
$ source bin/activate                               //启动虚拟环境
$ deactivate                                        //退出虚拟环境
$ cd .. & rm targetDir                              //删除虚拟环境
```