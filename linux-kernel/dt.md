
## Device Tree相关知识点汇总
-----

### 如何编译linux的设备树？

正常编译kernel的时候会附带编译设备树

    make ARCH=arm LOADADDR=0x007fc0 CROSS_COMPILE=arm-linux-gnueabi- uImage

不过，如果你想要单独编译所有的devicetree文件，如(dts,dtsi,\*.h)这样的集合，可以使用编译系统提供的如下命令

    make ARCH=arm LOADADDR=0x007fc0 CROSS_COMPILE=arm-linux-gnueabi- dtbs

如果你想要单独编译某一个dts文件，也有办法，可以使用编译内核时，生成的dtc工具来完成单编某一个dts文件

    $ROOT/scripts/dtc/dtc -I dts input.dts -O dtb -o output.dtb
    $ROOT/scripts/dtc/dtc -O dtb -o output.dtb input.dts #或者

反编译也可以

    $ROOT/scripts/dtc/dtc -I dtb input.dtb -O dts -o output.dts
    $ROOT/scripts/dtc/dtc -O dts -o output.dts input.dtb #或者

基本上，内核的设备树相关编译内容就这么多。


