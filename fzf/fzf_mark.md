---
title: fzf mark工具的使用
toc: true
date: 2019-01-10 20:24:26
categories: fzf
---

大家好，我是`红桃C`。
这回介绍一款也是利用fzf操作目录书签的工具`fzf-mark`。

## 什么是fzf-mark工具

本质上来说，这个工具是利用fzf提供的索引功能，扩展相关书签的能力。

正确的利用它可以提高工作效率。

官网入口位置：https://github.com/urbainvaes/fzf-marks

安装方法按照官方提供的教程来操作即可，该工具支持了bash和zsh(还有fish etc...)，
本文以bash为例，介绍安装和使用

## 如何安装bash环境下的fzf-mark工具

    # Clone the git repository in the current directory
    git clone https://github.com/urbainvaes/fzf-marks.git

    # Add a line to ~/.bashrc to load the plugin whenever bash starts in interactive mode
    echo "source $PWD/fzf-marks/fzf-marks.plugin.bash" >> ~/.bashrc

    # Source the plugin now so we don't have to restart bash to start using it
    source fzf-marks/fzf-marks.plugin.bash

## 如何使用fzf-mark工具

使用这个工具也很简单，它提供了两个基础的功能

1. 添加一个目录书签mark，只是简单的将作者自己设计的格式写到由FZF\_MARKS\_FILE指定的文件
2. 之后，利用快捷键`C-g`或者`fzm命令`调用fzf读取文件书签并跳转

简单概括就是跳转到指定的目录，然后执行`mark`，之后，到任何地方，只要你运行`fzm`或者`C-g`就可以列出并跳转。

当然，这个工具还定制了一些fzf操作书签的功能，如

1. `C-y`确认并跳转，我建议你忘记这个，直接使用`Enter`按键，也就是回车就可以了
2. `C-t`选择的功能，可以实现复选功能，不过你需要知道，在按照某些标准的键盘上，`C-t`和`Tab`还有`C-i`都是一个意思，所以你只要记住 `Tab`就行了
3. `C-d`删除所选的书签，就这样。

## 关于fzf-mark的定制

关于定制，其实没有什么，这个简单的工具已经可以满足我们大部分的要求了，如果可以的话，可以想办法和其他工具整合，变得更好

这里给出官方的一写参考设计内容，之列出一些感兴趣的选项

    | Config                  | Default                         | Description                          |
    | ------                  | -------                         | -----------                          |
    | `FZF_MARKS_FILE`        | `${HOME}/.fzf-marks`            | File containing the marks data       |
    | `FZF_MARKS_COMMAND`     | `fzf --height 40% --reverse`    | Command used to call `fzf`           |
    | `FZF_MARKS_JUMP`        | `\C-g` (*bash*) or `^g` (*zsh*) | Keybinding to `fzm`                  |


好了，到这里就介绍完了，希望可以帮助到你^\_^.
