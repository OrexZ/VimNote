
## 如何安装docker
本文以ubuntu-18为例，参考[docker官网推荐步骤][3]。


## 如何删除docker应用及其相关内容

其实这个问题是困扰很多使用docker，并有时候会对它深恶痛疾的开发人员。

笔者遇到过docker打印log将磁盘空间填满的情况。

所以，有时候你可能需要彻底删除本地的docker，那么你可以这样做：

以ubuntu-18版本为例：

首先，查询包管理器都安装了哪些docker相关的内容

    dpkg -l | grep -i docker

然后根据你查询到的内容进行删除，大概样例如下

    sudo apt-get purge -y docker-engine docker docker.io docker-ce docker-ce-cli
    sudo apt-get autoremove -y --purge docker-engine docker docker.io docker-ce docker-ce-cli

当然这只是包管理器帮忙删除的一些套件中的内容，它并不会删除用户数据，如image，containers，volumes以及配置等等。

如果你想要删除这些残留的内容，可以使用下面的方法删除

    sudo rm -rf /var/lib/docker /etc/docker
    sudo rm /etc/apparmor.d/docker
    sudo groupdel docker
    sudo rm -rf /var/run/docker.sock


参考资料：
[skubuntu][1]和[stackoverflow][2]有一些相关的讨论，如果使用不同的发行套件，请自行查询。


[1]: https://askubuntu.com/questions/935569/how-to-completely-uninstall-docker
[2]: https://stackoverflow.com/questions/51206062/remove-docker-on-ubuntu-18
[3]: https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04
