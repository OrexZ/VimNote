
## 如何修改Linux的hostname

本文只简单介绍一些使用方法，并未涉及深入理解的部分。

首先以ubuntu为例，如果要临时修改hostname，可以这样：

    hostname NEW_HOSTNAME

重新启动后hostname恢复原来的样子。

如果需要永久修改本机的hostname，那么请依据套件的不同，采用适合自己的方法来修改：

如果套件支持systemd，那么使用下面的简单指令可以永久修改hostname

    sudo hostnamectl set-hostname NEW_HOSTNAME

如果是类似sysVinit这样的管理架构，那么使用传统的方法，永久修改hostnmae

    #step 1
    $ vi /etc/hostname
    > NEW_HOSTNAME

    #step 2
    $ vi /etc/hosts
    > 127.0.0.1 NEW_HOSTNAME

    #step 3
    $ /etc/init.d/hostname restart

    #On RHEL/CentOS based systems that use init, the hostname is changed by modifying:
    > vi /etc/sysconfig/network
    >>
        #/etc/sysconfig/network
        NETWORKING=yes
        HOSTNAME="tecmint.com"
        GATEWAY="192.168.0.1"
        GATEWAYDEV="eth0"
        FORWARD_IPV4="yes"


如果有什么疑惑可以参考[这里][1]

[1]: https://www.tecmint.com/set-hostname-permanently-in-linux/


## 如何为单台主机添加多个hostname

这个需求一般不会存在，不过有时候公司的服务器默认是有命名规则的，所以如果你想要按照自己的想法来设计服务器的hostname，
这个时候就需要按照下面这样操作

其实也比较简单，需要注意的是/etc/hostname只能有一个名字生效，这里面不需要你进行更改，这是'主名字',
那么我们修改的是 /etc/hosts，本质上是添加该服务器的alias名字，就是别名。

    # step 1 : get your ip address
    $ ifconfig

    # step 2 : modify host name alias
    #/etc/hosts
    >
    HOST_IP NAME_1 NAME_2


具体内容请参考[这里][2]的解释与[这里][3]的回答

[2]: //www.unix.com/solaris/93371-assigning-two-hostname-single-server.html
[3]: https://serverfault.com/questions/155793/may-i-set-multiple-names-in-etc-hostname


