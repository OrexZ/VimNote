
# 使用linux操作系统的时候，发现磁盘空间爆满，无法正常使用或者启动系统，怎么办？

最近碰到一个问题，突然好好的工作电脑就没有办法正常运行了，会提示类似没有磁盘空间的警告。
这时候萌新一定会重新启动，不过这都是徒劳的，并且你很可能会看到系统卡在某个阶段，一直进不去系统。

一般这种问题请不要慌张，这很是linux系统磁盘空间不足导致的常见问题，解决步骤如下：

> 0. 如果你重启后进不去系统，也没有熟悉的桌面系统，请使用linux的tty控制台吧，就像终端一样使用它就可以。

    <Ctrl> + <ALT> + F1/F2/.../F5 #随你挑选
    # 输入用户名和密码开始下面的步骤。

> 1. 查看本机挂载的哪个磁盘空间已满，一般因该都是系统盘，就是挂载'/'的磁盘。

    df -lh
    mount

> 2. 根据查看到的已经爆满的系统盘，进行遍历搜索大文件

    find / -size +400M -exec ls -lh {}\; #一般不知道具体位置的时候，使用这样的搜索
    find /var -size +400M -exec ls -lh {}\; #经验来讲，一般只有系统日志信息会无怨无官的占用这么大的空间(/var/log)

> 3. 找到大文件后，自己斟酌是否应该删除，如果是log文件，那就果断干掉它，笔者就碰到了大文件log (/var/log/syslog.1)有350G大小。

> 4. 同步磁盘后重新启动计算机即可(其实不用重新启动，只不过强迫症会逼着你做的)。

    sync && reboot (sudoer)


NOTE: 不过一般不建议直接删除掉log文件或者较大的文件，至少应该知道是什么原因引起的问题。

笔者蹦到的问题是dockerd不断喂log给syslogd，导致log文件过大。

可以使用tail -f /var/log/syslog 看到到底谁在不断的扩大log，找到元凶才是正道。

由于笔者使用snap和apt都安装过docker，出现了一些奇怪的问题，所以需要snap和apt都删除掉docker，然后下载干净的docker方可。


关于如何关闭并移除Docker的讨论：

StackOverflow的一些回答
https://stackoverflow.com/questions/40192415/how-permanently-kill-dockerd
AskUbuntu的回答
https://askubuntu.com/questions/935569/how-to-completely-uninstall-docker
