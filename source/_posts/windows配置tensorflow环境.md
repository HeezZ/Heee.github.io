---
title: windows配置tensorflow环境
date: 2020-06-15 11:13:34
tags: 
    - 环境搭建
    - tensorflow
    - windows
---
<!-- toc -->
<!--more-->
### 安装显卡驱动
cuda 和 显卡驱动版本对应关系见[此链接](https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html)
![cuda and driver.png](https://note.youdao.com/yws/res/4804/WEBRESOURCEa7d44a5aed6256502b8e9db37bb85347)

### 安装vs
安装cuda时要用到vs，版本对应[地址](https://tensorflow.google.cn/install/source_windows)，图中MSVC即为VS版本：
![image.png](http://note.youdao.com/yws/res/4810/WEBRESOURCE54362f41e80aa9746012e0c14998f7c5)

### 安装cuda，cudnn
cuda，cudnn版本可见上图，CUDA直接一直下一步就行


CUDNN 解压后：
假如 cuda 安装目录：C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.0
1. 把 cudnn 解压包下的：cuda\bin\cudnn64_7.dll 复制到：cuda安装目录\bin
2. 把 cudnn 解压包下的：cuda\include\cudnn.h 复制到：cuda安装目录\include
3. 把 cudnn 解压包下的：cuda\lib\x64\cudnn.lib 复制到：cuda安装目录\lib\x64  


为防止出现未知错误，cudnn安装完成后，把 3 个路径加入 环境变量\用户变量\path 中：
- C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.0\bin
- C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.0\include
- C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.0\lib\x64

### 安装tensorflow
```
conda install tensorflow-gpu
```
可安装最新版tensorflow，若要指定版本
```
conda install tensorflow-gpu=version
```


### 总结

- 显卡 RTX2060 SUPER
- 显卡驱动：442.92
- python 3.7
- CUDA: 10.1
- CUDNN: cudnn-10.1-windows-x64-v7.6.0.64
- tensorflow: 2.1

可选：RTX2060 SUPER +  Win10+CUDA11+cudnn8.0+python3.7+tensorflow_gpu_2.1.0


