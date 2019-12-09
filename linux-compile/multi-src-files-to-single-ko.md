
## 将多个内核驱动源文件编译成为一个单独的ko文件

正如标题所说，本文讲述在linux内核中，如何将多个源文件编译成一个ko文件发布。


### 准备源文件

    $ tree
    .
    ├── foo_01.c
    ├── foo_02.c
    ├── foo_exit.c
    ├── foo.h
    ├── foo_init.c
    ├── Makefile
    └── sub
        ├── sub_foo.c
        └── sub_foo.h

    1 directory, 8 files


源文件的内容如下，仅仅作为例子

foo\_init.c

    #include <linux/init.h>
    #include <linux/module.h>
    #include "foo.h"
    #include "sub/sub_foo.h"

    int __init foo_init(void){
        printk("%s\n", __func__);
        m_show();
        sub_print();
        return 0;
    }

    module_init(foo_init);

    MODULE_LICENSE("GPL");

foo\_exit.c

    #include <linux/init.h>
    #include <linux/module.h>
    #include "foo.h"

    void __exit foo_exit(void){
        printk(KERN_ERR "sum: %d\n", m_sum(10, 1));
        printk(KERN_ERR "foo exit.\n");
    }

    module_exit(foo_exit);


foo\_01.c

    #include <linux/module.h>

    void m_show(void){
        printk(KERN_ERR "%s\n", __func__);
    }

foo\_02.c

    int m_sum(int a, int b){
        return (a + b);
    }

foo.h

    int m_sum(int, int);
    void m_show(void);

sub/sub\_foo.h

    void sub_print(void);

sub/sub\_foo.c

    #include <linux/module.h>

    void sub_print(void){
        printk(KERN_ERR "%s\n", __func__);
    }

Makefile

    # coding=UTF-8

    ifneq ($(KERNELRELEASE),)#{

    soga-objs += foo_01.o
    soga-objs += foo_02.o
    soga-objs += foo_init.o foo_exit.o
    soga-objs += sub/sub_foo.o

    obj-m := foo.o
    foo-objs := $(soga-objs)

    else

    PWD           := $(shell pwd)

    ARCH          ?= arm
    KDIR          ?= /labs/linux-lab/output/arm/linux-v2.6.36-versatilepb/
    CROSS_COMPILE ?= arm-linux-gnueabi-

    all:
            make -C $(KDIR) M=$(PWD) ARCH=$(ARCH) CROSS_COMPILE=$(CROSS_COMPILE) modules

    clean:
            $(H)rm -rf *.ko *.o *.symvers *.cmd *.cmd.o *.mod.c *.order .tmp_versions/ .*.o.cmd .*.mod.o.cmd .*.ko.cmd */*.o

    endif


### 简单的解释一下

首先，你已经看到，上述的代码实现了在不同的源文件中，实现linux驱动编写的例子。
这个例子是可行的，并且你可以在任何你想要的地方使用它，模仿它来编程。
例子中的代码并没有什么意思，而且也没有实际的意义，不过有几点是可以支持这样编程的基础：

1. 编译器的基础obj依旧是以文件为单位，所以foo.h是必要的，如果你不想这样，可以尝试'#include "foo\_01.c"'
2. 使用这个例子，你可以实现不同的目录分级，这个依赖于linux内核的编译机制，看看Makefile中的'soga-objs += sub/sub\_foo.o'
3. 需要注意基础的linux驱动编写方法，比如一个模块中需要有license，不论在哪里，你都要保证ko中包含。
4. 重点中的重点是Makefile中的下面的语法

    soga-objs += foo_01.o
    soga-objs += foo_02.o
    soga-objs += foo_init.o foo_exit.o
    soga-objs += sub/sub_foo.o

    obj-m := foo.o
    foo-objs := $(soga-objs) # >> foo-objs 中，foo是一个编译出来的名字，并且前面需要有obj-m支撑，soga-objs就是要编译成一个ko所需的所有source files.

