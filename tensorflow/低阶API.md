# TencorFlow Core

TensorFlow core由2个独立部分组成：
* 构建计算图(tf.Graph)
* 运行计算图(tf.Session)

**计算图**是排列成一个图的tensorflow指令。图由2种类型对象组成：
* 操作 op
* 张量 tensor

**tf.Tensors不具有值，它们只是计算图中元素的handle。**