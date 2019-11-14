
# Python 常见的几种处理路径问题

Python 虽然很方便，文档资料也是随处可见，本文内容也不是创新的内容，只是希望简单的总结一下编程中常见的处理路径的问题，希望对大家有所帮助。


## 如何获得当前文件的路径

一般这里指的路径分两种，一个是绝对路径，另一个是相对路径。

那么我们简单先介绍下几种路径的获得方法，最后看看例子。


> *\_\_file\_\_*

这是python内置的变量，表示包含文件名称的路径，可以肯定的是，
这个变量中一定包含文件名称，至于路径因执行 python 的方法不同而不同。


> *os.getcwd()*

这是os模块的函数，不是os.path。

它返回的是平台支持的文件路径，并不包含文件名称，而是路径的绝对值。

> *os.path.realpath(\_\_file\_\_)*

返回指定文件名的规范路径，消除路径中遇到的任何符号链接,
一般大部分系统都支持它。

> *os.path.abspath(\_\_file\_\_)*

返回路径名路径的标准化绝对化版本。在大多数平台上，这等效于按如下方式：
os.path.normpath(os.path.join(os.getcwd(), \_\_file\_\_)) 等价于 os.path.abspath(\_\_file\_\_)。

> *sys.path[0]*

sys.argv[0]|获得模块所在的路径，一般由系统决定是否是全名。


前面介绍了5种方法，我们一起看看在不同条件下的输出情况：

文件内容：
```python
#! /usr/bin/env python3
#filename: filename_test.py

import os,sys

print("filename:[__file__] >", __file__)
print("filename:[os.getcwd()] >", os.getcwd())
print("filename:[os.path.realpath(__file__)] >",os.path.realpath(__file__))
print("filename:[os.path.abspath(__file__)] >", os.path.abspath(__file__))
print("filename:[sys.path[0]] >", sys.path[0])
```

输出内容：
```bash
[cmd] > python filename_test.py
filename:[__file__] > filename_test.py
filename:[os.getcwd()] > /home/rex/C_LD/common_Makefile/scirpts
filename:[os.path.realpath(__file__)] > /home/rex/C_LD/common_Makefile/scirpts/filename_test.py
filename:[os.path.abspath(__file__)] > /home/rex/C_LD/common_Makefile/scirpts/filename_test.py
filename:[sys.path[0]] > /home/rex/C_LD/common_Makefile/scirpts

[cmd] > python ../scirpts/filename_test.py
filename:[__file__] > ../scirpts/filename_test.py
filename:[os.getcwd()] > /home/rex/C_LD/common_Makefile/scirpts
filename:[os.path.realpath(__file__)] > /home/rex/C_LD/common_Makefile/scirpts/filename_test.py
filename:[os.path.abspath(__file__)] > /home/rex/C_LD/common_Makefile/scirpts/filename_test.py
filename:[sys.path[0]] > /home/rex/C_LD/common_Makefile/scirpts

[cmd] > ./filename_test.py
filename:[__file__] > ./filename_test.py
filename:[os.getcwd()] > /home/rex/C_LD/common_Makefile/scirpts
filename:[os.path.realpath(__file__)] > /home/rex/C_LD/common_Makefile/scirpts/filename_test.py
filename:[os.path.abspath(__file__)] > /home/rex/C_LD/common_Makefile/scirpts/filename_test.py
filename:[sys.path[0]] > /home/rex/C_LD/common_Makefile/scirpts
```

一般获得当前模块或者你称之为脚本文件的绝对路径，使用:

    os.path.dirname(os.path.realpath(\_\_file\_\_))

看到了说出应该差不多就明白了，不过具体的细节请查看[官方文档][1]。
当然看看源码也是有好处的，如果你有时间的话。

## 其他常用的路径操作手法

> 获得文件名称

    os.path.basename(__file__)


> 获得文件名称和绝对路径

    os.path.split(os.path.realpath(__file__))


其他的路径裁剪，都可以使用本文提供的方法，
加上适当的`os.path`模块中的处理函数，来满足你的要求。


[1]: https://docs.python.org/3.8/library/os.path.htm

