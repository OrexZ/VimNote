
## python运行外部操作系统指令的方法

python有很多方法让你可以运行操作系统提供的外部指令，如linux和windows的一些你看到的所谓的命令。

下面列出一些可用的库

1. os.system
2. os.popen
3. command
4. subprocess

这些都是可以使用的，根据不同的python版本可能会有一些不同。

不过我不建议你都了解他们，如果你感兴趣的话当然可以，不过最好不要浪费时间。

本文需要强调的是，使用`subprocess`标准库吧，并且一直使用它吧，他是很高级的。

不过需要注意的是，如果你是用的python版本不是很高，那么可能其他的库是你需要了解的。

参见 [PEP 324][1] -- 提出 subprocess 模块的 PEP

官方3.8版本的说明文档在[这里][2]


[1]: https://www.python.org/dev/peps/pep-0324/
[2]: https://docs.python.org/zh-cn/3/library/subprocess.html#subprocess.TimeoutExpired


