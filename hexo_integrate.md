---
title: 如何将本仓库集成到hexo
date: 2020-01-01 22:00:00
categories: howto
---

## 概述仓库的准备思考

首先，需要三个仓库

1. 第一个是github.io，用来存放和发布hexo的public内容
2. 第二个是hexo的配置仓库，这个仓库一旦经过配置，不会随意更改，并且需要用它来发布hexo的内容到github.io
3. 第三个是VimNote仓库，这个仓库很灵活，可以随意的使用VIM书写markdown文档，不用关心任何网站的细节，什么时候需要更新网站时，用hexo推一下

## 快速集成和安装

仅仅需要两个步骤

1. 链接source文件到\_posts/下
    ln -s $path_for_note/VimNote $hexo_posts/VimNote

2. 链接images目录到source/下
    ln -s $path_for_note/VimNote/images $hexo_source/images


## 需要注意的点

由于hexo需要使用yaml的类型，所以不要用类似`---`这样的分割线


