
# 使用find命令

find命令是查找文件的好帮手，请不要光依靠grep来搜索大量文件，
当文件不多时，grep可以快速的满足要求，这个快速是相对的，也就是人可以接受的范围
而当文件很多的时候，请使用find,xargs,grep三兄弟完成高效的文件查找与字符过滤。

## 帮助文档

    man find # info find




## 使用案例

`find`指令默认会递归查找指定目录及其子目录

### 列出当前目录及子目录下的所有空文件或者空目录

    find . -empty

### 搜索并排除指定的子目录

    find . -path './sk' -prune -o name '*.txt' -print


查找除了子目录'sk'目录下的所有txt文件

    find . -path './.git' -prune -o -path '.vim' -prune -o -name '*.md' -print

排除选项-prune控制前面的-path指定的目录，排除多个目录使用-o选项隔开


### 使用find指令执行外部脚本

    find . -exec ./outside.sh {} \;

outside.sh可以自己定制，传入的参数{}是find找到的每个输出结果。

NOTE: -exec 只能接受一条指令


### 动作之前加确认

    find $HOME/. -name "*.txt" -ok rm {} \;

交互选项-ok就是一个不错的选择


### 找到指定用户或者指定群组的所属文件

    find . -type f -user rex
    find . -type f -group rex-group


### 查找文件大小超过500M的文件

    find . -size +500M -type f


### 查找中的排除操作

    find . -type f ! -name '*.txt'

注意叹号'!'，这之后的参数是排除的选项
这个选项也可以配合-o选项进行连续排除

    find . ! -name "*.md" -o ! -name "*.png"


### 为什么\*.c报错

其实，这不是一个bug，查看手册可知，\*.c被shell解释为连续的文件名，然后传递给find指令，这不是它需要的，如

    find . -name *.c # --> find . -name a.c b.c c.c d.c --> error

因此，正确的做法是，不用shell来扩展，而让find命令自行完成\*字符解析。

    find . -name \*.c
    find . -name "*.c"


