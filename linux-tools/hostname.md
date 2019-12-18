
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

