---
title: cmake 常用命令记录
date: 2023-02-26 21:28:06
tags: cmake
---

- aux_source_directory
    ```
    aux_source_directory(<dir> <variable>)
    ```
    将 dir 路径下的所有源文件名存入 variable 中，避免手动列出所有源文件  
    **注意**：若在 dir 目录添加了新的源文件而未修改 CMakeLists.txt，需要手动重新运行 cmake 来生成包含新文件的构建系统
    用法示例：

    目录结构
    ```
    ./test
    |
    +--- main.cpp
    |
    +--- Foo.cpp
    |
    +--- Foo.h
    ```
    CMakeLists.txt
    ```
    # CMake 最低版本号要求
    cmake_minimum_required (VERSION 2.8)

    project (test)

    # 查找当前目录下的所有源文件
    # 并将名称保存到 SRCS 变量
    aux_source_directory(. SRCS)

    add_executable(${PROJECT_NAME} ${SRCS})
    ```

- 

