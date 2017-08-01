# Tensorflow学习笔记

SHI Chongde
<shicd@istic.ac.cn>

---

## 一、 Variable

### 1. 创建

**Variables** 用来储存和更新参数(parameters)。Variables在内存中存储tensor，必须显式初始化，可以存储到硬盘。

与Variables相关的主要是下面两个类：
* tf.Variable
* tf.train.Saver

**创建** Variable的时候需要传递一个任意维度、任意形状的tensor给Variable类的constructor，tensor可以是常量，也可以是随机值。

调用**tf.Variable()**会进行下面三项操作：
* 设一个参数保留变量；
* 调用**tf.assign**初始化
* 添加到graph

变量可以直接指定其**设备**,调用**with tf.device(...)**

        with tf.device("/cpu:0"):
            v = tf.Variable(...)

        with tf.device("/gpu:0"):
            v = tf.Variable(...)
            
### 2. 初始化











---

