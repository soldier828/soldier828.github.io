# tf-record基本情况
- Tensorflow’s own binary storage format.
- takes up less space on disk, takes less time to copy and can be read much more efficiently from disk.
- [参考wiki](https://www.tensorflow.org/api_guides/python/reading_data)


# tf的读取数据的方式
## tf.data API（推荐）

## Feeding（最低效）
### 介绍
- TensorFlow's feed mechanism lets you inject data into any Tensor in a computation graph.（可以在任意节点feed数据）
- 用placeholder定义数据输入的节点，placeholder不用初始化且不hold数据，等着feed。


## QueueRunner（用tf.data替代）
### 介绍
- 历史原因产生，目前可cleanly被tf.data替代

### 一般流程
- 获取文件名list
- 将filenames传递给`tf.train.string_input_producer`函数， 该函数有shuffling、Max epoch功能。eopch功能通过加载几次filenames实现。shuffling也是打乱filenames，而如果此时filenames是record文件，那么只会打乱record文件顺序（这个函数之所以带shuffle，因为也可以直接输入图片的filenames，那么这个shuffle可能就有意义了）。这就涉及到怎么产生filenames：
    - 用python产生filenames
    - 用constant string Tensor，用`tf.train.match_filenames_once`等方法产生。
- `tf.train.string_input_producer`函数会产生一个FIFO队列，hold住filenames，直到有reader需要。queue runner单独开一个线程独立于reader，因此两者不会产生阻塞（如shuffle、或入栈）。
- 然后定义合适的reader，比如tf-record用`tf.TFRecordReader()`,csv用`tf.TextLineReader()`
- 注意以上已经产生了队列，因此我们写read的时候其实是按照一个image写的（一个处理流），因为tf都是链接，所以这样构成了图。
- batch：用处理流辅助以队列，构成batch。batch一般使用`tf.train`下面的函数，有`batch、batch_join、shuffle_batch、shuffle_batch_join`。注意这里才是batch的shuffle，`tf.train.string_input_producer`的shuffle是文件读取顺序的shuffle。batch的join形式，指多线程读取文件，获取更夸张的shuffle效果(文件之间)和并行效果（快）。不过一般用一个thread读数据+shuffle就足够了（也就是使用tf.train.batch和tf.train.shuffle_batch）。`tf.train.shuffle_batch`函数参数有num_threads，这个是读一个文件（因为没有join形式），但是用多线程读，会快一些。**总而言之**，一般用`tf.train.batch tf.train.shuffle_batch`，然后这两个函数参数`num_threads=1`就可以了（简单）。
- Another queue is needed to create batches from the examples. You can create the batch queue using tf.train.shuffle_batch([image, label], batch_size=10, capacity=30, num_threads=1, min_after_dequeue=10) where capacity is the maximum size of queue, min_after_dequeue is the minimum size of queue after dequeue, and num_threads is the number of threads enqueuing examples. Using more than one thread, it comes up with a faster reading. The first argument in a list of tensors which you want to create batches from.

## Preloaded data
- 仅用于小dataset，可以一次性载入内存。两种方法：
    - 用constant存储数据。（空间大一些，因为const在图中inline的，可能存储多次）
    - 用variable，初始化后永不改变（trainable=False）。（占的空间小一些）

# tf-record创建
- python的方式获取图片
- tf.python_io.TFRecordWriter(filename)的write写即可(tf.train.Example)
- tf.train.Example有Features域，写到对应key处即可。
