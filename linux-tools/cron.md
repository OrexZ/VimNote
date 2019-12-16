
## 定时执行任务 crontab

很多时候，你需要定时执行很多重复的任务，这个时候，crontab工具就会发挥它的作用。

crontab是一个定时调度指定任务的工具，它有自己的守护进程，你只需要定制自己的work给它完成，就这么简单。

网络上有很多比较好的资源可以[参考][1]，也可以使用man手册查看帮助。

这里列出一个简单的例子：

    # Cron-jobs for jenkins backup.
    SHELL=/bin/bash
    PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

    10 1 * * * bash /home/rex/projects/search-web/search_job.sh


[1]: https://blog.csdn.net/ithomer/article/details/6817019

