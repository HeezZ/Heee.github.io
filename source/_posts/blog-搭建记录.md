---
title: blog 搭建记录
date: 2020-04-09 15:26:29
tags: 环境搭建
---
<!-- toc -->
<!--more-->

- 环境: Windows，hexo + github

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
    - 在md文件需要截断处添加
    ```
    <!--more-->
    ```
    - 修改yilia主题，使其支持description摘要，[参考](https://github.com/litten/hexo-theme-yilia/issues/166#issuecomment-235274516)
    删除`yilia/layout/_partial/article.ejs`文件中12到19行（必须删除，不可注释），替换为如下
    ```
          <% if (index && (post.description || post.excerpt)){ %>
              <% if (post.description){ %>
                  <%- post.description %>
              <% } else { %>
                  <%- post.excerpt %>
              <% } %>
              <% if (theme.excerpt_link) { %>
                  <a class="article-more-a" href="<%- url_for(post.path) %>#more"><%= theme.excerpt_link %> >></a>
              <% } %>
          <% } else { %>
              <%- post.content %>
          <% } %>
    ```

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
    ```
    <!-- toc -->
    ```

#### 利用github分支同步源文件
[参考文章](https://lulua87.github.io/2017/03/06/hexo_blog_for_github_branch_auto_update/)
- 思路：在我们博客的远程仓库中新建一个分支，用这个分支来存储博客的源文件，这样我们每次在更新博客并部署之后可以顺手多执行两条命令将源文件同步到远程分支中去
- 初始化版本库&建立仓库关联
    通过以下命令查看是否有关联的仓库
    ```
    cd blog
    git remote
    // origin 若无输出则无关联
    ```
    初始化版本库，并建立与远程仓库的关联,这里的 origin 是定义的远程仓库在本地的名字，也可以叫别的，一般命名成 origin
    ```
    git init
    git remote add origin git@github.com:he-zhenghua/he-zhenghua.github.io.git

    git remote
    // orgin 可看到origin
    ```
- 提交文件
    像正常提交文件那样使用 git add 、 git commit 和 git push 命令提交文件，但这里在 push 的时候要注意新建一个分支去存你要提交的源文件，具体命令是 git push -u origin HEAD:分支名，这里的分支名自己取，HEAD 是版本库的头指针的意思，代表本地版本库里面的最新版本，origin 是刚刚你自己添加远程关联时候的名字，如果你的不是叫 origin 就写成自己定义的名字， -u 参数是为了建立本地分支与远程分支的关联，以后 push 的时候直接输入 git push 就可以了，所以这整个命令的意思就是：把本地最新版本的代码提交到远程仓库的某个分支上去，如果远程仓库还没有这个分支，就在远程仓库里新建一个分支，然后将它跟本地当前分支关联起来。提交之后你就会发现自己的 github 仓库多了一条分支，就是你刚刚提交的那个分支。   至于这里为什么不先在 github 上面手动建立分支，然后再在本地建立关联，是因为如果是远程手动建立分支会自动以 master 分支为模板建立一份一模一样的文件，而我们仓库里面 master 分支存的都是经过 hexo 编译的文件，跟源文件完全不一样，新建这样一个分支之后还要手动把里面的文件删掉，另一个原因是如果在远程手动建分支，你在本地还得手动用 git fetch origin 拉取远程分支的更新，然后再手动建立与分支的关联，比较麻烦，当然如果你是刚开始部署 hexo，github 仓库里面还一点东西都没有的话这些问题都不存在，那就随意。
    ```
    git add .
    git commit -m "add source" //windows下使用双引号
    git push -u origin HEAD:source
    ```
    以后修改之后执行
    ```
    git add .
    git commit -m "message"
    git push origin HEAD:source
    ```
    来同步
- 设置默认分支
    去github账号将source设置为默认分支，以后在别的机器上拉取代码的时候能够直接拉取源文件，不用再指定分支
- **一个坑**
    主题文件夹下有一个.git文件夹，若直接上传则上传空文件夹，因为一个git仓库中不能包含另一个git仓库,  
    需要一开始把.git删除，然后再add,commit，若先不小心add、commit了，则需先把主题文件夹中.git删除，然后移动到其他地方，add、commit一次，再把主题文件夹拷回来，重新
    ```
    git add ./themes/yilia //必须精确到文件夹
    git commit -m "message"
    ```
    [参考](http://jartto.wang/2017/12/28/cannot-nest-git-repository/)
- 同步到新设备
    - 克隆仓库的sorce分支（已经是默认分支，直接git clone就行）
    - 在克隆得到的文件夹执行
        ```
        npm install
        ```
        由于仓库有一个.gitignore文件，里面默认是忽略掉 node_modules文件夹的，也就是说仓库的hexo分支并没有存储该目录，所以需要install下
    - 需要注意的是每次更新博客之后, 都要把相关修改上传到source分支
- 总结一下修改一次blog需要执行的命令
    ```
    hexo clean
    hexo g
    hexo d

    git add .
    git commit -m "message"
    git push origin HEAD:source
    ```
#### 数学公式显示错误问题

[参考](https://www.jianshu.com/p/7ab21c7f0674)
1. 更换Hexo的markdown渲染引擎，hexo-renderer-kramed引擎是在默认的渲染引擎hexo-renderer-marked的基础上修改了一些bug，两者比较接近，先卸载原来的渲染引擎，再安装新的
    ```
    npm uninstall hexo-renderer-marked --save
    npm install hexo-renderer-kramed --save
    ```
2. 更换引擎后行间公式可以正确渲染了，但是这样还没有完全解决问题，行内公式的渲染还是有问题，因为hexo-renderer-kramed引擎也有语义冲突的问题。接下来到博客根目录下，找到node_modules\kramed\lib\rules\inline.js，把第11行的escape变量的值做相应的修改：
    ```
    //  escape: /^\\([\\`*{}\[\]()#$+\-.!_>])/,
    escape: /^\\([`*\[\]()#$+\-.!_>])/
    ```
    这一步是在原基础上取消了对\,{,}的转义(escape)。
    同时把第20行的em变量也要做相应的修改
    ```
    //  em: /^\b_((?:__|[\s\S])+?)_\b|^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
    em: /^\*((?:\*\*|[\s\S])+?)\*(?!\*)/
    ```
3. 在主题中打开mathjax，进入到主题目录，找到_config.yml配置问题，把mathjax默认的false修改为true，具体如下：
    ```
    mathjax:
      enable: true
      per_page: true
    ```
4. 在文章的Front-matter里打开mathjax开关
    ```
    ---
    title: 背包问题系列
    date: 2020-04-27 08:38:43
    mathjax: true
    tags: 
        - 动态规划
        - 算法
        - leetcode
    --
    ```
    之所以要在文章头里设置开关，是因为考虑只有在用到公式的页面才加载 Mathjax，这样不需要渲染数学公式的页面的访问速度就不会受到影响了。

#### 问题记录
- 报错FATAL Port 4000 has been used. Try other port instead
原因：hexo s命令预览博客效果后使用Control+C关闭  
解决：
```shell  
hexo s -p 5000
```