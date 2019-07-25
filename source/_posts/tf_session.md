---
layout: post
title: 'session(会话)——TensorFlow的运行模型'
date: 2019-6-17
comments: true
tags:
   - tensorflow
breif: '一生之情在于谁'
---

> 本文介绍tensorflow中会话的使用方法及参数配置
<!-- more -->

## TensorFlow系统结构阐述

tesorflow分为前端和后端系统。前端系统负责编程模型，构造计算图，后端系统负责运行时的环境、计算图。
主要包括四个组件：Client，Distributed Master，Work Service和Kernel Implements。

（1）Client（客户端）：通过Client可以构建简单或者复杂的计算图，实现各种形式的模式设计。客户端会以Session为接口与后端的Distributed Runtime 中的控制器及多个Worker相连，并启动计算图的执行过程。
（2）Distributed Master：分布式控制器，和Session相连的Master（控制器），用于在分布式运行的环境中对一个大的计算图进行拆分。它会根据Session.run()函数的fetches参数，从计算图中反向遍历，找到计算图的子图之后会将该子图再次拆分成多个子图片段。最后，将这些子图片段传递到Work Service，由Work Service启动“子图片段”的执行过程。
（3）Work Service：与Session相连的Worker，每个Worker可以与Device Layer中的多个硬件设备相连。对于Master传递过来的“子图片段”，Worker都会按照计算图中节点的依赖关系及当前的设备，调用OP的Kernel实现完成OP操作。
（4）Kernel Implement：底层实现接口，是OP在某种硬件设备上的底层实现。
TensorFlow分为单机模式和分布式模式。单机模式下只有一个worker，分布式模式下有多个worker。单机模式下将client、master、worker放在一台机器上，分布式模式则视情况放在不同的机器中。

## 简单使用会话

Session类提供run()方法执行计算图，用户给run()方法中传入需要计算的节点和数据，来自动寻找所有需要计算的节点并按依赖顺序执行它们。另外还提供了extend()方法用于添加新的节点和边。

两种方法使用Session类：
（1）通过Session类的Session()构造函数创建会话类实例，通过close()关闭会话释放资源。但是此方法有个缺陷，若程序发生异常而退出时，却没有执行close()方法关闭会话，这会导致资源泄漏。当神经网络过于庞大或者参数量过多时，资源泄漏会是件麻烦的事。
（2）使用with/as上下文管理器，当上下文管理器退出时，会自动释放所有的资源。

下面是方法使用的代码：
```python

import tensorflow as tf

# 定义常量
x = tf.constant([2.0, 3.0], tf.float32, name='x')
y = tf.constant([3.0, 4.0], tf.float32, name='y')
# 计算两个常量之和
result = x + y
# 定义session
sess = tf.Session()
with sess.as_default():  # 将session设置成默认的
    # 两种相同计算方法
    print(sess.run(result))
    print(result.eval(session=sess))
    sess.close() # 关闭
# 使用InteractiveSession()方法将session已经设置成默认的，不需手动设置。
sess = tf.InteractiveSession()
print(sess.run(result))

# 第二种
with tf.Session() as sess:
    tf.global_variables_initializer().run()
    print(sess.run(result))

```