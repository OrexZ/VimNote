---
title: Multiple SSH KEYs on The Same Computer
date: 2019-02-11
banner:
tags: ssh
toc: true
---

## 需求分析

有时候，可能需要在github上创建不同的帐户（虽然是很小的概率），但如果你的主机上只有一个sshkey，就会出现问题。

问题在于这个github会查询sshkey的唯一性，并告诉你这个sshkey已经存在了。

这个时候同一台机器创建不同的sshkey就有必要了。

## 具体操作步骤

任何的操作步骤都会有假设的前提。

我们假设现在你的主机还没有配置sshkey

### 1. 为本地主机创建多个sshkey
```bash
ssh-keygen -t rsa -C "youremail@temp.com"

> -t 为参数类型
> -C 关联邮件
```

这个步骤中，会要求你配置文件的位置和名称，注意并正确填写。

对应标题，我们需要假设需要在本地创建两个sshkey达到目的，所以需要执行**两次**上述命令。

注意，执行上述命令的时候，配置的文件应该是不同的。

现在，已经有了两个不同的sshkey在~/.ssh/目录下。
查看目录结构：

```bash
ls -l ~/.ssh/
```

### 2. 配置ssh的config文件，并关联不同帐户

查看ssh的手册可以发现下面的内容：
```txt
# man ssh && grep configfile

     -F configfile
             Specifies an alternative per-user configuration file.  If a configuration file is given on the command line, the system-wide configuration file (/etc/ssh/ssh_config) will
             be ignored.  The default for the per-user configuration file is ~/.ssh/config.
```

在~/.ssh/下创建config文件并添加配置信息。

```bash
$ touch ~/.ssh/config
```

使用编辑器完成配置信息添加：

```bash
$ vi ~/.ssh/config
```

配置信息样例如下：
```config
Host github.com # 这个名字理论上可以随便配置，但最好配置有意义的内容，并且可以看到这个内容是区分不同sshkey的必要条件
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa

Host my.github.com
    HostName github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/my
```

### 3. 上传指定的sshkey到github

上川sshkey到github也很容易，这个内容可以参考链接：
[GITHUB的SSHKEY的配置](https://blog.csdn.net/itmyhome1990/article/details/39668349)

### 4. 验证配置的内容是否生效

验证指令很简单：
```bash
$ ssh -T git@my.github.com
$ ssh -T git@github.com
```
如果出现Hi xxx!You've successfully authenticated 就说明连接成功了。


### 5. git仓库的配置

需要知道的是，本地关于ssh的相关操作，都是基于本地的配置完成的。这很重要！

这里需要注意的是，用户名和邮件的配置信息是需要使用git的指令完成配置的。
用户信息可以是本地仓库的，也可以是针对个别用户的，也可以是全局的。
具体的内容请参考：[git config 配置](https://blog.csdn.net/joe_007/article/details/7276195)

如果想要免密操作，那么clone自己的仓库就使用git协议，如：
```bash
git clone git@my.github.com:username/wiki.websit.git
```

如果仓库已经clone或者创建完成，那么可以修改本地的仓库完成配置，如：
```bash
#更改[remote "origin"]项中的url中的(.git/config)
#my.github.com 对应上面配置的host
[remote "origin"]
	url = git@my.github.com:itmyline/blog.git
```
或者：
```bash
#在Git Bash中提交的时候修改remote
$ git remote rm origin
$ git remote add origin git@my.github.com:username/blog.git
```

## 扩展内容

在使用hexo完成博客编写的时候，需要配置web中的 _config.yml文件的内容如下：
```config
...
deploy:
  repo: git@cool.github.com:oamsocool/oamsocool.github.io.git
  type: git
  branch: master
...
```
