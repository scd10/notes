# Tensorflow学习笔记

SHI Chongde
<shicd@istic.ac.cn>

---

## 1. Variable

### 1.1 创建

**Variables** 用来储存和更新参数(parameters)。Variables在内存中存储tensor，必须显式初始化，可以存储到硬盘。

与Variables相关的主要是下面两个类：
* tf.Variable
* tf.train.Saver

**创建** Variable的时候需要传递一个任意维度、任意形状的tensor给Variable类的constructor，tensor可以是常量，也可以是随机值。

调用 **tf.Variable()** 会进行下面三项操作：
* 设一个参数保留变量；
* 调用 **tf.assign** 初始化
* 添加到graph

变量可以直接指定其 **设备** ,调用 **with tf.device(...)**

        with tf.device("/cpu:0"):
            v = tf.Variable(...)

        with tf.device("/gpu:0"):
            v = tf.Variable(...)

### 1.2 初始化
变量初始化必须显式的调用，可以使用 **tf.global_variables_initializer()** ：

        weights = tf.Variable(...)
        biases = tf.Variable(...)
        init_op = tf.global_varialbe_initializer()
        with tf.session() as sess:
            sess.run(init_op)

可以用其他变量进行初始化：

        weights = tf.Variable(...)
        w2 = tf.Variable(weights.initialized_value())
        w3 = tf.Variable(weights.initialized_value() * 2.0)

### 1.3 保存和读取模型变量

保存和读取模型最简单的方法是使用 **tf.train.Saver** 

Variable保存为二进制文件，实际上是一个变量名到tensor值的映射。创建 **Saver** 对象时，可以设定变量名字，缺省变量名为 **Variable.name** 。

要检查ckpt文件里的变量，可以用 **inspect_checkpoint** 库的 **print_tensors_in_checkpoint_file** 函数。

**保存**

        v1 = tf.Variable(..., name="v1")
        v2 = tf.Variable(..., name="v2")
        init_op = tf.global_variable_initializer()
        saver = tf.train.Saver()
        with tf.Session() as sess:
            sess.run(init_op)
            ..
            save_path = saver.save(sess, "/tmp/model.ckpt")
            print("Model saved in file: %s" % save_path)

**读取**

        v1 = ..
        v2 = ..
        saver = ..
        with tf.Session() as sess:
            saver.restore(sess, "/tmp/model.ckpt")
            ..

另外可以部分保存或者读取，比如你已经训练了一个5层的模型，现在想要训练一个6层的，可以把先前保存的5层读取出来进行进一步训练。

---

## 2. Mechanics 101 
