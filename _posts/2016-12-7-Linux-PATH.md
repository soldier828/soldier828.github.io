---
layout: post
title:  "Linux—PATH"
date:   2016-12-7 18:20:09 +0800
categories: linux
tag: linux
---
# virtualenv
基于python、pip的虚拟环境，可以为一个项目开发一个单独的环境，不影响外界的依赖。

```
virtualenv [OPTIONS] DEST_DIR
//生成DEST_DIR的虚拟环境，所有在这个环境中安装的pip包都会放在这个文件夹中
eg：lib/python2.7/site-packages/tensorflow

--no-site-packages    DEPRECATED. Retained only for backward compatibility. Not having access to global site-packages** is now the default behavior.
也就是说不能访问系统默认python库目录

--system-site-packages  Give the virtual environment access to the global site-packages.
将继承python库目录(比如/usr/lib/python2.7/site-packages)的所有库。 注意是可以访问，不是复制进DEST_DIR目录中。

在这个环境中
echo $PATH
/home/cs/upload/xbk/tensorflow/bin:$PATH(外部PATH)
因此说白了这个虚拟环境经安装到本虚拟环境的包的路径加入到PATH中，因此可以找到。而离开这个虚拟环境，PATH为外部PATH，无法找到正虚拟环境中能安装的包。
```

# conda
是一个类似于pip的包管理软件，隶属于Anaconda。Anaconda中不但有conda，还有pip

```
在外部 which python（未将Anaconda路径添加.bashrc）
/usr/bin/python

加入anaconda路径（export PATH="/home/cs/anaconda/bin:$PATH"）
/home/cs/anaconda/bin/python

对于python命令使用程序的路径同理
因此一旦我们 export PATH="/home/cs/anaconda/bin:$PATH"，
pip、python等等均是使用anaconda的pip和python
```

如果只想使用conda安装的包，而python、pip像使用系统默认的，那么export PATH="$PATH:/home/cs/anaconda/bin"，这样优先搜索系统默认路径，然后再搜索conda路径。
# Anaconda


# conda的虚拟环境安装tensorflow-gpu
conda install 默认的各种包都是安装官方比较稳定的版本，在anaconda社区可以安装比较新的、用户自己添加的新版本包。
conda 官方只能安装 tensorflow cpu版本，因此应该使用**conda的pip安装tensorflow gpu版本到conda的虚拟环境**。

# pip
Ubuntu/Linux 64-bit, GPU enabled, Python 2.7
export

```
TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow-0.10.0-cp27-none-linux_x86_64.whl

pip install --ignore-installed --upgrade $TF_BINARY_URL  
```

--ignore-installed  Ignore the installed packages (reinstalling instead).

--upgrade  Upgrade all specified packages to the newest available version.
