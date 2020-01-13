---
title: xterm配置入门
toc: true
date: 2019-01-10 20:24:24
categories: xterm
---

大家好，我是`红桃C`。
这篇文章主要讲述的是xterm相关配置，希望能给你带来帮助。


## xterm配置文件的位置

一般配置xterm使用$HOME/.Xresources，当然$HOME/.Xdefaults也可以，
不过最好使用官方推荐的$HOME/.Xresources.

## 如何加载xterm配置文件

对于xterm的配置文件，你可以按照自己的要求来规划，
这得益于xinitrc和xrdb等这样的Xwindows相关的配置和工具

如何配置最简单？分三个步骤

1. 建立一个$HOME/.xinitrc文件，在Xwindows启动的时候加载你的配置脚本
2. 在配置文件中写入:
    [[ -f ~/.Xresources ]] && xrdb -merge -I$HOME ~/.Xresources
3. 最后定制你的.Xresources文件即可


PS: 有些同学喜欢这样来定义结构

    > ~/.Xresources

    #include ".Xresources.d/xterm"
    #include ".Xresources.d/rxvt-unicode"
    #include ".Xresources.d/fonts"
    #include ".Xresources.d/xscreensaver"

这完全看你个人的喜好，这里不再赘述

## xrdb 常用指令

加载资源文件

    xrdb -load $HOME/.Xresources

添加资源（只是添加设置，保留以往的部分没有覆盖的设置）

    xrdb -merge $HOME/.Xresources

查看当前生效的设置

    xrdb -query -all


## 如何配置中文输入法

在xterm中配置中文输入法并不难，不过你要知道自己使用的是什么输入法引擎。

比较流行的有：ibus，fictx等

搜狗输入狗需要fictx引擎的支持，在Centos7下面支持太麻烦，所以笔者直接使用
ibus引擎配合googlepingyin来完成中文的书写

配置方式就是一行，完美开启中文输入

    !input method
    xterm*inputMethod:ibus


## 如何安装字体

字体文件有很多种类，下文会根据需要更新不同字体的安装。

关于终端字体与显示器的匹配最佳方案，[这里][1]给出了比较好的解释。

### 安装ttf字体

由于不同的linux发行套件可能有不同的字体引擎，这里以Centos7为例

其实步骤很简单

1. 知道自己需要什么字体，然后下载它（这里是ttf格式）
2. 使用字体引擎的相关配置文件，一般配置文件的路径为
    - /usr/share/fonts
    - /usr/local/share/fonts
    - /home/$USER/.fonts
    - /home/$USER/.local/fonts
    - 随便找一个适合你的位置然后添加你的字体文件
3. sudo fc-cache -fv $where\_your\_fonts\_path，这里输出的log，你会看到new了几个字体
4. fc-list | grep <yourFonts> 查看字体列表，验证是否添加成功


## 如何配置字体

在xterm中，配置字体很简单，如下这样

    ! English font
    xterm*faceName: DejaVu Sans Mono:antialias=True:pixelsize=24
    ! Chinese font
    xterm*faceNameDoublesize:YouYuan:pixelsize=24

## 如何在VIM中调试颜色表？

有一款叫做[xterm-color-table.vim][2]的插件可以满足你的需求，自己尝试一样吧:)


## 简单的xterm配置方案

这里提供一个本人自己总结的小方案，获得步骤如下

1. git clone git@github.com:OrexZ/VimNote.git $HOME/.ovim
2. ln -s $HOME/.ovim $HOME/.vim
2. ln -s $HOME/.vim/xterm/Xresources $HOME/.Xresources
3. ln -s $HOME/.vim/xterm/xinitrc    $HOME/.xinitrc


## 不错的链接

- https://www.cnblogs.com/taosim/articles/3269541.html
- https://blog.csdn.net/u013181648/article/details/64501569
- http://www.kbase101.com/question/19486.html
- https://invisible-island.net/xterm/manpage/xterm-contents.html < xterm常用函数解释
- https://unix.stackexchange.com/questions/109509/how-to-search-xterm-console-history
- [how to enable xterm-256-color](https://blog.csdn.net/origin_lee/article/details/39141769)


[1]: https://blog.csdn.net/diy534/article/details/7327213
[2]: https://vimawesome.com/plugin/xterm-color-table-vim

