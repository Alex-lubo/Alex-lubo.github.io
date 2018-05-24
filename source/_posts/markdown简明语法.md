---
title: markdown简明语法
date: 2018-02-27 22:14:32
tags: blog， markdown
---

## 1. 为什么使用Markdown？
+ 易读、易写、易更改的纯文本
+ 兼容HTML，可以转成HTML发布
+ 跨平台使用
+ 更方便的组织email
+ 无需专门的编写软件，任意的文本编辑器就可以

## 2. 语法
Markdown语法主要分成以下几个大的部分：
- 标题
- 段落
- 区块引用
- 代码区块
- 强调
- 列表
- 分割线
- 链接
- 图片
- 反斜杠\
- 符号‘`’

### 2.1 标题
1）使用 `=` 和 `-` 标记一级和二级标题
> 一级标题  
> `=`  
> 二级标题  
> `-`  

效果：
> 一级标题   
> ==     
> 二级标题    
> --    


2）使用`#`，标示1-6级标题
> \# 一级标题  
> \## 二级标题   
> \### 三级标题  
> \#### 四级标题  
> \##### 五级标题  
> \###### 六级标题  

效果：
> # 一级标题
> ## 二级标题
> ### 三级标题
> #### 四级标题
> ##### 五级标题
> ###### 六级标题

### 2.2 段落
段落的前后要有空行，所谓的空行是指没有文字内容。若想在段内强制换行的方式是使用**两个以上**空格加上回车（引用中换行省略回车）。

### 2.3 区块引用
在段落的每行或者只在第一行使用符号`>`,还可使用多个嵌套引用，如：
> \> 区块引用  
> \>> 嵌套引用  

效果：
> 区块引用  
>> 嵌套引用

### 2.4 代码区块
代码区块的建立是在每行加上4个空格或者一个制表符（如同写代码一样）。如    
普通段落：

void main()    
{    
    printf("Hello, Markdown.");    
}    

代码区块：

    void main()
    {
        printf("Hello, Markdown.");
    }

**注意**:需要和普通段落之间存在空行。

### 强调
在强调内容两侧分别加上`*`或者`_`，如：
> \*斜体\*，\_斜体\_    
> \*\*粗体\*\*，\_\_粗体\_\_

2.5 效果：
> *斜体*，_斜体_    
> **粗体**，__粗体__

### 2.6 列表
使用`·`、`+`、或`-`标记无序列表，如：
> \-（+\*） 第一项
> \-（+\*） 第二项
> \- （+\*）第三项

**注意**：标记后面最少有一个_空格_或_制表符_。若不在引用区块中，必须和前方段落之间存在空行。

效果：
> + 第一项
> + 第二项
> + 第三项

有序列表的标记方式是将上述的符号换成数字,并辅以`.`，如：
> 1 . 第一项   
> 2 . 第二项    
> 3 . 第三项    

效果：
> 1. 第一项
> 2. 第二项
> 3. 第三项

### 2.7 分割线
分割线最常使用就是三个或以上`*`，还可以使用`-`和`_`。

### 2.8 链接
链接可以由两种形式生成：**行内式**和**参考式**。    
**行内式**：
> \[Markdown语法\]\(https:://https://alex-lubo.github.io/ "Markdown"\)。

效果：
> [Markdown语法](https:://https://alex-lubo.github.io/ "Markdown")。

**参考式**：
> \[alex's blog 1\]\[1\]    
> \[alex's blog 2\]\[2\]    
> \[1\]:https://alex-lubo.github.io/ "Markdown"    
> \[2\]:https://alex-lubo.github.io/ "Markdown"    

效果：
> [alex's blog 1][1]    
> [alex's blog 2][2]

[1]: https://alex-lubo.github.io/ "Markdown"
[2]: https://alex-lubo.github.io/ "Markdown"

**注意**：上述的`[1]: https://alex-lubo.github.io/ "Markdown"`不出现在区块中。

直接展示链接的写法:
> \<https://alex-lubo.github.io/>

效果：
> <https://alex-lubo.github.io/>

### 2.9 图片
添加图片的形式和链接相似，只需在链接的基础上前方加一个`！`。
> \!\[显示图片\]\(https://developer.mozilla.org/files/4617/default.svg　“eventloop”\)

效果：

![test](https://developer.mozilla.org/files/4617/default.svg)

### 2.10 反斜杠`\`
相当于**反转义**作用。使符号成为普通符号。
### 2.11 符号'`'
起到标记作用。如：
> \`ctrl+a\`

效果：
> `ctrl+a`   

### 2.13 其他
1. 分割线

三个以上的短线`-`即可做出分割线。
> \----

效果：
> ---

2. 键盘键

> \<kbd>ctrl+[\</kbd>

3. 使用icon图标文字

> \<i class="icon-cog"></i>

4. 表格

非traditional markdown.
使用`|`标示表格的纵向边界，　表头和表内容用`-`隔开，并可用`:`进行对齐设置，两边都有`:`表示居中，若不加，则默认左边对齐。
```
Item    | value | brand 
:-----: | ----: | -----
computer| $1000 | hp
phone   | $100  | apple
pipe    | $1    | dlp
```

效果：
Item    | value | brand 
:-----: | ----: | -----
computer| $1000 | hp
phone   | $100  | apple
pipe    | $1    | dlp

5. 公式

**方法1**
参考[这篇文章](https://magicly.me/markdown-math/)。
简单办法，首先上这个[在线工具](https://webdemo.myscript.com/views/math.html#)，手写公示，然后它会转成LaTex!,然后放置在需要的位置就可以了。
放置方法
> \$$ 此处插入公式 $$

效果：
> $$ H\left(x ,y\right) =\sum ^{M-1}_{i=0}\sum ^{M-1}_{j=0}I\left( x+i-a_{i},y+j-a_{j}\right) G\left( i,j\right) $$

**方法2**
直接上这个[在线编辑LaTex的工具](https://www.codecogs.com/latex/eqneditor.php),编辑需要的公式（类似ms公式编辑器，很好用）。直接复制生成html到md文件中即可。
```
<a href="https://www.codecogs.com/eqnedit.php?latex=\sum_{i=0}^{M-1}\sum_{j}^{M-1}I(x&plus;i-a_{_{i}},&space;y&plus;i-a_{_{j}})G(i,j)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\sum_{i=0}^{M-1}\sum_{j}^{M-1}I(x&plus;i-a_{_{i}},&space;y&plus;i-a_{_{j}})G(i,j)" title="\sum_{i=0}^{M-1}\sum_{j}^{M-1}I(x+i-a_{_{i}}, y+i-a_{_{j}})G(i,j)" /></a>
```

效果：
<a href="https://www.codecogs.com/eqnedit.php?latex=\sum_{i=0}^{M-1}\sum_{j}^{M-1}I(x&plus;i-a_{_{i}},&space;y&plus;i-a_{_{j}})G(i,j)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\sum_{i=0}^{M-1}\sum_{j}^{M-1}I(x&plus;i-a_{_{i}},&space;y&plus;i-a_{_{j}})G(i,j)" title="\sum_{i=0}^{M-1}\sum_{j}^{M-1}I(x+i-a_{_{i}}, y+i-a_{_{j}})G(i,j)" /></a>

