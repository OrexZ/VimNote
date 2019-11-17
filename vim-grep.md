
## vim内置搜索功能

很多插件都提供了这样的功能，不过我们看看vim内置的文件搜索功能：

帮助文档：

    :help vimgrep

这里面有很多可以定制的内容，下面仅仅讲述使用grep后端完成搜索。


常用命令及适用场景：

> 搜索本文件中的关键字

    :vim[grep] /keyword/ % | copen

> 搜索当前文件所在目录的所有文件中的关键字

    :cd %:h 或者已经使用了autochdir 选项
    :vim[grep] /keyword/ * | copen

> 递归地搜索指定目录下的所有文件

    :vim[grep] /keyword/ ../** | copen
    :vim[grep] /keyword/ ../ | copen

> 搜索多个指定目录下的所有文件

    :vim[grep] /keyword/ path_01/** | copen
    :vim[grep] /keyword/ ../path_02/** ../../path_03/* | copen


不过这个搜索并不是异步的，所以你应该知道你要搜索的内容大致应该在那里。

使用`<C-w> o`可以关闭多有除了当前窗口外的其他窗口，自己琢磨吧。

重新映射一个上下窗口跳转的快捷键可能会很不错：

    nmap <C-J> <C-W>j
    nmap <C-K> <C-W>k


## vim同时搜索多个关键字

以‘或’的形式，同时搜索多个关键字：

    /\vword01|word02
    /word01\|word02


以‘与’的形式，同时搜索多个关键字：(使用类似perl形式的正则表达式)

    /\vword01(some pattern)word02



