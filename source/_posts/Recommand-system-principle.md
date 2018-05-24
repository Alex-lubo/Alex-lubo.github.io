---
title: Recommand system principle
date: 2018-04-20 23:31:00
tags:
---

### 简述
最近打开头条看了几条程序员相关的文章，然后发现后面每天打开头条的推荐，有很多都是程序员相关的内容，然后我还点击了其中的一些。在jd上卖了几罐奶粉，然后下次登录时，发现旁边有不少奶粉的推荐，而且价格比上次买的还便宜，然后就点了进去看了看...
嗯，这体验还是很cool的，尤其对我这种有轻微选择困难的人来说还是很方便的，至少不用在搜索栏打字搜索然后过滤筛选了。作为一名工程师，不禁想这套机制是如何运作的。有人说这就是推荐系统啊，那么推荐系统是怎么回事呢？网上搜罗了一番，将推荐系统相关的原理大概记录一下。

推荐系统的核心问题是要知道用户可能对什么感兴趣。那么如何知道呢?有个生活经验：你去一家成衣店买衣服，售货员会给你推荐，他会说某件衣服今年很流行，或者他看你看了几件裤子都很fashion，那么他可能会给你看店里其他类似款式的裤子拿给你看，或者看你的衣着打扮，推测你感兴趣的款式...透过这一生活场景，可以看到推荐系统的基本逻辑：要么复制别人的选择推荐给你，要么根据历史行为推测你感兴趣的内容。这个逻辑透过数学建模和算法实现就形成了推荐系统。

### 算法

#### 相似度计算

##### 闵可夫斯基距离

<a href="https://www.codecogs.com/eqnedit.php?latex=d(x,y)=(\sum_{k=1}^{n}\left&space;|&space;x_{k}-y_{k}&space;\right&space;|^{r})^{\frac{1}{r}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?d(x,y)=(\sum_{k=1}^{n}\left&space;|&space;x_{k}-y_{k}&space;\right&space;|^{r})^{\frac{1}{r}}" title="d(x,y)=(\sum_{k=1}^{n}\left | x_{k}-y_{k} \right |^{r})^{\frac{1}{r}}" /></a> (1)

实际应用:

* 曼哈顿距离 (r = 1)
* 欧几里得（Euclidean）距离(r = 2)
* 极大距离(r = ∞)

特征：r越大，单个维度的差值对整体距离的影响就更大
优点： 理解简单，
缺点： 数据需完整， 用户评分原则不统一，影响推荐的准确性

##### pearson相关系数

<a href="https://www.codecogs.com/eqnedit.php?latex=r=\frac{\sum_{i=1}^{n}(x_{i}-\bar{x})(y_{i}-\bar{y})}{\sqrt{\sum_{i=1}^{n}(x_{i}-\bar{x}){}^{2}}\sqrt{\sum_{i=1}^{n}(y_{i}-\bar{y}){}^{2}}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?r=\frac{\sum_{i=1}^{n}(x_{i}-\bar{x})(y_{i}-\bar{y})}{\sqrt{\sum_{i=1}^{n}(x_{i}-\bar{x}){}^{2}}\sqrt{\sum_{i=1}^{n}(y_{i}-\bar{y}){}^{2}}}" title="r=\frac{\sum_{i=1}^{n}(x_{i}-\bar{x})(y_{i}-\bar{y})}{\sqrt{\sum_{i=1}^{n}(x_{i}-\bar{x}){}^{2}}\sqrt{\sum_{i=1}^{n}(y_{i}-\bar{y}){}^{2}}}" /></a>    (2)

尔逊相关系数用于衡量两个变量之间的相关性。如果r=1表示完全相关
上面的公式，在实际计算时必须对数据做多次遍历。可以使用另外一个公式来计算pearson系数的近似值。

<a href="https://www.codecogs.com/eqnedit.php?latex=r=\frac{\sum_{i=1}^{n}x_{i}y_{i}-\frac{\sum_{i=1}^{n}x_{i}\sum_{i=1}^{n}y_{i}}{n}}{\sqrt{\sum_{i=1}^{n}x_{i}^{2}-\frac{(\sum_{i=1}^{n}x_{i})^{2}}{n}}\sqrt{\sum_{i=1}^{n}x_{i}^{2}-\frac{(\sum_{i=1}^{n}x_{i})^{2}}{n}}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?r=\frac{\sum_{i=1}^{n}x_{i}y_{i}-\frac{\sum_{i=1}^{n}x_{i}\sum_{i=1}^{n}y_{i}}{n}}{\sqrt{\sum_{i=1}^{n}x_{i}^{2}-\frac{(\sum_{i=1}^{n}x_{i})^{2}}{n}}\sqrt{\sum_{i=1}^{n}x_{i}^{2}-\frac{(\sum_{i=1}^{n}x_{i})^{2}}{n}}}" title="r=\frac{\sum_{i=1}^{n}x_{i}y_{i}-\frac{\sum_{i=1}^{n}x_{i}\sum_{i=1}^{n}y_{i}}{n}}{\sqrt{\sum_{i=1}^{n}x_{i}^{2}-\frac{(\sum_{i=1}^{n}x_{i})^{2}}{n}}\sqrt{\sum_{i=1}^{n}x_{i}^{2}-\frac{(\sum_{i=1}^{n}x_{i})^{2}}{n}}}" /></a>  (3)

这个公式虽然看起来更加复杂，而且其计算结果会不太稳定，有一定误差存在，但它最大的优点是，用代码实现的时候可以只遍历一次数据

##### 余弦相似度

<a href="https://www.codecogs.com/eqnedit.php?latex=cos(x,&space;y)=\frac{x\cdot&space;y}{\left&space;\|&space;x&space;\right&space;\|\times&space;\left&space;\|&space;y&space;\right&space;\|}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?cos(x,&space;y)=\frac{x\cdot&space;y}{\left&space;\|&space;x&space;\right&space;\|\times&space;\left&space;\|&space;y&space;\right&space;\|}" title="cos(x, y)=\frac{x\cdot y}{\left \| x \right \|\times \left \| y \right \|}" /></a>   (4)

余弦相似度的计算中会略过这些非零值。为了解决“分数膨胀”的问题，可以采用修正的余弦相似度。就是用户评价中减去其评价的平均值。

<a href="https://www.codecogs.com/eqnedit.php?latex=s(i,j)=\frac{\sum_{u\in&space;U}(R_{u,i}-\bar{R_{u}})(R_{u,j}-\bar{R_{u}})))}{\sqrt{\sum_{u\in&space;U}(R_{u,i}-\bar{R_{u}})^{2}}\sqrt{\sum_{u\in&space;U}(R_{u,i}-\bar{R_{u}})^{2}}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?s(i,j)=\frac{\sum_{u\in&space;U}(R_{u,i}-\bar{R_{u}})(R_{u,j}-\bar{R_{u}})))}{\sqrt{\sum_{u\in&space;U}(R_{u,i}-\bar{R_{u}})^{2}}\sqrt{\sum_{u\in&space;U}(R_{u,i}-\bar{R_{u}})^{2}}}" title="s(i,j)=\frac{\sum_{u\in U}(R_{u,i}-\bar{R_{u}})(R_{u,j}-\bar{R_{u}})))}{\sqrt{\sum_{u\in U}(R_{u,i}-\bar{R_{u}})^{2}}\sqrt{\sum_{u\in U}(R_{u,i}-\bar{R_{u}})^{2}}}" /></a>    (5)

##### 相似度计算方法的选择
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

### 基本方法
#### 1)协同过滤

构建用户行为的模型来获得推荐内容。简单来说，就是要推荐物品给你，首先找一个和你类似的用户，然后把他喜欢的东西推荐给你。

有效场景：根据其他用户行为来构建模型。 基于多个用户的自动协作，过滤出类似偏好或行为的用户。
缺陷： 让流行的物品更流行，冷门的物品更无人问津。
![](https://img.mubu.com/document_image/91ee4954-32cd-4ea8-9151-7600251bc8d6-381062.jpg)

+ 显式评价

> 用户明确给出物品的评价。比如youtube上的“喜欢”“不喜欢”按钮以及亚马逊的星级系统。
>
> 显式评价存在的问题：
>
>   * 懒，不愿评价物品
>   * 存偏见，评价不客观
>   * 不会跟新评论
> 
+ 隐式评价

> 无须用户明确给出对物品的评价，而是通过观察他们的行为来获得偏好信息。比如用户操作记录。
>
> 隐式评价存在的问题：
>
>   * 用户操作随意性，不能简单根据历史记录就能准确捕捉用户喜好。需要仔细甄别，否则会给出啼笑皆非的推荐。

分类：
+ **基于用户的协同过滤**

也称内存型协同过滤，因为需要将所有的评价数据都保存在内存中来进行推荐。
通过计算用户之间的距离找出最相似的用户，并将他评价过的物品推荐给目标用户。

过程简单表述如下：

    1. 要为用户A推荐物品，首先计算评价数据库中其他用户与A的相似度。
    2. 找出k个相似度最高的用户，以此为基础来对A进行物品推荐。比如找出了三个用户及其pearson系数B（0.8），C（0.7），D（0.5)
    3. 根据相似度来决定其在对某件物品推荐时的影响因子，B（0.4），C（0.35），D（0.25），
    4. 最后根据各自的影响因子来计算某件物品的推荐指数，根据推荐指数的高低来对用户A进行推荐。比如B，C，D对某件物品的评分分别是3,5，4，那么最后的推荐指数=（0.4*3）+（0.35*5）+（0.25*4）=3.95
    5. 根据推荐指数来决定推荐的等级。

弊端：
1. 扩展性：计算量与用户量成正比关系。算法在几千个用户的情况下能够工作的很好，但达到一定数量级时会出现瓶颈。
2. 稀疏性：对于个人用户来说，用户评价过的物品数量要远远小于物品的数量，而且不同用户评价商品完全不一样。数据的稀疏性导致很难找到相似的用户。

+ **基于物品的系统过滤**

也称基于模型的协同过滤，不需要保存所有的评价数据，而是通过构建一个物品相似度模型来做推荐。

过程简单表述如下：

    1. 根据用户评价，一般采用修正的余弦相似度公式计算物品的相似度，可以获得一个物品之间相似度的矩阵；
    2. 根据物品相似度，计算用户对某物品可能的评分。如物品A用户未打分，那么结合用户对其他物品的评价，可以计算他对A可能的评分，计算方法采用公式（6）。
    3. 根据预测评分给出推荐内容。
<a href="https://www.codecogs.com/eqnedit.php?latex=p(u,i)=\frac{\sum_{N\in&space;similarTo(i)}(S_{i,N}\times&space;R_{u,N})}{\sum_{N\in&space;similarTo(i)}(\left&space;|S_{i,N}\right&space;|)}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?p(u,i)=\frac{\sum_{N\in&space;similarTo(i)}(S_{i,N}\times&space;R_{u,N})}{\sum_{N\in&space;similarTo(i)}(\left&space;|S_{i,N}\right&space;|)}" title="p(u,i)=\frac{\sum_{N\in similarTo(i)}(S_{i,N}\times R_{u,N})}{\sum_{N\in similarTo(i)}(\left |S_{i,N}\right |)}" /></a>  (6)

其中p(u,i)标示用户u对物品i的评分。N表示用户u打过分的物品集合，Si,N标示物品与N中物品相似度矩阵，Ru,N标示用户u对物品N的评分组成的矩阵。公式的计算效果更佳，对物品的评价分值做标准化处理使之介于-1和1之间。

关于基于物品的协同过滤算法还有[Slope One算法](http://www.daniel-lemire.com/fr/abstracts/SDM2005.html)。

优点：
1. 扩展性好，运算速度快，占用内存少


#### 2)基于物品特征的过滤

基于物品本身的特征来进行推荐。本质是做分类。
物品特征的推荐首先需要为物品选择合适的特征形成特征向量，然后根据这些特征向量来计算彼此的相似程度，对物品进行分类。推荐时根据用户浏览的物品，给出相同分类里面最高的物品作为推荐。

#### 3)混合方法

协作式过滤与基于内容过滤的结合， 以提高系统的效率（复杂性）。
将基于内容的过滤和协作式过滤的结果合并在一起，就有可能实现更准确的推荐内容。
混合方法也可用于解决从稀疏数据开始的协作式过滤问题 （冷启动）。方法是支持先对基于内容的过滤结果分配权重，然后随着可用用户数据集的成熟而将该权重转变为协作式过滤。


### 挑战

* 大量数据使‘集体智慧’生效的机会变得更加复杂。
   其它用户并没有表现出典型行为
   用户可利用推荐系统来选择一个产品然后放弃另外一个产品（因推荐系统而发生行为模型的扭曲变形）
* 可伸缩性：数据量逐渐增大，如何处理
* 保护隐私
* ‘信息胶囊’问题

### 开源推荐引擎

* SUGGESST - 明尼苏达大学的 George Karypis 开发
* esayrec - Research Studios Austria 在 Austrian Federal Ministry of Science and Research 的资助下开发和维护。使用 RESTful 接口，涵盖一个推荐系统所需的所有可能操作，包括物品购买、查看和评分。
* LensKit
* Crab
* MyMediaLite
* Recommenderlab
* milk - python的机器学习工具包， 专注于监督分类方法（SVM， k-NN）