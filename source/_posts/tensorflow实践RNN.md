---
title: tensorflow实践：RNN训练word2vec
date: 2018-06-12 11:04:50
tags:
---

在进行NPL时，最细粒度的是词（Term），词语组成句子，句子形成段落、篇章、文档。要将自然语言交个计算机处理，因为计算机只支持数值，因此首先需要将语言数字化，或者说将语言嵌入到一个数学空间（word embedding）。而Word2Vec就是Word-Embedding的一种。

Word2Vec主要有两种方式
+ One-hot Representation
> 采用bag-of-words（BOW）模型,使用one-hot编码将一篇文章（或句子）标示成一个稀疏的向量,向量的长度为词典的长度，每个单词词为向量的一个分量，用1或0标示。
> + 优点：简洁，理解和编程实现都比较容易，配合最大熵、SVM、CRF
> + 缺点： 
+ Distribution Representation

在Word2Vec之前，一般采用bag-of-words（BOW）模型，使用one-hot编码将一篇文章标示成一个稀疏的向量，然后可以去计算向量之间的距离来衡量文章之间的相似性，进而实现对文章进行分类，或者通过tf/idf的统计来计算词与文本之间的相关性（搜索引擎）。使用one-hot编码有个缺陷，那就是无法表征词与词之间的关系。

而向量空间模型（Vector Representation）可以将词转为连续值（one-hot为离散）的向量表达。
向量空间模型大致分类2类：

+ 计数模型
> 对语料库中的相邻出现的词进行统计，然后将统计结果转为小而稠密的矩阵
+ 预测模型
> 根据一个词周围相邻的词来推测这个词以及它的空间向量。

Word2Vec是一种高效的从原始语料数据库中学习字词空间向量（Distributed Representation）的预测模型。主要分为两种模式：
+ CBOW
> 拿一个词语的上下文作为输入，来预测这个词语本身，比如江苏省的省会是__，推测目标字词南京。适合小型语料库。
+ Skip-Gram
> 与CBOW相反，用一个词语作为输入，来预测它周围的上下文。大型语料库中表现较好。

## Distribution Representation
word2vec，vec是什么？简答：用一个向量标示一个词。one-hot是方式之一。

专门训练Word2Vec的方法：Hierarchical softmax， negative sampling

## CBOW



## Skip-Gram
在网上看到的一篇介绍，很详细。https://www.leiphone.com/news/201706/PamWKpfRFEI42McI.html

更多资料参考这个网站：
http://mccormickml.com/2016/04/27/word2vec-resources/