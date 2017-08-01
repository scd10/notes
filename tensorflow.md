<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

# Tensorflow学习笔记

石崇德
<shicd@istic.ac.cn>

---

## 1. Variable
**Variables** 用来储存和更新参数(parameters)。Variables在内存中存储tensor，必须显式初始化，可以存储到硬盘。

与Variables相关的主要是下面两个类：
* tf.Variable
* tf.train.Saver

**创建** Variable的时候需要传递一个任意维度、任意形状的tensor给Variable类的constructor，tensor可以是常量，也可以是随机值。

---

$x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}$
