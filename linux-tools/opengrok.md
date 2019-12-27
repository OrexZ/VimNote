
## OpenGrok 安装和使用

[这里][1]有同学已经将OpenGrok环境集成到了docker中，使用起来也比较简单

首先，自己下载docker image到本地，这里需要配置一些本地工具

依赖的工具：JAVA （这不用多说了，安装个JDK1.8就可以满足）
一般的包管理器都可以直接安装，可以就不再细述

安装脚本为：

    docker run -it -d --name <CONTAINER NAME> \
	-p <EXTERNAL PORT>:8080 \
	-v <LINUX JDK HOME>:/usr/lib/jvm \
	-v <OPENGROK DATA FOLDER>:/var/opengrok:delegated \
	-w /app krazakee/opengrok


```text
<EXTERNAL PORT>: 主机端口，由docker中端口8080映射过来，并给外部使用的
<LINUX JDK HOME>: JDK的HOME目录，一般在/lib/下面，可以使用locate定位
<OPENGROK DATA FOLDER>: 索引文件和项目代码存放的位置
```

创建和更新索引

这也是使用这个工具的目的，这个可以使用这位同学已经做好的脚本来完成

	docker exec <CONTAINER NAME> bash -c "/reindex.sh"

```text
<CONTAINER NAME> : 就是你之前run的时候分配的名字，当然ID也是可以的
```

[1]: https://zhuanlan.zhihu.com/p/32568717
