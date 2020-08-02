# Tensorflow 2.x 记录

## tf.data

* tf.data.Dataset.from_tensors() 
* tf.data.Dataset.from_tensor_slices()
* tf.data.Dateset.TFRecordDataset()
* Dateset.map()
* Dateset.batch()
* reduce
  * print(dataset.reduce(0, lambda state, value: state + value).numpy())
