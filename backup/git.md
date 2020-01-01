---
title: CentOS Git
toc: true
p: Linux/CentOS/git.md
date: 2019-02-22 22:09:01
tags:
categories:
---

## How-to Install The Latest Version

If you want get the latest version of git tools on CentOS, the one method of all is compile with source code.
[git repository](https://github.com/git/git)

Here are steps of installing git tools.

### Install Development Groups Tools

```bash
sudo yum groups list # query
sudo yum group install 'Development and Creative Workstation'
```

### Remove Old Version Git From System

```bash
sudo yum remove git
```

### Maybe Other Dependence

Maybe some other tools should be installed:
```bash
sudo yum install  curl-devel \
                  expat-devel \
                  gettext-devel \
                  openssl-devel \
                  zlib-devel \
                  gcc \
                  perl-ExtUtils-MakeMaker \
                  docbook2X.x86_64 \
                  curl-devel \
                  openssl-devel \
                  asciidoc.noarch
```

### Download Source Code

download source code methods as below:

1. clone source code from Github:
```bash
git clone https://github.com/git/git.git # to current dir
```

2. get source code zip-file from Github:
```bash
wget https://github.com/git/git/archive/master.zip # to current dir
```

### Compile Source Code and Install

```bash
sudo make prefix=/usr install install-doc install-html install-info ;# as root
```
[official installation documentation](https://github.com/git/git/blob/master/INSTALL)

### Verify

```bash
git --version
```

## Reference
- [blog about install git from source code ](https://www.cnblogs.com/hglibin/p/8627975.html)

