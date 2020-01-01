---
title: Hexo Q&A
toc: true
date: 2019-06-23 20:24:24
tags: hexo
categories:
---

本文记录关于hexo使用中一些常见的问题。


## 如何使用hexo配合github建立blog？


这个问题一般google或者baidu就可以简单的找到答案，这里推荐一片文章，[带你过去](https://zhuanlan.zhihu.com/p/60578464)。


## 如何显示本地链接图片与网络链接图片？


首先需要确认你所使用的hexo版本
 
    hexo -v

截止到本文，hexo应该有3.0+ 的版本，所以后续都以该版本说明问题。


### 网络图片链接

文章中引用网络链接图片很简单，而且一般也不会有太大的问题，格式为:

模板：

    ![picture text](URL "tag names")

例子：

    ![picture text](https://cn.bing.com/images/search?view=detailV2&ccid=2wrCKDWu&id=CC79FDA16B5CC6C52F5C1D864D0E69419F976EC8&thid=OIP.2wrCKDWuP8_1Xk5Ao_uQZQHaE7&mediaurl=https%3a%2f%2fwww.fjordtravel.no%2fwp-content%2fuploads%2f2013%2f10%2f002588_Roger-Johansen_www.nordnorge.com_Bodoe-1024x682.jpg&exph=682&expw=1024&q=picture&simid=608008046234307249&selectedIndex=1&qpvt=picture&ajaxhist=0 "picture tag")
    

### 本地图片链接

由于hexo不同版本加上不同版本的插件兼容性，可能导致本地图片链接有一些问题。

这里我们只介绍3.0 以后版本的使用，首先推荐官方使用的方法：

[hexo官方使用方法传送门](https://hexo.io/zh-cn/docs/asset-folders.html)


如果你还是像要按照markdown语法完成编写本地图片链接，那么请参考这片文章：

[比较清晰的讲述本地图片链接问题的blog](https://www.xilixili.net/2019/03/26/hexo-markdown-use-images/)

这里为了保证后续能够顺利使用，做一些简单的引用：


hexo-asset-image 这个插件是实现markdown语法显示本地图片链接，那么直接通过NPM安装这个插件，
总结一下，首先启用资源拷贝功能，在 _config.yml 中，修改

    post_asset_folder: true

然后在使用的时候，首先需要通过 *hexo n xxx* 创建新博客的时候，会创建同名 xxx 的目录，
相关的资源文件放里面即可，当然这个目录也可以手动创建，最终大概目录结构如下

    ./test.md
    ./test/
    ./test/bg.png


在博客中引用的时候，就按正常的引用方式引用即可，如

    ![](test/bg.png)

但是直接这么实现，我发现并没有成功，然后参考[这篇文章](https://www.jianshu.com/p/3db6a61d3782)

原来是兼容性问题，卸载刚安装的插件，安装参考中作者的修改版本

    npm uninstall hexo-asset-image
    npm install https://github.com/7ym0n/hexo-asset-image --save
    hexo clean && hexo g

到这里基本OK了，然后F12查看了地址，虽然可以访问了，但是地址却是带域名方式的，
通用性不好，查看了作者的实现逻辑，确认是字符串切割问题，然后在 _config.yml 中,
修改网站地址从 *https://xilixili.net* 修改为 *xilixili.net*， 不带 http(s) 头，重新生成就好了。
    
小结：
1. 可以严格按照官方的推荐方式书写本地图片链接
2. 可以借助插件实现markdown相似图片链接的写法，不过需要外置插件安装
3. 填加插件后，为了保持格式同步，请修改 *_config.yml* 文件中的url选项

