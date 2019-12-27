

## 如何安装FZF

如果你想要方便省事，可以这样来

    $ git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf

    Cloning into '/home/rex/.fzf'...
    remote: Enumerating objects: 95, done.
    remote: Counting objects: 100% (95/95), done.
    remote: Compressing objects: 100% (88/88), done.
    remote: Total 95 (delta 4), reused 22 (delta 2), pack-reused 0
    Unpacking objects: 100% (95/95), done.

    $ ./.fzf/install

    Downloading bin/fzf ...
    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
    Dload  Upload   Total   Spent    Left  Speed
    100   616    0   616    0     0    550      0 --:--:--  0:00:01 --:--:--   550
    100 1134k  100 1134k    0     0   277k      0  0:00:04  0:00:04 --:--:--  508k
    - Checking fzf executable ... 0.20.0
    Do you want to enable fuzzy auto-completion? ([y]/n) y
    Do you want to enable key bindings? ([y]/n) y

    Generate /home/rex/.fzf.bash ... OK

    Do you want to update your shell configuration files? ([y]/n) y

    Update /home/rex/.bashrc:
    - [ -f ~/.fzf.bash ] && source ~/.fzf.bash + Added

    Finished. Restart your shell or reload config file.
    source ~/.bashrc  # bash

    Use uninstall script to remove fzf.

    For more information, see: https://github.com/junegunn/fzf


当然有很多的方法可以安装，具体可以查看官方的安装[章节][1]。

[1]: https://github.com/junegunn/fzf
