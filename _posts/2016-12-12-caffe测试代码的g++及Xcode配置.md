---
layout: post
title:  "caffe测试代码的g++及Xcode配置"
date:   2016-12-12 16:11:09 +0800
catalog: true
tags:
    - 编程
---

# g++ 头文件及库的添加

## 方法一：指定参数
- 在编译自己的项目时添加-L和-I编译选项
  - 添加头文件路径
    - -I  指明头文件的路径   （eg. -I /usr/local/Cellar/glog/include)。
    - 系统默认是/usr/include 和 /usr/local/include 以及 /usr/lib/gcc-lib/i386-linux/2.95.2/include
  - 添加库文件路径
    - -L  指定目录。link的时候，去找的目录。gcc会先从-L指定的目录去找，然后才查找默认路径。（告诉gcc,-l库名最可能在这个目录下）。
    - -l  指定文件（库名）。这个是**编译**的时候使用的库。-l紧接着就是库名，这里的库名不是真正的库文件名。比如说数学库，它的库名是m，他的库文件名是libm.so。
  - 指定宏：-D CPU_ONLY

综合举例如下：

```
g++ blob_demo.cpp -o app -D CPU_ONLY -I /path/to/hpp -L /path/to/lib -lcaffe -lglog
```

参见下面，也就是说，在程序发布（运行）时，同样要添加**LD_LIBRARY_PATH**才可以正常运行。

## 方法二：将库路径添加到环境变量

### CPLUS_INCLUDE_PATH
这个是c++的头文件path，默认是空的所以不要觉得奇怪。例子如下：

```
# boost
export CPLUS_INCLUDE_PATH=/usr/local/Cellar/boost/1.62.0/include/:$CPLUS_INCLUDE_PATH
```

这样写的好处在于：可以为这个caffe-test配置独立的编译环境，不用加入~/.zshrc全局中，不同的工程使用不同的配置文件（动态库同理）。

### LIBRARY_PATH
LIBRARY_PATH环境变量用于在**程序编译期间**查找动态链接库时指定查找共享库的路径

```
export LIBRARY_PATH=/usr/local/Cellar/glog/0.3.4_1/lib/:$LIBRARY_PATH
```

### LD_LIBRARY_PATH
LD_LIBRARY_PATH环境变量用于在**程序加载运行期间**查找动态链接库时指定除了系统默认路径之外的其他路径。

```
export LD_LIBRARY_PATH=$CAFFE_ROOT/build/lib/:$LD_LIBRARY_PATH
```  

### LIBRARY_PATH与LD_LIBRARY_PATH区别
举个例子，我们开发一个程序，经常会需要使用某个或某些动态链接库，为了保证程序的**可移植性**，**可以先将这些编译好的动态链接库放在自己指定的目录下**，然后按照上述方式将这些目录加入到LD_LIBRARY_PATH环境变量中，**这样自己的程序就可以动态链接后加载库文件运行了**。  
- 开发时，设置LIBRARY_PATH，以便gcc能够找到编译时需要的动态链接库。
- 发布（运行）时，设置LD_LIBRARY_PATH，以便程序加载运行时能够自动找到需要的动态链接库。

综合举例如下：

```
CAFFE_ROOT=/Users/liunan/workspace/git_clone/caffe

# CPLUS_INCLUDE_PATH
# boost
export CPLUS_INCLUDE_PATH=/usr/local/Cellar/boost/1.62.0/include/:$CPLUS_INCLUDE_PATH
# gflags
export CPLUS_INCLUDE_PATH=/usr/local/Cellar/gflags/2.1.2/include/:$CPLUS_INCLUDE_PATH
# glog
export CPLUS_INCLUDE_PATH=/usr/local/Cellar/glog/0.3.4_1/include/:$CPLUS_INCLUDE_PATH
# protobuf
export CPLUS_INCLUDE_PATH=/usr/local/Cellar/protobuf/3.1.0/include/:$CPLUS_INCLUDE_PATH
# caffe
export CPLUS_INCLUDE_PATH=$CAFFE_ROOT/include/:$CPLUS_INCLUDE_PATH
export CPLUS_INCLUDE_PATH=$CAFFE_ROOT/.build_release/src/:$CPLUS_INCLUDE_PATH

# LIBRARY_PATH
export LIBRARY_PATH=$CAFFE_ROOT/build/lib/:$LIBRARY_PATH
#export LIBRARY_PATH=/usr/local/Cellar/glog/0.3.4_1/lib/:$LIBRARY_PATH
export LD_LIBRARY_PATH=$CAFFE_ROOT/build/lib/:$LD_LIBRARY_PATH
```
```
g++ blob_demo.cpp -o app -D CPU_ONLY -lcaffe -lglog
```

# xcode（其他IDE）
- 头文件：在工程的配置文件中按照方法二设置Header_Search_Path。
- 库文件：把用到的lib拷贝放在工程下面的lib文件夹,在Library_Search_Path添加这个文件。这个路径应该同时指定了编译和运行时用到的动态库。
- 添加宏：在Preprocessor Macros设置CPU_ONLY=1

<img src="{{ site.url }}/pic/{{page.title}}/1.png" />

# include语句的两种语法
include语句有两种方式包含头文件，分别是使用双引号" "与左右尖括号< >。其区别是（对于不是使用完全文件路径名的）头文件的搜索顺序不同：  

- 使用双引号" "的头文件的搜索顺序：
  - 包含该#include语句的源文件所在目录；
  - 包含该#include语句的源文件的已经打开的头文件的逆序；
  - 编译选项-I所指定的目录
  - 环境变量INCLUDE所定义的目录
- 使用左右尖括号< >的头文件的搜索顺序：
  - 编译选项-I所指定的目录
  - 环境变量INCLUDE所定义的目录

# TODO: DIfferences bewteen .so & .dylib
