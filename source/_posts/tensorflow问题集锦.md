---
title: tensorflow问题集锦
date: 2020-04-07 16:22:02
tags: tensorflow, keras, bugs
---

#  `SystemError: <built-in function len> returned a result with an error set`

## tf2.0 dataset api
使用tf2.0 dataset api时，模型结构中共享embedding超过一定次数,会出现出现了此错误。
https://github.com/tensorflow/tensorflow/issues/36642 这个issue属于同类问题。
问题出现的原因是dataset中使用了`dataset.batch(batch_size)`进行分批，默认情况下如果数据量不足一个batch，仍然会作为一个batch。此时模型训练会出问题。
要确保数据量是batch_size的倍数,或者设置`dataset.batch(batch_size, drop_remainder=True)`

## sequence api
自定义generator传递数据时
设定`tf.compat.v1.disable_eager_execution()`或者`model.compile(...,experimental_run_tf_function=False,..)`
