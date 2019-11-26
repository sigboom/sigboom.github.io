---
layout: post
title: TensorFlow学习之旅
subtitle: "尝试下现代工具"
date:  2019-11-09 12:00:00
author: "Doni Daniel"
header-img: "img/post-bg-prework.jpg"
catalog: true
tags: 
    - 环境配置
    - 技术
---


#TensorFlow学习之旅

##前言
由于想使用一些前沿的技术，自定了“基于神经网络的文件分类网站设计与实现”的毕业论文题目。想着使用TensorFlow应该可以实现我的需求，具体要根据学习情况进行修改。<br>
**我学习跟着的是[TensorFlow中文社区](http://www.tensorfly.cn/tfdoc/get_started/introduction.html)**

##尝试安装

###包安装
只是安装包的话就很简单了

```sh
$ conda create -n tensor2 python=3.7
(y)
$ conda activate tensor2
$ pip install update
$ pip install tensorflow
```

###源码安装(macOS)

####安装Bazel
```sh
$ brew tap bazelbuild/tap
$ brew install bazelbuild/tap/bazel
```
####安装pcre
[pcre文件地址](https://ftp.pcre.org/pub/pcre/)

####安装numpy

####创建pip包并安装(在git clone来的目录下执行)

```sh
bazel build -c opt //tensorflow/tools/pip_package:build_pip_package

```

###Doctor安装
```sh
$ docker run -it b.gcr.io/tensorflow/tensorflow
```

###第一个程序
```py

```

####发现
和Python一样，TensorFlow有1.X和2.0的区分，两个版本分别依据Python2.7和Python3.5+。



