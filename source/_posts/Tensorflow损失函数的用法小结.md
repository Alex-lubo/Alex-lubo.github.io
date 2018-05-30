---
title: Tensorflow损失函数的用法小结
date: 2018-05-30 16:47:34
tags:
---

常见的Loss函数时MSE（Mean squared Error）.计算方法如下：


均方差很好理解，就是反映预测值与真实值之间的差异情况。为避免负值，一般取绝对值或者平方。

<a href="https://www.codecogs.com/eqnedit.php?latex=L&space;=&space;\frac{1}{n}\sum_{k=1}^{n}(y_{k}-y_{k}^{'})^{2}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?L&space;=&space;\frac{1}{n}\sum_{k=1}^{n}(y_{k}-y_{k}^{'})^{2}" title="L = \frac{1}{n}\sum_{k=1}^{n}(y_{k}-y_{k}^{'})^{2}" /></a>

`Y`表示真实值，`Y‘`表示预测值。

在tensorflow中表达MSE：
```
import tensorflow as tf

Y_ = tf.matmul(X, W) + b
tf.reduce_mean(tf.squared_difference(Y, Y_))
```

MSE可以表达预测值与真实值之间的差异，但是在分类问题中效果不如交叉熵好。

**交叉熵**

1948年，Claude Shannon在他的著名论文（A Mathematic Theory of Communication）中提出信息熵的概念来对信息进行度量。信息的信息量与其不确定性有直接的关系。如果对某件事一无所知，那么就需要大量的信息，如果一件事情百分百确定，那么给出的再多信息都没有信息量。所以，信息量就等于不确定性的多少。

信息熵的定义：

<a href="https://www.codecogs.com/eqnedit.php?latex=H(X)=-\sum_{x\in&space;X}P(x)lnP(x)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?H(X)=-\sum_{x\in&space;X}P(x)lnP(x)" title="H(X)=-\sum_{x\in X}P(x)logP(x)" /></a>

其定义形式与热力学的熵有很大的相似性。

在信息论中，若一个符号字符串中每个字符出现概率`Pi`已知，就利用信息熵估计该字符串中每个符号`Si`编码所需的平均最小位数，其意义是，采用最优编码方案是，每个字符号需要的bit位。比如要对’helloworld‘这个字符串进行编码，可以先计算每个字符出现的概率，然后计算信息熵。
```
P('h') = P('e') = P('w') =P('r') = P('d') = 1/10 = 0.1
P('o') = 2/10 = 0.2
P('l') = 3/10 = 0.3
H = -0.3*log(0.3)-0.2*log(0.2)-5*0.1*log(0.1)≈2.646≈3
```
采用最优化编码方式来进行编码时每个符号需要3bit。

要了解交叉熵，首先了解下相对熵。

相对熵是用来衡量两个取值为正数的函数的相似性，这与变量的互信息不同（互信息是用来表示，在了解其中一个变量的情况下，对消除另一个X不确定性所提供的信息量，类似于条件概率）。交叉熵的公式表达为：

<a href="https://www.codecogs.com/eqnedit.php?latex=KL(f(x)||q(x))=\sum_{x\in&space;X}f(x)log\frac{f(x)}{q(x)}=\sum_{x\in&space;X}f(x)logf(x)-\sum_{x\in&space;X}f(x)log&space;q(x)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?KL(f(x)||q(x))=\sum_{x\in&space;X}f(x)log\frac{f(x)}{q(x)}=\sum_{x\in&space;X}f(x)logf(x)-\sum_{x\in&space;X}f(x)log&space;q(x)" title="KL(f(x)||q(x))=\sum_{x\in X}f(x)log\frac{f(x)}{q(x)}=\sum_{x\in X}f(x)logf(x)-\sum_{x\in X}f(x)log q(x)" /></a>

关于互信息，有三条结论：

+ 完全相同的函数，交叉熵等于0
+ 交叉熵越大，两个函数的差异就越大；交叉熵越小，两个函数的差异越小，
+ 对于概率分布或者概率密度函数，如果取值均大于0，交叉熵可以度量两个随机分布的差异性。

也可以直接采用其他编码方式进行编码，比如ASCII，这样每个符号赋予相等的概率值1/256，

