---
layout: post
title:  "caffe_python_network"
date:   2017-01-14 14:20:09 +0800
categories: caffe
tag: caffe
---

# python代码生成caffe网络的prototxt

## protobuf
Protocol buffers are Google's language-neutral, platform-neutral, extensible mechanism for serializing structured data – think XML, but smaller, faster, and simpler. You define how you want your data to be structured once, then you can use special generated source code to easily write and read your structured data to and from a variety of data streams and using a variety of languages.  
Protocol buffers currently supports generated code in **Java, Python, and C++**. With our new proto3 language version, you can also work with **Go, JavaNano, Ruby, and C#, with more languages to come**.

简而言之，protobuf是一种类似于数据结构的东西，平台、语言无关，使用者不需要关注在某个平台或者某个语言下的实现细节，只需定义好所需的数据结构的内容，不需关心实现细节。

## 所用到的protobuf语法
举例如下：关键字required（必须要定义，如下name和id），optional（根据需求选择定义与否，如email），repeated（如下PhoneNumber可以有多个，可定义多次。**注意** repeated的含义不是定义一个，自动扩展为相同的多个；而是可以写n次这个变量，才代表变量有n个数值）。

```
message Person {
  required string name = 1;
  required int32 id = 2;
  optional string email = 3;

  enum PhoneType {
    MOBILE = 0;
    HOME = 1;
    WO
  }

  message PhoneNumber {
    required string number = 1;
    optional PhoneType type = 2 [default = HOME];
  }

  repeated PhoneNumber phone = 4;
}
```

上面代码中的数字只代表变量的序号，不是赋值！声明protobuf结构体用message，结构体可嵌套。关于变量类型，可以使用常见的int32、string等，也可以使用自定义的枚举类型（如上PhoneType，在结构体PhoneNumber中使用）

## Caffe的具体应用
- protobuf定义文件：CAFFE_ROOT/src/caffe/proto/caffe.proto  
- 写法总原则：一切依据来自protobuf定义文件；repeated类型用list即[]表示；transform_param这样的数据结构message用dict即{}表示；

以下举例：
- Data层（已知的层，网上有大量示例）

  ```
  n.data, n.label = L.Data(batch_size=batch_size,
     backend=P.Data.LMDB, source=lmdb,mirror=train,
     transform_param=dict(scale=1./128,
     mean_value=[127.5,127.5,127.5]), ntop=2)
  ```

    - transform_param定义如下

      ```
      optional TransformationParameter transform_param = 36;
      ```

    - 然后找TransformationParameter的定义

      ```
      message TransformationParameter {
      optional float scale = 1 [default = 1];
      optional bool mirror = 2 [default = false];
      optional uint32 crop_size = 3 [default = 0];     
      optional string mean_file = 4;
      repeated float mean_value = 5;     
      optional bool force_color = 6 [default = false];
      optional bool force_gray = 7 [default = false];
      }
      ```
    - 由于transform_param本身是一个结构体，因此写成dict形式;其中mean_value为repeated，写成list形式

       ```
       transform_param=dict(scale=1./128,
       mean_value=[127.5,127.5,127.5])
       ```

    - 至于 batch_size、backend、source、ntop定义在DataParameter的顶层message内，故直接使用。
    - 注意这些变量的类型，注意匹配。

- Centerloss层（未知的层）
  - 首先找到该层总定义

    ```
    optional CenterLossParameter center_loss_param = 147;
    ```

  - 然后找到具体定义

    ```
    message CenterLossParameter {
    optional uint32 num_output = 1;
    optional FillerParameter center_filler = 2;
    optional int32 axis = 3 [default = 1];
    }
    ```

  - python代码

    ```
    center_loss_param=dict(num_output=10575,
    center_filler=dict(type='xavier'))

    n.center_loss = L.CenterLoss(n.fc5, n.label,
    param=dict(lr_mult=1, decay_mult=2),
    center_loss_param=center_loss_param,
    loss_weight=0.008)

    ```

    可以看到很多在CenterLossParameter定义里没有的变量，比如:n.fc5,n.label;param;loss_weight，它们分别bottom;param;center_loss_param;loss_weight。这些定义在LayerParameter中，属于每个层都有的公共定义。有兴趣可以看LayerParameter定义，这里不再一一举例。

完成：2017.1.12  
TODO:
- train test网络一次性生成在一个prototxt中，由于data层输出名字一致，目前python无法实现（变量重复赋值覆盖）。trade_off solution：train的data层和test的data层取不一样的名字，生成prototxt后再手动更改。
