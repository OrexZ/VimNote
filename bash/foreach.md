
## bash shell 中的遍历操作

### 使用内部字段分隔符分割输出流

有时候你想要使用shell的for遍历变量或者其他的输入流，
又苦于找不到好的方法来满足自己的分隔要求，那么请试试这个：

    cookie="hello,world,top,bottom,upper"

    OLD_IFS=$IFS
    IFS=','

    for x in $cookie
    do
        echo $x
    done
    IFS=$OLD_IFS


由于'IFS'是控制全局分隔符的全局环境变量，请慎重的处理它，并适当的根据需要还原它。


### 使用tr来控制分隔符的输出

当你不想使用全局'IFS'环境控制分隔符的时候，可以使用外部（其实也是linux发行版内置的）工具'tr'来完成分割。
试试这个方法：

    cookie="hello,world,top,bottom,upper"

    for i in `echo $cookie | tr "," " "`
    do
        echo $i;
    done

因为我们只是想要处理以特定分割符分割的项，所以根据你想要的操作，将','转化为系统默认的' '（空格）字符即可。


