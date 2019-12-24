
## Device Tree相关知识点汇总
-----

### 如何编译linux的设备树？

开启内核支持设备树选项

    make menuconfig ---> Boot options ---> Flattened Device Tree support

正常编译kernel的时候会附带编译设备树

    make ARCH=arm LOADADDR=0x007fc0 CROSS_COMPILE=arm-linux-gnueabi- uImage

不过，如果你想要单独编译所有的devicetree文件，如(dts,dtsi,\*.h)这样的集合，可以使用编译系统提供的如下命令

    make ARCH=arm LOADADDR=0x007fc0 CROSS_COMPILE=arm-linux-gnueabi- dtbs

如果你想要单独编译某一个dts文件，也有办法，可以使用编译内核时，生成的dtc工具来完成单编某一个dts文件

    #生成dtc工具
    make scripts

    $ROOT/scripts/dtc/dtc -I dts input.dts -O dtb -o output.dtb
    $ROOT/scripts/dtc/dtc -O dtb -o output.dtb input.dts #或者

反编译也可以

    $ROOT/scripts/dtc/dtc -I dtb input.dtb -O dts -o output.dts
    $ROOT/scripts/dtc/dtc -O dts -o output.dts input.dtb #或者

基本上，内核的设备树相关编译内容就这么多。


### fdtdump工具

这个工具可以根据发行版的包管理器进行安装,使用起来也非常简单

    fdtdump -h
    fdtdump -sd xx.dtb | tree dt.log | less #需要知道的是，//后面是分析过程信息


### 删除设备属性

可以利用设备树架构提供的源语`delete-property`删除那些你不想修改原始设备文件，不过又需要定制的时候

    / {
        HOME: home {
            property,01 = <0x01>;
            property,02 = [00];
            str,show = "new";
        }
    }

    // 删除时，在新的dts文件中进行如下操作

    / {
        &HOME {
            /delete-property/str,show;
            /delete-property/property,02;
        }
    }

这样就可以删除不想要的属性了哦。

###

[1]: https://elinux.org/Device_Tree_Mysteries#error_messages
[2]: https://www.cnblogs.com/pengdonglin137/p/4495056.html
