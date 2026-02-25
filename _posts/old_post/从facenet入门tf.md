# TODO
- 此板块为细枝末节

# 相关技术
## protobuf
```
protobuf除了性能好（比xml），代码生成机制是主要吸引俺的地方
比如有个电子商务的系统（假设用C++实现），其中的模块A需要发送大量的订单信息给模块B，通讯的方式使用socket。
假设订单包括如下属性：
－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－
　　时间：time（用整数表示）
　　客户id：userid（用整数表示）
　　交易金额：price（用浮点数表示）
　　交易的描述：desc（用字符串表示）
－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－
　　如果使用protobuf实现，首先要写一个proto文件（不妨叫Order.proto），在该文件中添加一个名为"Order"的message结构，用来描述通讯协议中的结构化数据。该文件的内容大致如下：

－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－

message Order
{
  required int32 time = 1;
  required int32 userid = 2;
  required float price = 3;
  optional string desc = 4;
}

－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－


　　然后，使用protobuf内置的编译器编译 该proto。由于本例子的模块是C++，你可以通过protobuf编译器的命令行参数（看“这里 ”），让它生成C++语言的“订单包装类”。（一般来说，一个message结构会生成一个包装类）
　　然后你使用类似下面的代码来序列化/解析该订单包装类：


－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－

// 发送方

Order order;
order.set_time(XXXX);
order.set_userid(123);
order.set_price(100.0f);
order.set_desc("a test order");

string sOrder;
order.SerailzeToString(&sOrder);

// 然后调用某种socket的通讯库把序列化之后的字符串发送出去
// ......

－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－

// 接收方

string sOrder;
// 先通过网络通讯库接收到数据，存放到某字符串sOrder
// ......

Order order;
if(order.ParseFromString(sOrder))  // 解析该字符串
{
  cout << "userid:" << order.userid() << endl
          << "desc:" << order.desc() << endl;
}
else
{
  cerr << "parse error!" << endl;
}

－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－
```

# python
## os.path.expanduser

## parser.add_argument

```
add_argument()方法，用来指定程序需要接受的命令参数

定位参数：

parser.add_argument("echo",help="echo the string")

可选参数：

parser.add_argument("--verbosity", help="increase output verbosity")

在执行程序的时候，定位参数必选，可选参数可选。

add_argument()常用的参数：

dest：如果提供dest，例如dest="a"，那么可以通过args.a访问该参数

default：设置参数的默认值

action：参数出发的动作

store：保存参数，默认

store_const：保存一个被定义为参数规格一部分的值（常量），而不是一个来自参数解析而来的值。

store_ture/store_false：保存相应的布尔值

append：将值保存在一个列表中。

append_const：将一个定义在参数规格中的值（常量）保存在一个列表中。

count：参数出现的次数

parser.add_argument("-v", "--verbosity", action="count", default=0, help="increase output verbosity")

version：打印程序版本信息

type：把从命令行输入的结果转成设置的类型

choice：允许的参数值

parser.add_argument("-v", "--verbosity", type=int, choices=[0, 1, 2], help="increase output verbosity")

help：参数命令的介绍
```

# TF
## tf.Graph
- A default `Graph` is always registered, and accessible by calling
@{tf.get_default_graph}.
- Another typical usage involves the @{tf.Graph.as_default} context manager, which overrides the current default graph for the lifetime of the context.注意@{tf.Graph.as_default}会覆盖默认Graph，但要使用```with tf.Graph().as_default()```，其内部才有用

## tf.Session
- A `Session` object encapsulates the environment in which `Operation`
objects are executed, and `Tensor` objects are evaluated.

```python
# Build a graph.
a = tf.constant(5.0)
b = tf.constant(6.0)
c = a * b

# Launch the graph in a session.
sess = tf.Session()

# Evaluate the tensor `c`.
print(sess.run(c))
```

- A session may own resources, such as @{tf.Variable}, @{tf.QueueBase}, and @{tf.ReaderBase}. It is important to release these resources when they are no longer required. To do this, either invoke the @{tf.Session.close} method on the session, or use the session as a context manager（也就是with的方式）.

```python
# Using the `close()` method.
sess = tf.Session()
sess.run(...)
sess.close()

# Using the context manager.
with tf.Session() as sess:
  sess.run(...)
```

## OP
### tf.placeholder(dtype, shape=None, name=None)
- Inserts a placeholder for a tensor that will be always fed.**Important**: This tensor will produce an error if evaluated. Its value must be fed using the `feed_dict` optional argument to `Session.run()`,`Tensor.eval()`, or `Operation.run()`.


## Others
- @{tf.Operation} objects,which represent units of computation
- @{tf.Tensor} objects, which represent the units of data that flow between operations.
- tf.cond 有点像if-else
```python
z = tf.multiply(a, b)
result = tf.cond(x < y, lambda: tf.add(x, z), lambda: tf.square(y))
但是不管x<y是否成立，z已经必然运算过了。
```
