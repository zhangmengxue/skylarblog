title: 关于博客主题以及HEXO的使用
categories: 
- 文章
tags: 
- 博客
---

博客使用 [Hexo](http://hexo.io/) 搭建的，他相关的文档介绍在这里 [DOC](http://hexo.io/docs/)。

## 一、下载代码

现在自己的 github 下新建一个叫做 blog 的仓库。然后到本地找个目录：

### 新建文件夹

    mkdir skylarBlog
    cd skylarBlog

### fork 代码

    # 初始化
    git init
    # 添加源
    git remote add barret https://github.com/barretlee/skylarblog.git
    # 拉取代码，记得要加 barret
    git pull barret

### 切换分支

    # 切换主干代码
    git checkout master

## 二、安装 hexo 及相关插件

### 第一步先安装 hexo：

    # (mac/linux)下使用 sudo安装
    npm install -g hexo 
    # 初始化一个 hexo 项目
    hexo init  

给博客安装 5 个插件：

- hexo-generator-feed    
- hexo-renderer-ejs      
- hexo-renderer-stylus
- hexo-generator-sitemap 
- hexo-renderer-marked

写在 `package.json` 文件中，通过 `npm install` 即可安装。

### 安装组件

    # 运行插件安装命令
    npm install
    简写： npm i


## 三、文件目录及配置

    ├── _config.yml       # 配置文件
    ├── package.json      # npm 配置文件
    ├── public            # 别鸟他
    ├── scaffolds         # 别鸟他
    ├── source            # 文章全部在这里
    │   ├── CNAME         # 用于配置域名
    │   ├── _posts        # 文章内容（文件夹）
    │   └── about         # 可直接访问的页面
    └── themes            # 主题，可在 _config.yml 中配置主题类型
        ├── jacman
        └── landscape

配置 _config.yml，在最后找到：

    deploy:
      type: github
      repository: https://github.com/barretlee/skylarblog.git
      branch: gh-pages   

将 repository 改成你自己的地址，`https://github.com/zhangmengxue/blog.git`

目前在 master 分支下操作，执行下面的命令之后，会在 github 上自动生成一个 gh-pages 分支，这个分支不用理会，它是自动生成的。只需要在这个分支下开发。

## 四、快速指南

### 编译

    $ hexo generate
    简写：hexo g

编辑的文档使用的 md 语法，使用 `hexo g` 将 md 文件编译成 html 文件，然后执行 `hexo s`就可以在本地预览了。

更多信息: [Generating](http://hexo.io/docs/generating.html)

### 启动本地服务器

    $ hexo server
    简写：hexo s

会开启一个端口为 4000 的本地服务器，可以用于本地调试

更多信息: [Server](http://hexo.io/docs/server.html)

### 直接将代码部署到 Github Pages

    $ hexo deploy
    简写： hexo d

更多信息: [Deployment](http://hexo.io/docs/deployment.html)

### 组合命令

    # 编译之后，打开本地服务器
    hexo s -g
    # 编译之后，直接发布到 Github Pages
    hexo d -g


## 五、发表文章

### 创建一篇新的文章

    $ hexo new post-name
    简写：hexo n post-name

这句命令会在 source/_posts 下生成一个叫做 post-name.md 的文件，然后通过 `hexo d -g`，打开本地服务器查看网页内容。

### 创建一个页面

    hexo new page about
    简写：hexo n page about

这条命令就可以生成一个 about 页面。

更多信息: [Writing](http://hexo.io/docs/writing.html)

### 把修改保存到自己的 github 上

    # 添加自己的仓库源
    git remote add skylar https://github.com/zhangmengxue/blog.git
    # 推上去
    git push skylar master
    ## 这里需要注意的是，不需要管 gh-pages 分支，这个分支已经被 hexo 自动推送了


## 六、其他方面的介绍

在 themes/jacman 中可以配置页面信息，css 采用 stylus 写的，博客信息的配置也可在 `themes/jacman/_config.yml` 中修改。

如果需要修改文件内容，建议直接在全局搜索关键词。

