
## C语言中的`悬挂式else`


C语言并不像python这类语言有缩进的限制，所以也就不存在`悬挂式else`

那么什么是`悬挂式else`，我们先看下例子

首先说明，下面例子中的代码，都是能够在笔者主机平台(x86)使用gcc编译通过，并且每部分都可以正常运行的。

为了说明问题，并且聚焦问题，并没有使用宏来简化代码。


    #include <stdio.h>
    #include <string.h>

    static void test_entry_01(int a, int b){
        if (a)
            if (b)
                printf("%s> b if\n", __func__);
            else
                printf("%s> b else\n", __func__);
            else
                printf("%s> who am I ? # a if ??? yes\n", __func__);
    }

    static void test_entry_02(int a, int b){
        if (a)
            if (b)
                printf("%s> b if\n", __func__);
        else
        {
            printf("%s> a else ? no I'm b else.", __func__); // no new line
            printf("more info...\n");
        }
            else
                printf("%s> who am I ? # I'm a else.\n", __func__);
    }

    static void test_entry_03(int a, int b, int c){
        if (a)
            printf("%s> nothing to be done...\n", __func__);
            if (b)
                printf("%s> b if, but not nest in a.\n", __func__);
            else
                printf("%s> b else ? yes\n", __func__);
                if (c)
                    printf("%s> who am I? I don't belong to any of the control blocks above.\n", __func__);
        else
            printf("%s> who am I? I'm c else.\n", __func__);
    }

    int main( int argc, char **argv){

        int a, b, c;
        if (argc != 4)
        {
            printf("Usage: <APPNAME> a b c\n");
            return 0;
        }

        a = strcmp(argv[1], "1") == 0 ? 1: 0;
        b = strcmp(argv[2], "1") == 0 ? 1: 0;
        c = strcmp(argv[3], "1") == 0 ? 1: 0;
        printf("a=%d, b=%d, c=%d\n", a,b,c);

        test_entry_01(a,b);
        test_entry_02(a,b);
        test_entry_03(a,b,c);

        return 0;
    }


编译并查看输出：

    $ gcc -o test ./test_else.c
    $ ./test
    > Usage: <APPNAME> a b c
    $ ./test 1 1 1
    >
    a=1, b=1, c=1
    test_entry_01> b if
    test_entry_02> b if
    test_entry_03> nothing to be done...
    test_entry_03> b if
    test_entry_03> who am I? I don't belong to any of the control blocks above.

这里并没有列出所有的情况，感兴趣的小伙伴可以自己编译试试。

其实printf包含的字符串已经说明了所有相关的可能。


小结：

1. 悬挂式else的语法并不具有很好的可读性，请在使用C语言的时候加上控制块`{}`
2. 添加控制块`{}`后，可以避免你认为的这种歧义性
3. if-else在不添加控制块符号`{}`的时候，表示后面很简短，简短到只能允许一条指令
4. 请在书写代码的时候注意缩进，虽然C语言很自由，但请体谅有可能阅读你代码的其他人。


## 如何写出好看的C语言if-else控制流

当你阅读比较好的C项目代码的时候，你可能会感慨为何人家的代码写的这么好，一看就懂，或者控制流程很是流畅？

这个时候你其实应该停下你的动作，等等你的灵魂，仔细花时间琢磨下为什么他们写的好。


我写一个例子，看看为什么这样写


    #include <stdio.h>
    #include <string.h>

    #define TRUE  1
    #define FALSE 0


    int do_it_01(int cond){
        if (cond != 1)
        {
            printf("%s> normal branch\n", __func__);
            return TRUE;
        }
        else
        {
            printf("%s> exception branch\n", __func__);
            return FALSE;
        }
    }

    int do_it_02(int cond){
        if (cond != 2)
        {
            printf("%s> normal branch\n", __func__);
            return TRUE;
        }
        else
        {
            printf("%s> exception branch\n", __func__);
            return FALSE;
        }
    }

    int do_it_03(int cond){
        if (cond != 3)
        {
            printf("%s> normal branch\n", __func__);
            return TRUE;
        }
        else
        {
            printf("%s> exception branch\n", __func__);
            return FALSE;
        }
    }

    int do_it_04(int cond){
        if (cond != 4)
        {
            printf("%s> normal branch\n", __func__);
            return TRUE;
        }
        else
        {
            printf("%s> exception branch\n", __func__);
            return FALSE;
        }
    }

    int deal_exception(){
        printf("deal exception...\n");
        return TRUE;
    }

    int main (int argc, char ** argv){
        int cond;
        static int exception = 1;

        if (argc != 3){
            printf("Usage: <appname> cond exception\n");
            return 0;
        }

        // simple implement, just for test.
        cond = *(argv[1]) - '0';
        exception = *(argv[2]) - '0';
        printf("cond: %d, exception: %d\n", cond,exception);


        if (!do_it_01(cond))
        {
            printf("do_it_01 err ? do some thing.\n");
            return -1;
        }
        else if (!do_it_02(cond))
        {
            printf("do_it_02 err ? do some thing.\n");
            return -2;
        }
        else if (!do_it_03(cond))
        {
            printf("do_it_03 err ? do some thing.\n");
            return -3;
        }
        else if (!do_it_04(cond))
        {
            printf("do_it_04 err ? do some thing.\n");
            return -4;
        }
        else
        {
            if (exception){
                deal_exception();
                return -5;
            }
            else
            {
                printf("default..., do some process..\n");
            }
        }

        return 0;
    }


执行与输出结果，同学可以自行尝试，本文代码可以正常编译运行

    $ ./test 5 0
    >
    cond: 5, exception: 0
    do_it_01> normal branch
    do_it_02> normal branch
    do_it_03> normal branch
    do_it_04> normal branch
    default..., do some process..
    $ ./test 5 1
    >
    cond: 5, exception: 1
    do_it_01> normal branch
    do_it_02> normal branch
    do_it_03> normal branch
    do_it_04> normal branch
    deal exception...
    $ ./test 3 0
    >
    cond: 3, exception: 0
    do_it_01> normal branch
    do_it_02> normal branch
    do_it_03> exception branch
    do_it_03 err ? do some thing.

这里没有列出所有的输出结果，不过已经可以从源码上能够说明问题。

这样写的好处有哪些：
1. 易读，流程一条线，异常很明显，都是在判断失败后处理错误逻辑
2. 函数设计结构统一，返回值是精心设定的
3. 好看的输出格式

设计要点：
1. 前文提到，函数的返回值需要精心设计，当然，如果返回的TRUE、FALSE不能满足，其实其他的值也是可以的，只不过要多加判断
2. 书写逻辑一条线，异常都在判断if失败的时候处理




