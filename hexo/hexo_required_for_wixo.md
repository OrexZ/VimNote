---
title: 搭建hexo并使用wixo主题
date: 2020-01-01 22:00:00
categories: howto
---


## 最简单最好用的Blog主题WIXO

这里记录搭建hexo wixo主题的过程


### 首先安装hexo并初始化

安装hexo很容易，在[官网][1]可以很容易的找到答案


当前版本


    [rex@localhost hexo]$ hexo -V
    hexo-cli: 2.0.0
    os: Linux 3.10.0-957.el7.x86_64 linux x64
    node: 11.10.0
    v8: 7.0.276.38-node.17
    uv: 1.26.0
    zlib: 1.2.11
    brotli: 1.0.7
    ares: 1.15.0
    modules: 67
    nghttp2: 1.34.0
    napi: 4
    llhttp: 1.1.1
    http_parser: 2.8.0
    openssl: 1.1.1a
    cldr: 34.0
    icu: 63.1
    tz: 2018e
    unicode: 11.0


初始化hexo项目步骤

    hexo init blog
    cd blog
    npm install



### 下载wixo主题并使能

使用git，clone主题仓库

    git clone https://github.com/wzpan/hexo-theme-wixo.git themes/wixo


安装依赖的库（对于本文所指当前版本来说）

    npm install hexo-generator-search --save # required
    npm install hexo-tag-bootstrap --save # optional 当前版本使用0.2.0不行，需要使用0.0.8版本才会正常
    npm install hexo-deployer-git --save # for deploy.. required


部署时候的例子，当然需要修改配置文件\_config.yml

    deploy:
        type: git
        repo: <repository url> # https://bitbucket.org/JohnSmith/johnsmith.bitbucket.io
        branch: [branch]
        message: [message]


最后，修改\_config.yml,使能wixo主题



### 快速尝鲜

仅仅堆叠指令，common on

    hexo clean
    hexo g && hexo s


[1]: https://hexo.io/zh-cn/
