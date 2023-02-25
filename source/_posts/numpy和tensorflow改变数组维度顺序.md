---
title: numpy和tensorflow改变数组维度顺序
date: 2020-07-10 14:04:48
tags: 
    - numpy
    - tensorflow
---

- 适用场景：将一堆二维张量拼接成三维张量的时候，默认的Chanel维度在首位；然而在TensorFlow中张量的默认Channel维度在末尾。因此有时需要将变量模式从NCHW转换为NHWC以匹配格式。

- 以CHW改成HWC为例
    - numpy  
        np.transpose(1,2,0)
    - tensorflow  
        tf.transpose(1,2,0)