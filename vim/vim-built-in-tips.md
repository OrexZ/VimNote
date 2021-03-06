
# VIM内置功能的使用技巧

很多开发者都狂热于VIM的可定制话，并迷失在VIM众多插件的海洋中，无法自拔。

其实VIM作为一个好用的文本编辑器，已经内置了很多好用的功能，一起看看吧。

使用内置技巧有一个好处，那就是你可以在不同的机器上有相同的动作，
别人看的眼花缭乱的时候，你可以牛逼轰轰的拂袖而去。


## 如何手动reload你的文件到内存？

有时候我们对一个文件可能有多个buffer与之对应，并且可能在不同的时候进行修改，
这很可能引起同步问题，那使用VIm的时候如何使用手动更新当前文件的内存映射呢？

    :e

就这么简单的一句话。

如果你不想要存储你当时的所有修改，请使用下面的指令将文件的内存映射恢复到原来(或者其他地方已经修改后的版本)

    :e!


## 如何删除文件中的'^M'符号？

其实这个是一个dos版本的换行符，一般是windows相关编辑器引入的。

不过在VIM中可以手动删除掉。

1. 删除掉指定位置

    :%s/^M//g

'^M'是在类插入模式下，使用<C-v>+<CR>打印出来的。

NOTE: <C-V>可以打印不可见字符。


2. 格式化整个文件，并保存

    :set ff=unix
    :w
    :set ff=dos
    :w


## 如何利用vim保存文件为不同格式？

使用vim编辑器，需要知道的一点是，如何格式化自己的文件

其实很简单，这里介绍不改变vim配置的情况下，修改当前文件的编码格式并保存

    1. > set fileencoding=utf-8 #修改当前文件的编码格式
    2. > wq

如何要配置全局生效的文件，并打算去除编码格式带来的乱码，可以这样

    # in vimrc
    set fileencoding=utf-8,gbk,...


还有一些小技巧，可以参考[这里][1]


[1]: https://blog.csdn.net/mergerly/article/details/50899575
