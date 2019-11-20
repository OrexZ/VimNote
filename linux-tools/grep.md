
## 使用grep

本章内容主要讲述如何使用linux常用工具grep来搜索文本内容。


### 经常查看帮助文档是一个好习惯

    man grep #or info grep

### 写在前面的一些内容

grep的命令行语法格式：

    grep OPTIONS PATTERN INPUT_FILE_NAMES

grep命令的选项说明：

1. 一些短选项是符合POSIX.2协议标准的
2. 长选项都是GNU扩展选项

什么叫'单词'?
grep认定的单词是：下划线，数字，字母组成的内容。

grep输出的shell状态码：
通常情况下，如果能匹配到内容，则退出状态码为0，
否则为1。但是如果发生了错误，则退出状态码为2.(除非使用了"-s"或"-q"选项)



### 控制匹配模式的选项

> -e PATTERN # --regexp=PATTERN'

明确指定使用此处的PATTERN作为待匹配的pattern

> -f FILE # --file=FILE

从FILE文件中获取PATERN列表，每行一个。

> -i # --ignore-case

忽略大小写

> -v # --invert-match

反转匹配结果，优先级高于'-l'选项，后面看到

> -w # --word-regexp

全词匹配，如果你输入'abc'，而文本中是'abcd'无法匹配，必须全词匹配。

> -x # --line-regexp

全行匹配，什么是一行？就是到'\n'为止。
和'-w'选项一样，是全部内容匹配才可以。


### 控制输出内容

> -c # --count

输出匹配到行的数量，与'-v'选项可以表示未匹配到行的数量

> --color[=WHEN]

输出颜色，WHEN有never, always, auto,一般我们设置了auto选项


> -L # --files-without-match

输出未能匹配到的文件名列表

> -l # --files-with-matches

输出匹配到的文件名列表

> -m NUM # --max-count=NUM

这个一般配合脚本执行，使用这个选项可以在当前匹配到NUM个行时停止执行读取搜索文件。
不过这个停止时暂时的，并且grep会标记这次索引的最后一次匹配行的位置，使得调用两一个进程可以从此处恢复并继续向下搜索。

    while grep -m 1 PATTERN
    do
    echo xxxx
    done < FILE

> -o # --only-matching

输出匹配到的字符串，而不是输出整行内容。调式与练习时候还是不错的。

> -s # --no-messages

禁止输出因文件不存在或文件没有读权限而产生的错误信息.

注意在POSIX与GNU规范中，有所不同，可以在grep中添加重定向到/dev/null设备节点上。


### 控制输出行的前缀与上下文

> -H # --with-filename

输出匹配的文件名字，在搜索多个文件的时候，这个选项时默认的
一般我们也打开这个选项，毕竟在终端中执行命令是为了定位内容位置

> -h # --no-filename

禁止输出文件名称，这在搜索一个文件的时候时默认的

> -Z # --null

在输出文件名时，使用'\0'放在文件名字的后面，进而替代原本使用的字符，如'\n'或者':'

1. grep -lZ 输出每个文件都在同一行，而不是分行
2. grep -HZ 输出文件名字后没有冒号

> -A NUM /-B NUM / -C NUM #(--after-context=NUM) | (--before-context=NUM) | (--context=NUM)

附加输出在匹配到的行的后面，前面，或者前后的NUM行的内容

> z # --null-data

以'\0'最为输入行的分隔符，而不再以换行符分割，为grep提供跨行匹配的能力.


### 文件和目录的选择

> --exclude=GLOB

忽略文件名称（不加目录也叫basename）能被GLOB匹配到的文件。GLOB通配符包括："\*"、"?"和"[...]"。

> --exclude-from=FILE

从FILE中读取exclude的排除规则。

> --exclude-dir=DIR

筛选出不进行递归搜索的目录，使用DIR进行匹配。

> --include=GLOB

只搜索basename匹配GLOB的文件中的内容

> -r # -R --recursive

递归搜索给定的目录


### grep的后端引擎

有4种grep程序分别支持不同的搜索引擎，使用下面4个选项可以选择使用哪种grep程序。

> -G # --basic-regexp

使用基础正则表达式引擎解析PATTERN，因此只支持基础正则表达式(BRE)。
这是默认grep使用的后端。

> -E #--extended-regexp

使用扩展正则表达式引擎解析PATTERN，因此支持扩展正则表达式(ERE)。('-E'是POSIX指定的选项。)

> -F #--fixed-strings

不识别正则表达式，而是使用字符的字面意义解析PATTERN，因此只支持固定字符串的精确匹配。('-F'是POSIX指定的选项。)

> -P #--perl-regexp

使用perl正则表达式引擎解析PATTERN，因此支持Perl正则表达式。但该程序正处于研究测试阶段，因此会给出一个警告。


此外，"grep -E"和"grep -F"可分别简写为egrep和fgrep。但这两个简写程序是传统写法，已被废弃，虽仍支持，但只是为了兼容老版本程序。

还有zgrep和pgrep，但它们不是grep家族的程序，zgrep是gzip提供，pgrep用于查看进程名和pid的映射关系

### grep 支持的正则表达式


### grep
