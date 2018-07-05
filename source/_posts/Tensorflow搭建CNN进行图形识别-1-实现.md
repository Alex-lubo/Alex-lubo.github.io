---
title: 基于Tensorflow搭建CNN进行图形识别(1)-实现
date: 2018-06-06 10:48:43
tags:
---

根据基础的介绍，对CNN的卷积，常见层（非线性，池化，全连接等），图像加载，图像操作和颜色空间等相关知识做了了解。下面就是要把这些知识串联用起来，来实现一个简单的图像目标识别和分类的CNN。CNN的网络架构有很多种，这里实现采用AlexNet（2012年ILSVRC冠军）的简化版。
网络架构：
<div>
  <div>Stanford Dogs DataSet</div>
  <div>Conv Layer1</div>
  <div>pool layer1</div>
  <div>Conv Layer2</text>
  <div>pool layer2</div>
  <div>Dropout</div>
  <div>Hidden</div>
  <div>fc</text>
</div>

可以基于AlexNet的基础，我们搭建一个简单的CNN：有2个卷积层，两个全连接层（其中一个softmax）。

过程如下：
因为需要对多层需要对权重值和偏置值进行初始化，先定义两个初始化函数：
``` python
def weight_initializer(shape):
  initial = tf.truncated_normal(shape, stddev=0.1)
  return tf.Variable(initial)

def bias_initializer(shape):
  initial = tf.constant(0.1, shape=shape)
  return tf.Variable(initial)
```
然后定义卷积操作和max_pool操作的函数。

卷积操作的设置长宽方向的步长均为1，即strides=[1，1,1,1]，如果要设置长宽方向的步长分别为2,3的话，strides就等于[1,2,3,1],[具体参考tensorflow的官方说明.](https://www.tensorflow.org/versions/master/api_docs/python/tf/nn/conv2d)
``` python
def conv2d(x, kernel):
  return tf.nn.conv2d(
    x, 
    kernel, 
    strides=[1, 1, 1, 1], 
    padding='SAME'
  )
```
池化操作采用max_pool，池化操作的核大小为2，步长为2.
``` python
def max_pool_2x2(x):
  return tf.nn.max_pool(
    x,
    ksize=[1, 2, 2, 1],
    strides=[1, 2, 2, 1],
    padding='SAME'
  )
```

**CNN网络结构定义**
样本参数
``` python
x_width = 28    # 输入图片的宽度
x_height = 28   # 输入图片的高度
x_channel = 1   # 输入图片的颜色通道数
class_num = 10  # 样本种类
```
输入参数
``` python
x = tf.placeholder(tf.float32, [None, x_width, x_height, x_channel])
y_ = tf.placeholder(tf.float32, [None, class_num])
keep_prob = tf.placeholder(tf.float32)
```
定义卷积层
``` python
with tf.name_scope('Converlution_Layer_1'):
  conv1_k_size = 5      # 卷积核大小 5x5
  conv1_filter_num = 32 # 滤波器个数，也就是想要抽取的特征数
  conv1_channel = x_channel
  W_conv1 = weight_initializer([conv1_k_size, conv1_k_size, conv1_channel, conv1_filter_num])
  b_conv1 = bias_initializer([conv1_filter_num])
  h_conv1 = tf.nn.relu(conv2d(x, W_conv1) + b_conv1)     # 隐藏层
  pool_conv1 = max_pool_2x2(h_conv1)                     # pool层
  map_size = x_width/2

with tf.name_scope('Converlution_Layer_2'):
  conv2_k_size = 5
  conv2_filter_num = 64
  conv2_channel = conv1_filter_num
  W_conv2 = weight_initializer([conv2_k_size, conv2_k_size, conv2_channel, conv2_filter_num])
  b_conv2 = bias_initializer([conv2_filter_num])
  h_conv2 = tf.nn.relu(conv2d(pool_conv1, W_conv2) + b_conv2)
  pool_conv2 = max_pool_2x2(h_conv2)
  map_size = map_size/2
```
定义全连接层
``` python
with tf.name_scope('Fully_Connected_1'):
  nodes_num_fc1 = 1024    # 隐含节点数
  features_size_fc1 = map_size * map_size * conv2_filter_num
  W_fc1 = weight_initializer([features_size_fc1, nodes_num_fc1])
  b_fc1 = bias_initializer([nodes_num_fc1])
  pool2_flat = tf.reshape(pool_conv2, [-1, features_size_fc1])
  h_fc1 = tf.nn.relu(tf.matmul(pool2_flat, W_fc1) + b_fc1)
  drop_fc1 = tf.nn.dropout(h_fc1, keep_prob)

with tf.name_scope('Softmax_Regression'):
  W_fc2 = weight_initializer([nodes_num_fc1, class_num])
  b_fc2 = bias_initializer([class_num])
  y_predict = tf.nn.softmax(tf.matmul(drop_fc1, W_fc2) + b_fc2)
```

定义训练方法：
``` python
learning_rate = 1e-3
training_steps = 4000
loss_corss_entropy = tf.reduce_mean(-tf.reduce_sum(y_ * tf.log(y_predict), reduction_indices=[1]))
train_step = tf.train.AdamOptimizer(learning_rate).minimize(loss_corss_entropy)
```
定义评估方法：
``` python
prediction = tf.equal(tf.argmax(y_predict, 1), tf.argmax(y_, 1))
accuracy = tf.reduce_mean(tf.cast(prediction, tf.float32))
```
最后是训练和效果评估过程
``` python
# training
tf.global_variables_initializer().run()
for i in range(training_steps):
  train_x, train_y = input_train()
  if i % 100 == 0:
    print(
      'step %d accuracy is %g' %(i, accuracy.eval(feed_dict={x: train_x, y_: train_y, keep_prob: 1.}))
    )
  train_step.run(feed_dict={x: train_x, y_: train_y, keep_prob:0.75})

# evaluation 
test_x, test_y = input_test()
print('test accuracy is %g' % accuracy.eval(feed_dict={x: test_x, y_: test_y, keep_prob: 1.0}))
```

可以使用手写数据mnist来输入进行训练和评估。
``` python
from tensorflow.examples.tutorials.mnist import input_data
mnist = input_data.read_data_sets('MNIST_data',one_hot=True)
def input_train():
  """
  add your input procedure to import training data
  """
  batch = mnist.train.next_batch(50)
  x = np.reshape(batch[0], [-1, 28, 28, 1])
  return x, batch[1]

def input_test():
  """
  add your input procedure to import testing data
  """
  test_x = np.reshape(mnist.test.images, [-1, 28, 28, 1])
  return test_x, mnist.test.labels
```

当然也可在input_train和input_test中添加自己的数据输入资料来训练。
