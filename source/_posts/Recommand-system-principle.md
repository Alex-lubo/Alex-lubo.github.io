---
title: Recommand system principle
date: 2018-04-20 23:31:00
tags:
---

### 基本方法
* 协作式过滤
构建用户行为的模型来获得推荐内容。
有效方法：根据其他用户行为来构建模型。 基于多个用户的自动协作，过滤出类似偏好或行为的用户。
![](https://img.mubu.com/document_image/91ee4954-32cd-4ea8-9151-7600251bc8d6-381062.jpg)
* 基于内容的过滤
根据用户的行为来构造推荐内容。用过用户行为的历史信息，不依赖系统其他用户的行为。

* 混合方法
协作式过滤与基于内容过滤的结合， 以提高系统的效率（复杂性）。
将基于内容的过滤和协作式过滤的结果合并在一起，就有可能实现更准确的推荐内容。
混合方法也可用于解决从稀疏数据开始的协作式过滤问题 （冷启动）。方法是支持先对基于内容的过滤结果分配权重，然后随着可用用户数据集的成熟而将该权重转变为协作式过滤。


### 算法

#### 相似度计算

##### 闵可夫斯基距离

<a href="https://www.codecogs.com/eqnedit.php?latex=d(x,y)=(\sum_{k=1}^{n}\left&space;|&space;x_{k}-y_{k}&space;\right&space;|^{r})^{\frac{1}{r}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?d(x,y)=(\sum_{k=1}^{n}\left&space;|&space;x_{k}-y_{k}&space;\right&space;|^{r})^{\frac{1}{r}}" title="d(x,y)=(\sum_{k=1}^{n}\left | x_{k}-y_{k} \right |^{r})^{\frac{1}{r}}" /></a>

实际应用:

* 曼哈顿距离 (r = 1)
* 欧几里得（Euclidean）距离(r = 2)
* 极大距离(r = ∞)

特征：r越大，单个维度的差值对整体距离的影响就更大
优点： 理解简单，
缺点： 数据需完整， 用户评分原则不统一，影响推荐的准确性

##### pearson相关系数

<a href="https://www.codecogs.com/eqnedit.php?latex=r=\frac{\sum_{i=1}^{n}(x_{i}-\bar{x})(y_{i}-\bar{y})}{\sqrt{\sum_{i=1}^{n}(x_{i}-\bar{x}){}^{2}}\sqrt{\sum_{i=1}^{n}(y_{i}-\bar{y}){}^{2}}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?r=\frac{\sum_{i=1}^{n}(x_{i}-\bar{x})(y_{i}-\bar{y})}{\sqrt{\sum_{i=1}^{n}(x_{i}-\bar{x}){}^{2}}\sqrt{\sum_{i=1}^{n}(y_{i}-\bar{y}){}^{2}}}" title="r=\frac{\sum_{i=1}^{n}(x_{i}-\bar{x})(y_{i}-\bar{y})}{\sqrt{\sum_{i=1}^{n}(x_{i}-\bar{x}){}^{2}}\sqrt{\sum_{i=1}^{n}(y_{i}-\bar{y}){}^{2}}}" /></a>
尔逊相关系数用于衡量两个变量之间的相关性。如果r=1表示完全相关

##### 余弦相似度

<a href="https://www.codecogs.com/eqnedit.php?latex=cos(x,&space;y)=\frac{x\cdot&space;y}{\left&space;\|&space;x&space;\right&space;\|\times&space;\left&space;\|&space;y&space;\right&space;\|}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?cos(x,&space;y)=\frac{x\cdot&space;y}{\left&space;\|&space;x&space;\right&space;\|\times&space;\left&space;\|&space;y&space;\right&space;\|}" title="cos(x, y)=\frac{x\cdot y}{\left \| x \right \|\times \left \| y \right \|}" /></a>

弦相似度的计算中会略过这些非零值

* 相似度计算方法的选择
    + 如果数据存在“分数膨胀”问题，就使用皮尔逊相关系数
    + 如果数据比较“密集”，变量之间基本都存在公有值，且这些距离数据是非常重要的，那就使用欧几里得或曼哈顿距离。曼哈顿距离和欧几里得距离在数据完整的情况下会运作得非常好，如果数据比较稀疏，则要考虑使用余弦距离。
    + 如果数据是稀疏的，则使用余弦相似度

#### 集群算法
* k-means
* Adaptive Resonance Theory (ART) 系列
* Fuzzy C-means
* Expectation-Maximization（概率集群）
其它算法
* Bayesian Belief Nets，能够可视化为一个定向非循环图，其中的弧线表示变量之间关联的概率。
* Markov chains，它采用一种与 Bayesian Belief Nets 类似的方法，但将推荐问题视为顺序优化，而不是简单的预测。
* Rocchio classification（使用 Vector Space Model 开发），它利用项目相关性的反馈来提高推荐的准确

### 挑战

* 大量数据使‘集体智慧’生效的机会变得更加复杂。
   其它用户并没有表现出典型行为
   用户可利用推荐系统来选择一个产品然后放弃另外一个产品（因推荐系统而发生行为模型的扭曲变形）
* 可伸缩性：数据量逐渐增大，如何处理
* 保护隐私

### 开源推荐引擎

* SUGGESST - 明尼苏达大学的 George Karypis 开发
* esayrec - Research Studios Austria 在 Austrian Federal Ministry of Science and Research 的资助下开发和维护。使用 RESTful 接口，涵盖一个推荐系统所需的所有可能操作，包括物品购买、查看和评分。
* LensKit
* Crab
* MyMediaLite
* Recommenderlab
* milk - python的机器学习工具包， 专注于监督分类方法（SVM， k-NN）