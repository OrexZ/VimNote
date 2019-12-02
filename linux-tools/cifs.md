
## 在linux操作系统上挂载windows操作系统

有一些工具可以很好的完成标题提到的内容，不过本文主要介绍'cifs'文件系统的挂载和使用

使用手册可以在[这里][1]得到。

### 如何使用cifs文件系统挂载远程windows目录

这很简单，不过需要你的linux先支持cifs文件系统才可以，所以需要安装如下工具，以ubuntu为例

    # 这和安装samba服务差不多
    apt search cifs-utils
    >
        cifs-utils/bionic 2:6.8-1 amd64
        Common Internet File System utilities

        samba/bionic-updates,bionic-security,now 2:4.7.6+dfsg~ubuntu-0ubuntu2.13 amd64 [installed]
        SMB/CIFS file, print, and login server for Unix

        smbclient/bionic-updates,bionic-security 2:4.7.6+dfsg~ubuntu-0ubuntu2.13 amd64
        command-line SMB/CIFS clients for Unix

    sudo apt install cifs-utils

    # 确保windows侧正确的开启共享服务,cifs服务会默认的被开启

    # 挂载远程windows目录/sales到本地/mnt/cifs上，-o选项的内容可以查看前文提到的帮助手册
    # 这只是一个例子
    sudo mount -t cifs //192.168.101.100/sales /mnt/cifs -o username=shareuser,password=sharepassword,domain=nixcraft


### 如何使用密码文件避免每次输入？

    # 密码文件就是$HOME/.win-file.cerd，名字任意，密码格式瑞安
    sudo mount -t cifs -o ro,credentials=$HOME/.win-file.cerd //cnshz-nv-fl01/FILE .win-file/

    # 密码文件格式
    username=hello
    password=world

### 如何开机自动挂载文件系统？

这也很简单，这里提供一个例子

    # 在文件/etc/fstab中，添加一个挂载项，并使用指定的鉴权密码文件
    //cnshz-nv-fl01/FILE/balabala/share /home/jenkins/.win-file cifs auto,ro,credentials=/home/jenkins/.win-file.cred 0 0

[1]: https://linux.die.net/man/8/mount.cifs

