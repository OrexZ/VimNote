
## VIM Help 使用方法


本文主要介绍VIM帮助文档的使用方法，以便开发者或者使用者可以通过本文提供的方法，
快速定位或者找到你想要的且VIM已经提供的帮助。

### 常用的内置方法

按照类别列出VIM内置函数：

    :help function-list

按照字母顺序列出内置函数：

    :help functions


查看内建函数的解释：

    :help built_in_func_name()

查看所有VIM支持的类型：

    :help type()

查看所有内置自动命令事件类型：

    :help autocmd-events

列出所有脚本与编号

    :scriptnames

查看按键映射的时候，这两个函数比较有用：

    mapcheck({lhs})
    hasmapto({rhs})


当 :source 命令之后加个 ! 符号，就是表示所读的 文件不是当作 ex 命令的脚本了，而是当作普通命令的“宏”了


查看命令映射参数<expr>的内置说明，多了解一些映射函数的限制

    :help map-<expr>





