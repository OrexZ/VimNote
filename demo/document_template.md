---
title: 文章样例模板
---

本模板参考**泰晓科技**文档模板与约定，使用**Hexo**渲染引擎生成规范式文档。

具体格式内容，需要查看**本文源码**，由于该系列属于个人笔记，所以并没有安排自动生成模板格式草稿。

不过各文章的编辑基本参照该模板格式书写。


这里分别就几个方面展开介绍：

## 文章分类

文章目前记录随笔和系列撰稿，当前分类按照作者自己本人的意愿划分。

## 内容列表

1. 数字列表，条目 1
  * 普通列表，条目 1
  * 普通列表，条目 2

2. 数字列表，条目 2

## 代码缩进

代码在正文下，用 4 个空格缩进：

    #include <stdio.h>

    int main(void)
    {
       printf('Hello, World!');
    }

*注*: 如果要跟列表一起缩进显示，得添加相应空格。

当然，如果你使用类似**三个反引号**的形式标注代码也没有限制，一般在**emacs**中可以得到很好的支持,类似这样:

```lua
function demo()
    io.write("so...", "\n")
end
```

在 hexo 中建议使用**三个反引号**的形式，可以使用语法高亮，这是Hexo引擎使然。

## 正文内内嵌代码

如果正文中包含了命令、接口、代码片段、变量等属于代码的部分时，请用 ` 括起来，例如：`grep Free /proc/meminfo`，用法为：

    `grep Free /proc/meminfo`

## 中英文以及数字混排

当中 English 文以及中文、数字混排时，记得在 English 和数字，例如 1 2 3 4 周边添加空格，进而确保可阅读性。

## 表格用法

| 标题 1      | 标题 2     | 标题 3          |
|-------------|-----------:|:---------------:|
| 左对齐      |右对齐      | 居中对齐        |

## 在正文中嵌入图片

Markdown 基本语法如下：

![图片名](https://getspet.com/wp-content/uploads/2018/04/Scottish-Fold-beautiful-cats.jpg '图片内容提示，可选')

*注*：如果想规范图片大小，想增加额外的特性，可以用 html 的 `<img>` 标记。

### Hexo 本地图片显示

在Hexo的Markdown使用的时候需要注意一些细节，这可能和标准的Markdown有所不同。

[Hexo本地图片显示问题解决方案](https://www.xilixili.net/2019/03/26/hexo-markdown-use-images/)

所以，一般用法是：

![我们家的偷情的俩货...](/VimNote/images/cats.jpg)

### 网络图片显示

这没有什么特别的，按照Markdown的用法来:

![别人家的猫叔](https://cn.bing.com/th?id=OIP.GTbdLeO3kutK1Uaw2lOiKgHaFu&pid=Api&rs=1)


## 链接以及各类内容混排

### 链接

* [链接用法一][1]

  在列表后面放入脚本，为了确保跟列表一起缩进，需要额外增加两个空格：

      #!/bin/bash
      echo 'Hello, World.'


* [另外一种链接用法](http://tinylab.org)

  在列表后面再嵌入子列表，包括数字列表和非数字列表：

  * Another list
    * Another list
      1. Third list
      2. Third list


* 第三种链接用法：<http://tinylab.org>

### 更复杂的列表用法

1. 表项 1

    这里再嵌入代码：

        #include <stdio.h>

        int main() { return 0; }

2. 表项 2

    这里嵌入图片：

    ![图片名](https://halopets.com/sites/default/files/2017-07/hp_caticon_kitten_adobe_84509847_crop.jpg '图片内容描述信息')

3. 表项 3

    普通正文


*注*：数字列表跟普通列表有一个差别是，数字列表后面如果要加正文自动缩进，得增加 4 个空格，而普通列表只需要两个。估计是 Markdown 解释器的问题，请尽量遵循这个约定吧。

## 正文引用

如果要引用第三方的信息，可以这么做：

> 这里是来自第三方的信息，信息内容可以用普通的 Markdown 语法来标记[链接][1]、**加粗**、*斜体*、`命令`等等。

## Markdown 入门资料

[简单如何，动手实践下](https://blog.csdn.net/zhuzhuyule/article/details/58347687)

[Markdown 的一些罕见用法](https://sspai.com/post/37273)

[1]: http://tinylab.org

