---
title: Shell
toc: true
date: 2019-06-25 20:40:55
tags:
categories:
---

## 哪里有在线的免费shell书籍可以查看？

开源shell书籍，你值得拥有！

[官网地址](http://tinylab.org/open-shell-book/)

[在线阅读](https://tinylab.gitbooks.io/shellbook/)


## 如何查看shell的命令类型？

可以使用shell内置的命令type完成类型的打印反馈。
    
    
## 查看shell中某个命令类型的时候，返回 xx is hashed是什么意思？

一般这类命令肯定不是内置命令，并且可以在PATH路径中索引到，之所以显示hashed是因为当你运行一遍该命令后，它会被缓存在内存的hash表中。


## shell中的help命令用来干什么？

help命令用来显示shell内置命令的帮助文档，如果是外置命令一般会有它自己的帮助文档，不过打开方式是man命令。


## shell中如何进行数字的比较？
shell中这点比较反人类，比较数字的时候并不使用大于和小于号，"<" ">" "==" "!="，而是使用下面的特殊组合表示：

    A -lt B # A < B
    A -gt B # A > B
    A -eq B # A == B
    A -nq B # A != B


## shell中 [ ] 和test命令的区别？

其实没有区别，可以理解为[ ]是test的简写，不过要注意[ ]前后的空格，如果你理解它是一个命令，那就好解释了，毕竟命令和参数之间需要空格隔开的。


## shell中是否可以使用分号？

当然可以，而且可以使用分号区分指令，如：

    cd $HOME; ls -al

## 执行shell命令时使用bash，source，. 都可以，那么它们的区别是什么？


## shell中的time命令是用来干什么的？

它并不是用来显示当前时间的，显示当前时间是date，而显示某条指令运行的时间统计是time。


## 如何使用shell完成数字的进制转换？

shell内置方法就不推荐了，可以使用套件中的默认安装工具bc完成该操作，并且很灵活，如：

    echo "obase=10;ibase=8;11" | bc -l
    
进制的替换可以按照你的个人需求来。


## 如何在shell中显示字符的acsii码？

通常情况下，使用man acsii命令可以查看acsii编码的规则，不过如果你不想计算，那可以使用下面的技巧，完成字符的转换显示：
例如，默认的分隔符 IFS 包括空格、 TAB 以及换行，可以用 man ascii 佐证

    $ echo -n "$IFS" | od -c
    0000000      t  n
    0000003

    $ echo -n "$IFS" | od -b
    0000000 040 011 012
    0000003
    
