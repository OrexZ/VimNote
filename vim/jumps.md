
## VIM 中使用`jumps`命令的技巧

很多人都不太清楚VIM内置的jumps命令应该怎么用？

其实很简单，使用`jumps`命令查看跳转位置堆栈表，然后使用`<c-o>` 和 `<c-i>`配合`<num>`跳转到指定位置。

如：
```text
   2     1    0 ~/VimNote/vim/vim-grep.md
   1     5    0 ~/VimNote/vim/jumps.md
>  0   234   20 boolean qbi_qmux_is_qmi_ctl_request
   1   237    0 uint32      len
   2  1142    6 ~/tmp/mbim/qbi/platform/src/qbi_hc_linux.c
```

'>'表示当前光标位置，只要你看到想要去的地方。
使用`<num>` + `<c-o>`，如`2<C-o>`就跳到`~/VimNote/vim/vim-grep.md`。
使用`<num>` + `<c-i>`，如`1<C-i>`就跳到`uint32      len`。

## 不得不提的插件

`vim-bookmarks`

这个插件真的很不错，强烈推荐。

获取位置[在这里][1]


[1]: https://vimawesome.com/plugin/vim-bookmarks

