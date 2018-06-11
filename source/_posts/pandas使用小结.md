---
title: ‘pandas使用小结’
date: 2018-05-31 17:32:56
tags:
---

Pandas处理以下三种数据结构：
+ Series
> 相同数据类型的一维数组结构。

+ DataFrame
> 具有异构数据的二维数据结构。

+ Panel
> 具有异构数据的三维数据结构。

简单理解三者的关系就是： DataFrame是Series的容器，Panel是DataFrame的容器。

## read_csv
如iris_training.csv文件内数据格式为：
```
120,4,setosa,versicolor,virginica
6.4,2.8,5.6,2.2,2
5.0,2.3,3.3,1.0,1
4.9,2.5,4.5,1.7,2
4.9,3.1,1.5,0.1,0
5.7,3.8,1.7,0.3,0
...
```
使用pandas可以读入数据到DataFrame。
```
import pandas as pd

train_df = pd.read_csv('./iris_training.csv')
print train_df
```
输出为：
```
     120    4  setosa  versicolor  virginica
0    6.4  2.8     5.6         2.2          2
1    5.0  2.3     3.3         1.0          1
2    4.9  2.5     4.5         1.7          2
3    4.9  3.1     1.5         0.1          0
4    5.7  3.8     1.7         0.3          0
...
```
可以在读入时制定列名和列数据的类型：
```
column_names = ['sepel_length', 'sepel_width', 'petal_length','petal_width', 'label']
  type = {
    'sepel_length':np.float, 'sepel_width':np.float, 'petal_length':np.float,'petal_width':np.float, 'label':np.int
  }
train_df = pd.read_csv('./iris_training.csv'，names=column_names, dtype=data_type，header=0)
```
输出变为：
```
     sepel_length  sepel_width  petal_length  petal_width  label
0             6.4          2.8           5.6          2.2      2
1             5.0          2.3           3.3          1.0      1
2             4.9          2.5           4.5          1.7      2
3             4.9          3.1           1.5          0.1      0
4             5.7          3.8           1.7          0.3      0
...
```

