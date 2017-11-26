# [Tensorflow for Deep Learning Research](https://web.stanford.edu/class/cs20si/syllabus.html)


- example
```
import tensorflow as tf
a = tf.constant([2, 2], name="a")
b = tf.constant([[0, 1], [2, 3]], name="b")
x = tf.add(a, b, name="add")
y = tf.mul(a, b, name="mul")
with tf.Session() as sess:
  writer = tf.summary.FileWriter('./graphs, sess.graph)
  x, y = sess.run([x, y])
  print x, y
# >> [5 8] [6 12]

tensorboard --logdir="./graphs" --port 6006
http://localhost:6006/
```
- tf.constant(value, dtype=None, shape=None, name='Const', verify_shape=False)

- tf.zeros(shape, dtype=tf.float32, name=None)
```
tf.zeros([2, 3], tf.int32) ==> [[0, 0, 0], [0, 0, 0]]
```

- tf.ones() like zeros()

- tf.fill(dims, value, name=None)
```
tf.fill([2, 3], 8) ==> [[8, 8, 8], [8, 8, 8]]
```