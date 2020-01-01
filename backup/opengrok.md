---
title: CentOS opengrok
toc: true
p: Linux/CentOS/opengrok.md
date: 2019-02-23 18:17:01
tags:
categories:
---

## Install OpenJDK

install by yum
```bash
sudo yum -y install java-1.8.0-openjdk java-1.8.0-openjdk-devel
```

location:
```bash
dirname $(readlink $(readlink $(which java)))
```


## Install OpenGrok docker

{% post_link Linux/CentOS/docker install docker click here %}

```bash
sudo docker pull krazakee/opengrok #pull a opengrok-repo
```

## Privatize the JDK To the User Directory

NOTE: please copy all files to path you want, not Link files
so you should do this with '-L' option:
```bash
bash cp -arL `dirname $(readlink $(readlink $(which java)))` /path/to/jvm
```


## Make Script

run docker daemon for opengrok
```bash
docker run -it -d --name openg \
            -p 8111:8080 \
            -v /path/to/jvm:/usr/lib/jvm \
            -v /path/to/opengrok/data:/var/opengrok:delegated \
            -w /app krazakee/opengrok
```

update index DB-data
```bash
docker exec openg bash -c "/reindex.sh"
```

NOTE: your source code should be put to /path/to/opengrok/data/src/

for example: 
```bash
cd /path/to/opengrok/data/src/flask

git clone https://github.com/pallets/flask.git

docker exec openg bash -c "/reindex.sh"
```

## Reference
- [Good Blog form zhihu](https://zhuanlan.zhihu.com/p/32568717)
