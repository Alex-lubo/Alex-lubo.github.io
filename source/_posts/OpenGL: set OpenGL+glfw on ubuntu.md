---
layout: 'OpenGL: set OpenGL+glfw on ubuntu'
title: ubuntu’
date: 2018-08-10 10:27:31
tags:
---
# OpenGL
准备复习并提高一下openGL，参考资料为https://learnopengl-cn.readthedocs.io/zh/latest/01%20Getting%20started/01%20OpenGL/。

看了这篇教程，学习环境和开发环境准备采用如下配置：
- ubuntu
- vscode
- openGL
- GLFW

首先，先在ubuntu上安装openGL开发环境。
## install OpenGL，OpenGL Utilities and Develop libs
```
$ sudo apt-get install builid-essential libgl1-mesa-dev libglu1-mesa-dev freeglut3-dev
$ sudo apt-get install libglew-dev libsdl2-dev libsdl2-image-dev libglm-dev libfreetype6-dev
```
## Test the installment is success.

```C++
// main.cpp
#include <GL/glut.h>
#include <stdlib.h>

/* 初始化材料属性、光源属性、光照模型，打开深度缓冲区 */
void init(void)
{
  GLfloat mat_specular[] = {1.0, 1.0, 1.0, 1.0};
  GLfloat mat_shininess[] = {50.0};
  GLfloat light_position[] = {1.0, 1.0, 1.0, 0.0};
  glClearColor(0.0, 0.0, 0.0, 0.0);
  glShadeModel(GL_SMOOTH);
  glMaterialfv(GL_FRONT, GL_SPECULAR, mat_specular);
  glMaterialfv(GL_FRONT, GL_SHININESS, mat_shininess);
  glLightfv(GL_LIGHT0, GL_POSITION, light_position);
  glEnable(GL_LIGHTING);
  glEnable(GL_LIGHT0);
  glEnable(GL_DEPTH_TEST);
}
/*调用GLUT函数，绘制一个球*/
void display(void)
{
  glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
  glutSolidSphere(1.0, 40, 50);
  glFlush();
}

int main(int argc, char **argv)
{
  /* GLUT环境初始化*/
  glutInit(&argc, argv);
  /* 显示模式初始化 */
  glutInitDisplayMode(GLUT_SINGLE | GLUT_RGB | GLUT_DEPTH);
  /* 定义窗口大小 */
  glutInitWindowSize(300, 300);
  /* 定义窗口位置 */
  glutInitWindowPosition(100, 100);
  /* 显示窗口，窗口标题为执行函数名 */
  glutCreateWindow(argv[0]);
  /* 调用OpenGL初始化函数 */
  init();
  /* 注册OpenGL绘图函数 */
  glutDisplayFunc(display);
  // /* 进入GLUT消息循环，开始执行程序 */
  glutMainLoop();
  return 0;
}
```
## compile and run
```
$ g++ main.cpp -o main -lGL -lglut
$ ./main
```
The program should run successfully.That means the installment is ok now.

# GLFW
Install GLFW.
```
$ sudo apt-get install xorg-dev
$ wget https://github.com/glfw/glfw/releases/download/3.2.1/glfw-3.2.1.zip
$ unzip glfw-3.2.1.zip
$ cd glfw-3.2.1
$ cmake
$ sudo make && make install
```
You can run simple in ./examples to check weather the installment is success.
```
$ cd examples
$ ./gears
```
Then you will see a program runing with three gear running.

# OpenGL简介
OpenGL一般被认为是一个API(Application Programming Interface, 应用程序编程接口)，包含了一系列可以操作图形、图像的函数。然而，OpenGL本身并不是一个API，它仅仅是一个由Khronos组织制定并维护的规范(Specification)。

OpenGL规范严格规定了每个函数该如何执行，以及它们的输出值。至于内部具体每个函数是如何实现(Implement)的，将由OpenGL库的开发者自行决定（注：这里开发者是指编写OpenGL库的人）。因为OpenGL规范并没有规定实现的细节，具体的OpenGL库允许使用不同的实现，只要其功能和结果与规范相匹配（亦即，作为用户不会感受到功能上的差异）。

实际的OpenGL库的开发者通常是显卡的生产商。你购买的显卡所支持的OpenGL版本都为这个系列的显卡专门开发的。当你使用Apple系统的时候，OpenGL库是由Apple自身维护的。在Linux下，有显卡生产商提供的OpenGL库，也有一些爱好者改编的版本。
## 渲染模式
在OpenGL3.2版本之前，OpenGL采用的是立即渲染模式。在这个模式下绘制图形很方便。OpenGL的大多数功能都被库隐藏起来，开发者很少能控制OpenGL如何进行计算的自由。而开发者迫切希望能有更多的灵活性。随着时间推移，规范越来越灵活，开发者对绘图细节有了更多的掌控。立即渲染模式确实容易使用和理解，但是效率太低。因此从OpenGL3.2开始，规范文档开始废弃立即渲染模式，推出核心模式(Core-profile)，这个模式完全移除了旧的特性。

当使用OpenGL的核心模式时，OpenGL迫使我们使用现代的函数。当我们试图使用一个已废弃的函数时，OpenGL会抛出一个错误并终止绘图。现代函数的优势是更高的灵活性和效率，然而也更难于学习。立即渲染模式从OpenGL实际运作中抽象掉了很多细节，因而它易于学习的同时，也很难去把握OpenGL具体是如何运作的。现代函数要求使用者真正理解OpenGL和图形编程，它有一些难度，然而提供了更多的灵活性，更高的效率，更重要的是可以更深入的理解图形编程。
## 扩展
OpenGL的一大特性就是对扩展(Extension)的支持，当一个显卡公司提出一个新特性或者渲染上的大优化，通常会以扩展的方式在驱动中实现。如果一个程序在支持这个扩展的显卡上运行，开发者可以使用这个扩展提供的一些更先进更有效的图形功能。通过这种方式，开发者不必等待一个新的OpenGL规范面世，就可以使用这些新的渲染特性了，只需要简单地检查一下显卡是否支持此扩展。通常，当一个扩展非常流行或者非常有用的时候，它将最终成为未来的OpenGL规范的一部分。

使用扩展的代码大多看上去如下：
```
if(GL_ARB_extension_name)
{
    // 使用一些新的特性
}
else
{
    // 不支持此扩展: 用旧的方式去做
}
```
## 状态机
OpenGL自身是一个巨大的状态机(State Machine)：一系列的变量描述OpenGL此刻应当如何运行。OpenGL的状态通常被称为OpenGL上下文(Context)。我们通常使用如下途径去更改OpenGL状态：设置选项，操作缓冲。最后，我们使用当前OpenGL上下文来渲染。

假设当我们想告诉OpenGL去画线段而不是三角形的时候，我们通过改变一些上下文变量来改变OpenGL状态，从而告诉OpenGL如何去绘图。一旦我们改变了OpenGL的状态为绘制线段，下一个绘制命令就会画出线段而不是三角形。

当使用OpenGL的时候，我们会遇到一些状态设置函数(State-changing Function)，这类函数将会改变上下文。以及状态应用函数(State-using Function)，这类函数会根据当前OpenGL的状态执行一些操作。只要你记住OpenGL本质上是个大状态机，就能更容易理解它的大部分特性。

## 对象
OpenGL库是用C语言写的，同时也支持多种语言的派生，但其内核仍是一个C库。由于C的一些语言结构不易被翻译到其它的高级语言，因此OpenGL开发的时候引入了一些抽象层。“对象(Object)”就是其中一个。

在OpenGL中一个对象是指一些选项的集合，它代表OpenGL状态的一个子集。比如，我们可以用一个对象来代表绘图窗口的设置，之后我们就可以设置它的大小、支持的颜色位数等等。可以把对象看做一个C风格的结构体(Struct)：
```C++
struct object_name {
    GLfloat  option1;
    GLuint   option2;
    GLchar[] name;
};
```
>*使用OpenGL时，建议使用OpenGL定义的基元类型。比如使用float时我们加上前缀GL（因此写作GLfloat）。int、uint、char、bool等等也类似。OpenGL定义的这些GL基元类型的内存布局是与平台无关的，而int等基元类型在不同操作系统上可能有不同的内存布局。使用GL基元类型可以保证你的程序在不同的平台上工作一致。*

## Graphics Pipeline
- 3D转2D坐标
  - 定点数据（Vertext Data）
    >3D坐标数据的几何，简单起见，可以认为每个定点由一个3D位置和颜色值组成。
  - *定点着色器（Vertext Shader）
    >转换3D坐标， 转化到标准化设备坐标体系（-1 ，1）中，对定点属性进行一些基本处理。 
  - 图元装配（Shape Assemble）
    >将定点组成制定图元的形状，比如Points， Triangle等。
  - *几何着色器（Geometry Shader）
    >以图元形式的定点的几何作为输入， 产生新顶点构造出形状。
  - 光栅化（Rasterization）
    >将几何着色器的输出作为输入，将图元映射到2D屏幕上，生成供片段着色器使用的片段。会执行裁切，丢弃超出视图的像素，加快渲染效率。
  - *片段着色器（Fragment Shader）
    >计算每一个像素点的最终颜色。通常片段着色器包含3D场景的数据（光照，阴影，光的颜色等），这些数据用来计算最终的像素点的颜色。
  - 测试与混合（Tests and Blending）
    >检测片段的对应的深度，判断物体的前后顺序，是否因为遮挡而丢弃，同时会检测alpha值，对物体颜色进行混合。

  `* 表示开发者可以自定义的着色器配置，即着色器编程语言编程（GLSL）`
- 2D坐标转变成实际有颜色的像素
  - 视口变换

## OpenGL对象
- 缓冲对象
  - VBO(Vertext Buffer Object)
    - 类型: GL_ARRAY_BUFFER
- 着色器对象
 