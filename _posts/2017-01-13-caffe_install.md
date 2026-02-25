---
layout: post
title:  "caffe_install"
date:   2017-01-13 14:20:09 +0800
catalog: true
tags:
    - 编程
---
# INSTALL CAFEE ALL
NVIDIA driver & CUDA & cuDNN & Caffe Installation

## summary
- NVIDIA driver 与 CUDA 的关系：两者存在一定的匹配关系，驱动更新相对于CUDA频繁很多，因此一些版本的驱动对应一个某个CUDA版本。例如，CUDA8.0与CUDA8.0放出之后更新的所有NVIDIA驱动应该都是相互匹配的，直到CUDA8.5可能又会对应另外一些NVIDIA驱动。注意：也许某些NVIDIA驱动同时可以对应CUDA7.5和CUDA8.0（未考证）。总之，一般**安装比较新的NVIDIA驱动和最新CUDA是不会有匹配问题的**。
- NVIDIA driver 与 CUDA 最好下载.run本地文件。一般CUDA会内置一个NVIDIA driver版本，但相对于最新的NVIDIA driver较旧，因此安装CUDA注意不要选择安装老版本NVIDIA driver（只有用.run安装方式可以有这个选项）。
- 目前尝试的比较稳定容易安装的搭配是 **Ubuntu14.04+NVIDIA375+CUDA8.0+CUDNN5**
- 个人认为：如果没有特殊的需求（或者安装caffe时opencv出问题）不要手动编译opencv，直接apt-get安装即可。
- 推荐Anaconda管理python包（即使用conda install），opencv的python版本也可轻易安装。若安装速度慢可使用国内镜像。
- 安装的这些工具最好都在默认位置，如果更改安装位置，请在相应配置文件中记得修改。

## NVIDIA driver
0.  确保BIOS中优先显示模式为集显（onboard），确保DVI或VGA插在机箱核显位置。
1.  英伟达官网下载本地显卡支持版本的.run文件
2.  sudo chmod +x NVIDIA*.run
3.  注销图形界面，并进入终端模式

    ```
    sudo service lightdm stop
    sudo ./NVIDIA*.run  // 一直yes，直到结束
    nvidia-smi  //如果可以显示显卡版本以及用量信息 安装成功
    sudo reboot
    ```

4.  上一步重启之后，进BIOS设置优先显示模式为独显（offboard或GPU），关机。把DVI插到显卡上，开机（注意调节显示器显示模式以适应DVI，有的显示器无需设置）。如果能登录图形界面则成功。**如果卡在登录界面循环**，则检查BIOS设置优先显示模式是否为独显以及是否DVI插到显卡上。

## CUDA
1.  英伟达官网下载本地.run文件
2.  安装

    ```
    sudo chmod +x cuda*.run
    sudo ./cuda*.run //注意不要安装cuda带的驱动；注意提示建立软连接；注意安装完会提示加入cuda系统路径，按提示操作,记得source ~/.bashrc
    ```
3.  nvcc --version 若能显示cuda信息则安装成功；若提示nvcc未安装，则**检查是否加入cuda系统路径**，**后续编译caffe可能会出现cuda相关的lib找不到，这种情况有可能是加入cuda系统路径未生效，则尝试向/etc/ld.so.conf加入cuda路径，sudo ldconfig生效**。

## cuDNN
1.  下载cuDNN，并解压
2.  复制cuDNN头文件及lib到系统目录

    ```
    sudo cp ./include/*.h /usr/local/include/
    sudo cp ./lib64/lib* /usr/local/lib/
    cd /usr/local/lib
    sudo ln -sf libcudnn.so.5.0.4 libcudnn.so.5 //更改相应版本号
    sudo ln -sf libcudnn.so.5 libcudnn.so
    sudo ldconfig
    ```

## Anaconda
1.  官网下载.sh文件
2.  bash anaconda.sh
3.  注意提示，将anaconda路径加入系统路径

## Caffe
1.  git clone caffe
2.  针对Ubuntu14.04 安装以下

    ```
    sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
    sudo apt-get install --no-install-recommends libboost-all-dev
    sudo apt-get install libatlas-base-dev
    sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev
    ```

3.  配置caffe的Makefile.config，以下为注意的地方

    ```
    # 修改python位置为Anaconda
    #PYTHON_INCLUDE := /usr/include/python2.7 \
    #               /usr/lib/python2.7/dist-packages/numpy/core/include
    # Anaconda Python distribution is quite popular. Include path:
    # Verify anaconda location, sometimes it's in root.
    ANACONDA_HOME := $(HOME)/anaconda
    PYTHON_INCLUDE := $(ANACONDA_HOME)/include \
                 $(ANACONDA_HOME)/include/python2.7 \
                 $(ANACONDA_HOME)/lib/python2.7/site-packages/numpy/core/include \
    ```

    ```
    # We need to be able to find libpythonX.X.so or .dylib.
    #PYTHON_LIB := /usr/lib
    PYTHON_LIB := $(ANACONDA_HOME)/lib
    ```

4.  编译caffe(cd CAFFE_ROOT)
    ```
    sudo make all -j
    sudo make test -j
    sudo make runtest -j
    ```

5.  编译pycaffe

    ```
    cd python
    for req in $(cat requirements.txt); do conda install $req; done
    ```
    ```
    cd ..
    sudo make pycaffe
    ```

6.  注意
  apt-get 安装的protobuf-compile可能会与conda安装的protobuf的编译器冲突。如果严格按照4、5的顺序第一次安装不会出现这个问题；如果按照4、5的步骤编译成功，之后再次编译新的caffe在第4步会出现上述问题。  
  解决办法： conda remove libprotobuf，然后执行第4步，成功后conda install protobuf(自动安装libprotobuf)，再make pycaffe。
