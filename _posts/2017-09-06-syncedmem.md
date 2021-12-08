---
layout: post
title:  "Caffe（二）之 Syncedmem"
date:   2017-09-06 12:00:00 +0800
categories: caffe
tag: caffe
---


# syncedmem.hpp & syncedmem.cpp

- other
    - 由于GPU知道主机内存的物理地址，因此可以通过“直接内存访问DMA（Direct Memory Access)技术来在GPU和主机之间复制数据。由于DMA在执行复制时无需CPU介入。

- 函数
    - CaffeMallocHost用于分配内存或者gpu显存
    - CaffeFreeHost用于释放内存或gpu显存，SyncedMemory类析构函数调用

- SyncedMemory类
    - 主要负责在GPU或者CPU上分配内存以及保持数据的同步作用，SyncedMemory类主要应用在BLOB类中。用在reshape时发现count>capacity重新分配空间。
    - enum SyncedHead { UNINITIALIZED, HEAD_AT_CPU, HEAD_AT_GPU, SYNCED }; 定义了data的状态信息（状态机）
    - to_cpu() 根据SyncedHead的实例head状态，，从代码中看到的逻辑如下
        - UNINITIALIZED，分配cpu空间或者gpu空间（根据用cpu还是gpu），HEAD_AT_CPU
        - HEAD_AT_GPU，若cpu无数据先分配cpu空间，然后拷贝gpu到cpu（caffe_gpu_memcpy），SYNCED
        - 其他两种情况，无操作
    - to_gpu() 逻辑如下
        - UNINITIALIZED， 分配gpu空间，HEAD_AT_GPU
        - HEAD_AT_CPU，若gpu无数据分配gpu空间，然后拷贝从cpu到gpu（caffe_gpu_memcpy），SYNCED
        - 其他两种无操作
    - cpu_data()，返回cpu\_data的指针，不管怎样，先to_cpu()，然后返回指针
    - gpu_data()，同上
    - set_cpu_data(Dtype* data)：释放掉原来的数据指针，指向data
    - set_gpu_data，同上

    - mutable_cpu_data()，获取可更改的data指针。
**注意**，cpu\_data 通过 return (const void*)cpu\_ptr\_获得不可更改的data，mutable\_cpu\_data 通过return cpu\_ptr\_获取可更改的data，其实原始的blob私有成员cpu\_ptr\_本就不是const的，原来是这么粗暴实现的。。。其他类似的函数同理。


- c++ 新知识
    - void即“无类型”，void\* 则为“无类型指针”，可以指向任何数据类型。
