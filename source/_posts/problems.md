---
layout: post
title: "记录学习卷积网络的问题"
date: 2019-04-01 18:01
comments: true
tags: 
	- problem
---

## terminate called after throwing an instance of 'std::bad_alloc' what(): std::bad_alloc Aborted (core dumped)

在基于卷积神经网络训练mnist数据集时，设置的batch的值是50，当训练到一万九千多次时发生这种情况。主要原因是由于内存不足，可以尝试减少batch的值，再重新训练。

## AttributeError: module 'tensorflow._api.v1.train' has no attribute 'saver'
问题描述：
Traceback (most recent call last):
File "/home/xiaonan/python_workspace/tensorflow/tensorflow_learning/variables.py", line 14, in <module>
    saver = tf.train.saver()
AttributeError: module 'tensorflow._api.v1.train' has no attribute 'saver'

```
主要原因是tensorflow在1.0之后已经修改这段代码，将saver()函数修改成类，类需要大写。
saver = tf.train().Saver()
```