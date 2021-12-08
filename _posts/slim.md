# tf.variable_scope
- tf.variable_scope(self, name_or_scope, default_name=None, values=None, initializer=None, regularizer=None, caching_device=None, partitioner=None, custom_getter=None, reuse=None, dtype=None, use_resource=None, constraint=None, auxiliary_name_scope=True)

- 作用：A context manager for defining ops that creates variables (layers). This context manager **validates** that the (optional) `values` are from the same
graph, ensures that graph is the default graph, and **pushes a name scope and a
variable scope**.

- name_or_scope不为空，就用它；name_or_scope为空，default_name不为空，用default_name。**如果是后者**，在同一个scope中，default_name重复，则会在其后加一个`_N`。


说白了，下面第二行，Branch_0是name_or_scope而不是default_name。**name_or_scope不为空，就用它（两个都有，后者自动忽略）；name_or_scope为空，default_name不为空，用default_name**。
```
with tf.variable_scope(scope, 'Block35', [net], reuse=reuse):
    with tf.variable_scope('Branch_0'):
```


```
#import tensorflow
import tensorflow as tf
#open a variable scope named 'scope1'
with tf.variable_scope("scope1"):
    #open a nested scope name 'scope2'
    with tf.variable_scope("scope2"):
        #add a new variable to the graph
        var=tf.get_variable("variable1",[1])
#print the name of variable
print(var.name)

scope1/scope2/variable1:0
```
上面这种情况，nest（嵌套）。如果改为`with tf.variable_scope("here","scope1")`，结果就会变成`here/scope2/variable1:0`。


variable_scope可以常见新的变量，或者share已经存在的变量

- reuse有三个值：`True`, `None`, `tf.AUTO_REUSE`。`tf.AUTO_REUSE`，如果存在直接reuse，如果不存在就创建；`True`，仅仅reuse，前提是得存在，否则raise异常。**reuse是继承的**，如果一个scope是reuse，那么它的sub-scopes也全都是reuse的。
- initializer和regularizer，即在这个scope中，变量的初始化方式、正则化方式

# slim.conv2d
- `slim.conv2d(inputs, num_outputs, kernel_size, stride=1, padding='SAME', data_format=None, rate=1, activation_fn=<functi
on relu at 0x111ca9230>, normalizer_fn=None, normalizer_params=None, weights_initializer=<function _initializer at 0x115522ed8>, w
eights_regularizer=None, biases_initializer=<tensorflow.python.ops.init_ops.Zeros object at 0x115521710>, biases_regularizer=None,
 reuse=None, variables_collections=None, outputs_collections=None, trainable=True, scope=None)`
- padding默认SAME；activation_fn默认ReLU；stride默认1
