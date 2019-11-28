
## 使用git清理工作空间中的未跟踪文件或目录

首先查看当前工作目录下有哪些文件或者目录需要清理

    git clean -n

然后，有一些针对性的操作能够满足你的需求：

    #只删除文件
    git clean -f

    #删除目录和文件
    git clean -fd

    #删除忽略的文件
    git clean -fX

    #删除忽略的文件和没有忽略的文件
    git clean -fx

    #删除忽略的和没有忽略的文件和目录
    git clean -fdx

另外，如果你使用repo管理仓库，可以这样使用来清理你不想要的所有内容：

    repo forall -c git clean -fdx

详细内容请参考[git-clean][1]和[stackoverflow的回答][2]

[1]: https://git-scm.com/docs/git-clean
[2]: https://stackoverflow.com/questions/61212/how-to-remove-local-untracked-files-from-the-current-git-working-tree/20846779

