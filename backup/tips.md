---
title: CentOS Chinese Inputmethod
toc: true
date: 2019-02-18 12:39:11
tags:
categories:
---

# Tips

日常技巧速记，以Q&A形式发布。

## python 如何只检查语法而不执行具体的代码逻辑？
可以使用python的编译模块来达到检查语法的目的，使用如下：
```bash
$ python -m py_compile target.py
```

## Linux如何查看磁盘空间？如何查看目录文件的总体占用大小？

在Linux操作系统下有很多默认集成到套件中的命令可以帮助你完成这些事情，如：

查看整体磁盘的占用比例和使用情况可以使用df命令

    df -h
    
查看个别目录的文件磁盘占用可以使用du命令来完成

    du -sh *
    
具体使用细节请查看相应的帮助手册：man du/df


## Awesome锁屏配置

**锁屏**功能在工作环境下比较常用。

推荐一个比较[不错的配置教程](http://www.chengweiyang.cn/2016/02/16/how-to-lock-screen-in-awesome-wm/)

## FireFox Flash Plugin on [Ubuntu]

On ubuntu (version == 18.04), we had test and follow us:
```bash
sudo apt install flashplugin-installer
```
> - [How to install firefox flash plugin](https://linuxconfig.org/how-to-install-adobe-flash-player-plugin-for-firefox-on-centos-7-linux)

## Configuration Of Bash

https://github.com/junegunn/fzf
https://www.peterdavehello.org/2013/12/bash-completion/
https://github.com/donnemartin/gitsome


## Configuration Of Zsh

https://github.com/zsh-users/zsh
https://github.com/robbyrussell/oh-my-zsh
https://github.com/zsh-users/zsh-completions
https://github.com/junegunn/fzf
https://github.com/makeitjoe/incr.zsh
http://yijiebuyi.com/blog/36955b84c57e338dd8255070b80829bf.html
http://mimosa-pudica.net/zsh-incremental.html
https://github.com/changyuheng/fz
https://github.com/donnemartin/gitsome


## minicom usage

```bash
# example
sudo minicom -m -c on -C ./minicom_scp.txt -s
```

## how to install samba in CentOS

> - [Install Samba](https://www.cnblogs.com/cq146637/p/7806550.html)

