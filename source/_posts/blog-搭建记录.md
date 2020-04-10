---
title: blog 搭建记录
date: 2020-04-09 15:26:29
tags: 环境搭建
<!-- toc: true
description: 此blog搭建过程的记录 -->
---
<!-- toc -->
<!--more-->

- 环境: Windows

#### 安装Node.js 和 git
- [Node.js](https://nodejs.org/en/): 一路默认安装即可
- [git](https://git-scm.com/downloads): 选择windows版一路默认安装即可  

#### 安装hexo
- 打开cmd查看node和npm（node包管理工具）是否安装成功
    ```shell
    hexo -v
    npm -v
    ```
    若打印出版本信息则安装成功
- npm因为墙等原因可能安装包很慢，安装国内的cnpm
    ```shell
    npm install -g cnpm --registry=https://registry.npm.taobao.org
    ```
- 安装hexo
    ```shell
    cnpm install -g hexo-cli
    ```
- 验证hexo安装
    ```shell
    hexo -v
    ```

#### 建立本地博客
- 创建博客文件夹
    ```
    cd Desktop  //我把文件夹放到桌面了
    mkdir blog
    ```
- 将文件夹初始化为hexo管理的文件夹
    ```
    hexo init
    ```
    运行该命令后可在blog看到hexo生成的文件目录
- 运行
    ```
    hexo s
    ```
    根据输出的地址即可在浏览器中打开博客
- 常用命令
    ```
    hexo new "my blog"
    ```
    生成新的博客，可用在source/_ posts/中打开，用本地编辑器编辑，编辑完成后，使用下面两条命令先清理，后编译
    ```
    hexo clean
    hexo generate
    ```

#### 连接到github
- 在blog文件夹安装deployer(每个新的hexo文件夹都需运行)
    ```
    cnpm install --save hexo-deployer-git
    ```
- 在github上生成一个主页page的repo,将repo命名为“用户名.github.io”（必须），如：he-zhenghua.github.io
- 修改_config.yml，对depoly部分做如下修改：
    ```
    deploy:
      type: git
      repo: https://github.com/he-zhenghua/he-zhenghua.github.io.git
      branch: master
    ```
- deploy
    ```
    hexo d
    ```
    若git没有设置默认用户名和用户邮箱，则会报错，需使用如下命令设置默认：
    ```
    git config --global user.name [username] #不用[]
    git config --global user.email [email]
    ```
#### 修改主题
- 将[主题](https://github.com/litten/hexo-theme-yilia)克隆到themes文件夹下
    ```
    https://github.com/litten/hexo-theme-yilia.git themes/yilia
    ```
- 修改_config.yml，将theme:修改为主题名
    ```
    theme: yilia
    ```

#### 内容截断和目录
- 截断
    在md文件需要截断处添加
- 目录
    使用[插件](https://github.com/bubkoo/hexo-toc),先安装插件
    ```
    cnpm install hexo-toc --save
    ```
    修改主目录下_config.yaml,添加以下内容：
    ```
    toc:
      maxdepth: 3
      class: toc
      slugify: transliteration
      decodeEntities: false
      anchor:
      position: after
      symbol: '#'
      style: header-anchor
    ```
    参数涵义可在[此](https://github.com/bubkoo/hexo-toc)查看
    在需要添加目录处添加



