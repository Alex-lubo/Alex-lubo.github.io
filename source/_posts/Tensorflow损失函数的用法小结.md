---
title: Tensorflow损失函数的用法小结
date: 2018-05-30 16:47:34
tags:
---

在进行机器学习时，一般的过程是，根据问题的特征（样本参数）建立模型，当输入特征参数到模型中时，可以获得与实际测量结果相同或者相近的计算结果。开始时并不知道模型应该使用什么样的参数，只能通过给定结果的训练数据集，对模型进行训练，使得模型计算结果与实际测量结果相同或者接近，用数据表示就是损失函数。对模型进行优化的过程其实就是在求损失函数最小值的过程。

常用的损失函数有MSE，交叉熵。Tensorflow提供了不同损失函数的实现。下面简要介绍总结一下Tensorflow中MSE和交叉熵损失函数的用法。

## MSE
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

## 交叉熵

1948年，Claude Shannon在他的著名论文（A Mathematic Theory of Communication）中提出信息熵的概念来对信息进行度量。信息的信息量与其不确定性有直接的关系。如果对某件事一无所知，那么就需要大量的信息，如果一件事情百分百确定，那么给出的再多信息都没有信息量。所以，信息量就等于不确定性的多少。

信息熵的定义：

<a><img src="https://latex.codecogs.com/gif.latex?H(X)=-\sum_{x\in&space;X}P(x)lnP(x)" title="H(X)=-\sum_{x\in X}P(x)logP(x)" /></a>

其定义形式与热力学的熵有很大的相似性。

在信息论中，若一个符号字符串中每个字符出现概率`Pi`已知，就利用信息熵估计该字符串中每个符号`Si`编码所需的平均最小位数，其意义是，采用最优编码方案是，每个字符号需要的bit位。比如要对’helloworld‘这个字符串进行编码，可以先计算每个字符出现的概率，然后计算信息熵。
```
P('h') = P('e') = P('w') =P('r') = P('d') = 1/10 = 0.1
P('o') = 2/10 = 0.2
P('l') = 3/10 = 0.3
H = -0.3*log(0.3)-0.2*log(0.2)-5*0.1*log(0.1)≈2.646≈3
```
采用最优化编码方式来进行编码时每个符号需要3bit。

要了解交叉熵，首先了解下相对熵（kullback-leibler divergence）。

相对熵是用来衡量两个取值为正数的函数的相似性，这与变量的互信息不同（互信息是用来表示，在了解其中一个变量的情况下，对消除另一个X不确定性所提供的信息量，类似于条件概率）。交叉熵的公式表达为：

<a><img src="https://latex.codecogs.com/gif.latex?KL(f(x)||q(x))=\sum_{x\in&space;X}f(x)log\frac{f(x)}{q(x)}=-\sum_{x\in&space;X}f(x)log&space;q(x)&plus;\sum_{x\in&space;X}f(x)logf(x)" title="KL(f(x)||q(x))=\sum_{x\in X}f(x)log\frac{f(x)}{q(x)}=-\sum_{x\in X}f(x)log q(x)+\sum_{x\in X}f(x)logf(x)" /></a>

关于相对熵，有三条结论：

+ 完全相同的函数，交叉熵等于0
+ 相对熵越大，两个函数的差异就越大；交叉熵越小，两个函数的差异越小，
+ 对于概率分布或者概率密度函数，如果取值均大于0，相对熵可以度量两个随机分布的差异性。

如果`f(x)`是数据的真实概率分布，`q(x)`是由数据计算得到的概率分布。这样公式的前半部分是一个常数。当`q(x)`和`f(x)`越趋近相同，则相对熵越趋近于最小值（0）。

相对熵的前半部分，就是交叉熵。当相对熵趋近于最小值也就是交叉熵趋近于最小值。

<a><img src="https://latex.codecogs.com/gif.latex?H=-\sum_{x\in&space;X}f(x)log&space;q(x)" title="H=-\sum_{x\in X}f(x)log q(x)" /></a>

`f(x)`一般为数据的真实概率（比如样本的真实值），`q(x)`为计算的得到的概率分布（预测值），机器学习的目的就是希望`q(x)`尽可能的逼近`f(x)`，因此对`q(x)`的优化就等效于求交叉熵的最小值。（也等效于求最大似然估计）。所以，交叉熵可以被用来作损失函数。

![](https://github.com/Alex-lubo/CodeSnippets/blob/master/ML/CrossEntropy-MSE.png?raw=true)

对于分类问题的最优化，比如适合LR方法的问题，如上图所示，期望为1时，如果模型预测值为0时，误差函数为交叉熵时，惩罚项的值会增加到无穷大，而如果误差函数为MSE时，惩罚项的值也才等于1。这使得交叉熵更适合作为该类问题的损失函数。

TensorFlow针对分类问题，实现了四个交叉熵函数，分别是
+ tf.nn.sigmoid_cross_entropy_with_logits
> 标准的Cross Entropy算法实现。函数输入为logits和targets，targets就是正确的label值，logits为（W*X+b），**不需要**经过sigmoid。logits和labels必须为相同type和shape。比如判断100张图中**是否**包含5种动物，shape为[100,5]。需要知道，经过sigmoid函数后，预测值就限制在0~1之间。因此，此函数只在二分问题（Yes-No）的多目标任务（类别之间是独立的，不要求互斥）中使用，不适用于多分类问题（如识别手写数字）。
> 
> tensorflow中实现：
> ```
> x=logits, z=labels
> z * -log(sigmoid(x)) + (1 - z) * -log(1 - sigmoid(x))
> = z * -log(1 / (1 + exp(-x))) + (1 - z) * -log(exp(-x) / (1 + exp(-x)))
> = z * log(1 + exp(-x)) + (1 - z) * (-log(exp(-x)) + log(1 + exp(-x)))
> = z * log(1 + exp(-x)) + (1 - z) * (x + log(1 + exp(-x))
> = (1 - z) * x + log(1 + exp(-x))
> = x - x * z + log(1 + exp(-x))
> ```
> 为保证稳定性和防止溢出，实际实现时采用如下方程式来计算。
> ```
> max(x, 0) - x * z + log(1 + exp(-abs(x)))
> ```

+ tf.nn.softmax_cross_entropy_with_logits_v2
> 为了在多分类任务（label的可能值大于2）中使用交叉熵，就要使用Softmax算法。它是把所有的值用自然指数计算，求和后计算每个值占的比率，保证总和为1。softmax要求每个样本必须属于某个输出类别，并且样本要求覆盖所有的类别。函数会返回C个分量的概率向量，每个分量对应于一个输出类别的概率，如果各分量之和小于1，意味着有一些隐藏的类别，如果分量之和大于1表示每个样本可能属于多个类别。该函数适用于多分类问题。
> 该问题的输出需要C个，所以与sigmoid_cross_entropy_with_logits方法不同，它需要C个不同的权值组。每组对应一个可能的输出。
> 类别必须是互斥的，但是概率不是。label的每行表示为属于各类的概率，给类别概率总和为1（或者对类别做onehot encoding）。如果使用互斥标签，即每次只有一个类别为true时，使用sparse_softmax_cross_entropy_with_logits。
> logits和labels必须为相同的类型和shape，如[batch_size, num_classes]
> 为避免混淆，需传递命名参数。

+ tf.nn.sparse_softmax_cross_entropy_with_logits
> 是softmax_cross_entropy_with_logits的易用版本，除了输入参数不同，作用和算法实现都是一样的。logits和labels的shape可以不一样，一般logits的shape为[batch_size, num_classes],labes的shape为[batch_size],labels中的每个值的范围为(0, num_classes)，步长为1，如果步长大于1或者从1开始，某些label值会超过这个范围，代码会直接报错退出.

+ tf.nn.weighted_cross_entropy_with_logits
> 是sigmoid_cross_entropy_with_logits的拓展版，输入参数和实现和后者差不多，可以多支持一个pos_weight参数，目的是可以增加或者减小正样本在算Cross Entropy时的Loss。
> > tensorflow中实现：
> ```
> x=logits, z=labels
> cross-entropy的计算为：
> z * -log(sigmoid(x)) + (1 - z) * -log(1 - sigmoid(x))
> weighted_cross_entropy的计算公式为：
> z * -log(sigmoid(x))*pos_weight + (1 - z) * -log(1 - sigmoid(x))
> ...
> = (1-z)*x + (1 + (q -1)*z)*log(1 + exp(-x))
> ```
> set s=(1+(q-1)*z)为保证稳定性和防止溢出，实际实现时采用如下方程式来计算。
> ```
> max(-x, 0) + x * (1 - z) + s*(log(1 + exp(-abs(x)))
> ```

## 总结
以上是TensorFlow目前提供的有关Cross Entropy的函数实现，用户需要理解多目标和多分类的场景，根据业务需求（分类目标是否独立和互斥）来选择基于sigmoid或者softmax的实现。

