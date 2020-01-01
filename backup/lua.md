---
title: Lua
toc: true
date: 2019-07-21 14:47:04
tags:
categories:
---

本文主要介绍关于Lua语言的使用技巧和设计细节。


## Lua如何安装？

Lua语言的官方网站：https://www.lua.org/

安装的步骤简记如下：
编译安装时需要注意依赖的库 *readline*

```lua
# if on Centos
yum install  readline-devel

# if on Ubuntu or Debian
apt-get install libreadline-dev


curl -R -O http://www.lua.org/ftp/lua-5.3.5.tar.gz
tar zxf lua-5.3.5.tar.gz
cd lua-5.3.5
make linux test
```

## Lua 是否可以交互？

使用过lua的程序员可能都清楚，lua是一门解释型语言，提供解释器，并且提供交互式编程接口。
你只需要使用简单的敲击lua就可以进入交互模式。


## 如何修改Lua交互模式的提示符？

修改lua交互模式的提示符，可以简单的定义一个全局变量来进行处理：

第一种：
```bash
lua -e "_PROMPT=' lua> '" -i
```

第二种：
写一个lua文件，内容如下：
```lua
-- filename: prompt.lua
_PROMPT = ' lua> '_
```
然后运行文件并切到交互式模式：
```bash
lua -i prompt.lua
```

第三种：
使用lua默认的环境变量进行前置代码运行，这个环境变量是解释器所认识的内容，这里先不用关心。
环境变量为 LUA_INIT, 具体步骤如下：
```bash
(export LUA_INIT="_PROMPT=' lua> '"; lua -i) #注意这里的分号，需要明确-i参数的所属，否则保错,外面的括号是shell隔离环境的好办法
```

或者

```bash
export LUA_INIT="@/path/to/prompt.lua"
lua -i
```

撤销动作如下：
```bash
unset LUA_INIT
```

## Lua中打印环境arg表的一些怪异现象？

lua默认的命令行参数会存储在arg table中，而使用这个表的时候往往新手会有点混乱，实例如下：
测试程序：
```lua
-- filename: arg.lua
print("arg: ", arg)
pl = string.rep('-', 20)

function show_all_args_v1(a)
    for i,v in ipairs(a) do
        print("[#",i,"]:",v)
    end
end

function show_all_args_v2(a)
    for k,v in pairs(a) do
        print("[#",k,"]:",v)
    end
end

print(pl,"may be you saw as below",pl)
print("total number of arg:", #arg)
show_all_args_v1(arg)

print(pl,"real arg as below",pl)
local len = function (a) local i=0 for _,_ in pairs(a) do i=i+1 end return i end

show_all_args_v2(arg)
```

测试结果如下：
```bash
(base) ➜  lua-learning git:(master) lua ./arg.lua a b c
arg: 	table: 0x20199f0
--------------------	may be you saw as below	--------------------
total number of arg:	3
[#	1	]:	a
[#	2	]:	b
[#	3	]:	c
--------------------	real arg as below	--------------------
total number of arg:	5
[#	1	]:	a
[#	2	]:	b
[#	3	]:	c
[#	0	]:	./arg.lua
[#	-1	]:	lua


(base) ➜  lua-learning git:(master) lua -e "just_for_test=100" ./arg.lua a b c 
arg: 	table: 0xc149f0
--------------------	may be you saw as below	--------------------
total number of arg:	3
[#	1	]:	a
[#	2	]:	b
[#	3	]:	c
--------------------	real arg as below	--------------------
total number of arg:	7
[#	1	]:	a
[#	2	]:	b
[#	3	]:	c
[#	0	]:	./arg.lua
[#	-3	]:	lua
[#	-2	]:	-e
[#	-1	]:	just_for_test=100

```

解释：
一般看到上面的程序和验证结果就已经了解了大概的端倪，这里做下简单的分析。
1. 首先lua中的table是一个符合型的结构，他可以存储所谓的数组或者字典(关联数组)。
2. lua解释器会把所有的命令行参数全部收集起来放在arg table中。
3. lua中table其实是关联数组，也就是hash速查表的实现方式，之所以可以使用数组，那时因为table认识1,2,3这样的key，并且明白如何解析。
4. arg table中参数是按照‘顺序’排列的，并且看起来可以按照数组的形式引用它们。
5. ipairs和pairs的区别也很明显，一个计算从数字1开始的能够按照数组样式索引的函数：ipairs，另一个是索引标准hashtable的key:value方法：pairs
6. arg[0]表示脚本名字，也是一个看似的分界线，并且复数的索引也是有规律可寻的。



## 用在命令行参数解析时的三个点(...)？

lua提供一种可变参数的支持，和C语言很像，在命令行传入参数的时候也可以使用它来替代arg table，不过有一些需要注意的地方。
```lua
-- filename: arg_for_tree_points.lua
print("... content:", ...)
print("... type:", type(...))

local function show_args(t)
    for i,_s in ipairs(t) do
        print("[",i,"]:",string.byte(_s,1,#_s))
    end
end

show_args({...})
```
```bash
(base) ➜  lua-learning git:(master) lua ./arg_for_tree_points.lua a b c          
... content:	a	b	c
... type:	string
[	1	]:	97
[	2	]:	98
[	3	]:	99
```

上面的内容看起来还不错，不过它隐藏了一个bug，当不传入参数的时候，就会保错
```bash
(base) ➜  lua-learning git:(master) lua ./arg_for_tree_points.lua
... content:
lua: ./arg_for_tree_points.lua:3: bad argument #1 to 'type' (value expected)
stack traceback:
	[C]: in function 'type'
	./arg_for_tree_points.lua:3: in main chunk
	[C]: in ?
```

这是因为我们没有传入参数，那么程序里的三个点根本没有被使用，错误是由lua虚拟机基础C代码报出来的，所以尽量使用arg作为脚本参数解析原料。



## 先与脚本文件的运行技巧

有时候，我们想要在运行脚本前运行一些其它前置条件或者陪置内容，lua中提供了一些方法可以尝试，这里列出两种：

第一种：
使用命令行传递lua代码
```lua
-- filename: which_first.lua
print(Iam)
```

```bash
(base) ➜  lua-learning git:(master) lua -e "Iam=100" ./which_first.lua
100
```
第二种：
使用环境变量LUA\_INIT做preload定制
```lua
-- filename: test_preload.lua
print(preload_var)
```
```bash
(base) ➜  lua-learning git:(master) (export LUA_INIT="preload_var = 'so....easy'"; lua test_preload.lua)
so....easy
```
或者

```bash
(base) ➜  lua-learning git:(master) (export LUA_INIT="@/home/rex/mz/mz-lua/self/lua-learning/preload_from_LUA_INIT.lua";lua test_preload.lua)
so....easy
```

## lua中的真与假

在lua中，空字符串和数字0都表示真，这和其它语言不同语言注意。

那么在lua中，只有 false和nil被视为假，其它的内容都是真～

所以，在lua判断中，最好显示的比较内容，不要取巧。



## 在模式中的引号

lua没有字符串和字符的区别，当然如果你深入后，可以自己实现。
不过通常来说最好按照统一的风格来书写代码。

这里强调在模式中，一般引号并不是使用％来转义，而是使用标准的lua程序的\\来转义。

```bash
> string.match("sss\"mmmm", "^[^h].-\"%a*$")
sss"mmmm
```

## 在lua中的数组索引

lua中数组索引从1开始，这已经是惯例，不过如果你想要从0开始构造一个table也是可以的，如：
```lua
> t = { [0] = 'one', 'two', 'three'}
> t

> #t
2

> t = { 'one', 'two', 'three'}
> #3

> #t
3

```
从0开始是可以，但如果这样你就没有办法享受lua提供的各种内置功能，比如这里的数组长度，为2而不是3.


## lua 中的变量的作用域，生命周期，存储空间

这里先分析作用域，后虚内容再补上。
通过例子来看应该是最好的，那么接下来看看lua怎么使用变量的

变量的遮蔽
```lua
-- filename: block.lua

a = "global a"
local a = "local a"
print(a)
```
```bash
(base) ➜  lua-learning git:(master) lua ./block.lua
local a
```
这里如果要使用全局变量a，只能在使用前保存，后续再恢复。

lua对于代码快的界定和其它语言差不多，如：
```lua
-- filename: block.lua

a = "global a"

do
    local a = "in do"
    print(a)
end
print(a)

function sample()
    local a = "sample"
    print(a)
end
sample()
print(a)

function out()
    local out_a = "out_a"
    function _out()
        local out_a = "_out_a"
        print(out_a)

        if true then
            out_a = "in if"
            print(out_a)
        end
        print(out_a)

    end
    _out()
    print(out_a)
end
out()

x = 10
local i = 8
while i < x do - 这里的x是global的，而不是里面的local，被记住了
    local x = 11
    print(i)
    i = i + 1
end

a = { 10, 20, 30 }
local i = 2
while a[i] do -- 注意这里的索引跟着变化了
    print(a[i])
    i = i + 1
end

local c = 100
repeat
    c = 1000
    local m = 999
until m < c -- 这里的m可以使用
print(c)

while m < 10 do -- 这里的m不能用
    local m = 9
    m = m + 1
    print(m)
end
```

```bash
(base) ➜  lua-learning git:(master) lua ./block.lua
in do
global a
sample
global a
_out_a
in if
in if
out_a
8
9
20
30
1000
lua: ./block.lua:58: attempt to compare nil with number
stack traceback:
	./block.lua:58: in main chunk
	[C]: in ?

```

## Lua中的break和retuen语句

由于语法构造的原因，在lua中break与return必须是程序块的最后一条语句；
当我们需要在程序中间return或者break的时候，可以使用下面的技巧：

```lua
function foo ()
  return -- fail
  do return end -- nice
end
```

## Lua中的形参作用域

在lua中，形参的作用域和其它语言一样，被限定为本地作用域。
只所以强调这个，也是为了对前文的变量作用域一个补充。


## 形参与实参的对应关系
实参和形参的对应关系如下：
```lua
function f(a, b) return a or b end

f(3) -- a = 3, b = nil
f(4,5) -- a = 4, b = 5
f(3,4,5) -- a = 3, b = 4, 5 被丢弃
```

## 函数的多重返回值

在lua中，函数可以返回多个值，它的描述应该是这样的：

1. 如果只是作为独立的表达式调用，那么所有返回值被丢弃

    string.find("hello", "he")
    
2. 如果函数调用作为表达式的一部分，并且不是表达式需要的最后一个参数的时候，只返回第一个值，其它的值被丢弃

    print("test",string.find("hello", "h"),100)
    
4. 如果函数调用作为表达式的一部分，并且是表达式需要的最后一个，或者唯一一个参数的时候，它将返回所有的返回值

    print(string.find("hello", 'h'))

函数和变量是第一元素，可以看成是同等级别，函数的返回值也和变量一样，也有多值分配功能：
```lua
start, end = string.find("hello", 'h')
start = string.find("hello", 'h')
start, end, nothing = string.find("hello", 'h'), "this is nothing"
t = { string.find('hello', 'h') }
t = { 100, string.find('hello', 'h'), 200}
```

## Lua中函数的重载？

在lua或者python这类语言中，没有函数重载的功能，但有了可变参数。

如果复写了函数会怎么样呢？我们看看：
```lua
-- filename: override.lua
function hello ()
    return "hello 1"
end

function hello ()
    return "hello 2"
end

print(hello())

local function hello ()
    return "hello 3"
end

print(hello())

do
    local function hello ()
        return "hello 4"
    end
    print(hello())
end
print(hello())
```

```bash
(base) ➜  lua-learning git:(master) lua ./override.lua
hello 2
hello 3
hello 4
hello 3
```

## Lua中的可变形参，三个点的使用

在lua中支可变参数，使用方法和C语言一致，不过需要注意的是：
1. 三个点是一个表达式，并且表示的是一个string类型变量
2. 使用type时，如果没有参数给它，便会保错
3. 支持string类型的所有操作

来个例子：
```lua
-- filename: var.lua
function test(...)
    return ...
end
print( test(19,20,21) )
```

```bash
(base) ➜  lua-learning git:(master) lua ./var.lua 
19	20	21
```

一般三个点符号可以配合table的构造式一起使用：
```lua
t = {...}
```

在变长参数中可能会包含很多的值是nil的情况，这时候，需要使用内置的select函数来完成任务，过滤掉不要的nil参数
```lua
-- filename: var2.lua

function how_to_use_select(...)
    for i=1, select('#',...) do
        local a = select(i, ...)
        print('pop one: ',a)
        print(select(i,...))
    end
end

how_to_use_select('a','b', 100, nil, 'm', nil, 'hello')

function how_to_use_select2(...)
    local tt = {select(1, ...)}
    print(type(tt))
    for k,v in pairs(tt) do
        print(k,v)
    end
end

print("@@ next test:")
how_to_use_select2('a','b', 100, nil, 'm', nil, 'hello')
```

```bash
(base) ➜  lua-learning git:(master) lua ./var2.lua
pop one: 	a
a	b	100	nil	m	nil	hello
pop one: 	b
b	100	nil	m	nil	hello
pop one: 	100
100	nil	m	nil	hello
pop one: 	nil
nil	m	nil	hello
pop one: 	m
m	nil	hello
pop one: 	nil
nil	hello
pop one: 	hello
hello
@@ next test:
table
1	a
2	b
3	100
5	m
7	hello
```

分析：
1. select能够过滤掉nil类型的名字
2. select 能够根据传入不同的参数，给出不同的动作
3. 上面的例子中，select(i,...)是返回从i开始到最后的所有参数，不过通过函数返回赋值操作，只有第一个被用了，其它的被扔掉
4. 传入 -1 时，从后向前排列参数


## Lua中的命名参数

在lua中，没有像python一样的命名参数机制，不过，可以变换一种方法，也可以达到一样的效果，如：
```lua
function windows(t)
if t.name == 'tony' then print('tony trap') end
if t.hello == 'world' then print('world trap') end
end
windows {hello = 'world', name = 'tony'} -- 注意这里的写法：在lua中，如果函数的参数只有一个，调用的时候可以免去括号
```

## Lua中的函数语法糖

在lua中，函数被设计成为第一类元素，和其它变量是等价的，这和python不同，而且函数可以存在任意位置，主要体现在匿名函数.
简单的转换如下：
```lua
function foo () return 100 end
-- equal
foo = function () return 100 end

local function demo () return 101 end
-- equal
local demo = function () return 101 end
```

由于这样的特性，lua既然可以在任意的地方使用，那么也可以在table中使用，这也是它最强大的地方之一，可以用这个特性来实现很多功能：
－－ 面向对象和模块编程

## 什么叫‘词法域’？什么又是闭包？

如果将一个函数写在另一个函数中，那么这个内部函数可以访问外部函数的局部变量（当然也可以是全局变量，不过尽量不要使用），这项特性就叫‘词法域’，而这也是为闭包提供的理论基础。

简单实例：
```lua
-- filename: closure.lua

foo = "hello, tom"
foo2= "hello, rex"

function demo ()
    local so = "easy"
    return function ()
        print(so)
        print(foo)
        print(foo2)
        foo = 100
        local foo2 = 100
        print(foo)
        print(foo2)
    end
end

demo()()
print(foo)
print(foo2)

do
    dd = 'hello dd'
    hello = function () print('hello func') end
end
print(dd)
hello()
```
```bash
(base) ➜  lua-learning git:(master) lua ./closure.lua
easy
hello, tom
hello, rex
100
100
100
hello, rex
hello dd
hello func
```

函数是闭包closure的一个特例，在lua实现中，并没有使用function，而是使用closure来表示函数或者说闭包。

在前面的实例中有很多内容发人思考，这里再简单做下分析：

闭包是由一个函数和它所能访问的所有非局部变量组成的，这里的非局部变量在lua中叫做upvalue，这里也需要中点的说明下，
看个例子：
```lua
-- filename: closure2.lua

function getID(index)
    return function ()
        index = index + 1
        return index
    end
end

f = getID(1)
print(f())
print(f())
print(f())

f2 = getID(1)
print(f2())

function newCounter()
    local i = 0
    return function ()
        i = i + 1
        return i
    end
end

c = newCounter()
print(c())
print(c())

c2 = newCounter()
print(c2())
```

在这个例子中，index不是内部函数的局部变量，也不是全局变量，在lua中它叫非全局变量，术语为upvalue，
虽然函数getID调用完成后，变量的作用域已经跳出来了，可是index仍然被存储保留着，这就是闭包的强大特性。
同样的，例子中local i也是这样的upvalue，和index是一样的作用，这个闭包closure特性是经过考验的编程技巧，所以lua也这样的设计它。


在lua程序设计中有一个例子很有意思，需要这里重点解释下：
```lua
- filename: closure3.lua

do
    dd = "hello dd"
    say = function () print("good morning") end
end
print(dd)
say()


oldSin = math.sin
math.sin = function (x)
    return oldSin(x*math.pi/180)
end

print(math.sin(10))

math.sin = oldSin

do
    local oldSin = math.sin
    local k =math.pi/180
    math.sin = function (x)
        return oldSin(x*k)
    end
end

print(math.sin(10))
```

```bash
(base) ➜  lua-learning git:(master) lua ./closure3.lua
hello dd
good morning
0.17364817766693
0.17364817766693
```

例子中dd和say是属于do-end块的内容，但可以在文件全局范围内访问，如果不想在全局的块中访问，那么可以使用local限定。
第二个是使用do-end实现闭包的概念，完全的隔离旧版本的math.sin而使用封装过的函数，没有想到可以使用do-end配合函数使用closure！

```lua
-- filename: nonlocal_func2.lua

local m = function ()
    return 100
end

local w = function ()
    return 200
end
```
这也是一个闭包的例子，这个例子说明了下面的方式是可以正确使用的，
```lua
local fucntion m () return 100 end
local function w () return m() end
```
关于局部函数的语法糖和正常函数是一样的。

## 非全局函数

在lua中，非全局函数其实是函数作为第一元素的一个特殊作法，就是将函数放在table中，也正是因为它，lua有了强大的面向对象编程的能力。

先来看几个例子：
```lua
-- filename: nonlocal_func.lua
Lib = {}
Lib.foo = function (x,y) return x + y end
Lib.goo = function (x,y) return x - y end

Lib2 = {
    foo = function (x,y) return x + y end, -- note!
    goo = function (x,y) return x - y end
}

Lib3 = {}
function Lib3.foo (x,y)
    return x + y
end

function Lib3.goo (x,y)
    return x - y
end

print(Lib.foo,Lib.goo)
print(Lib2.foo,Lib2.goo)
print(Lib3.foo,Lib3.goo)
```

```bash
(base) ➜  lua-learning git:(master) lua ./nonlocal_func.lua
function: 0x1428ef0	function: 0x14290a0
function: 0x1428fa0	function: 0x1428bc0
function: 0x1428c30	function: 0x1428c90

```
这个例子主要说明的是非全局函数的定义方法。

非全局函数中的语法糖和正常的函数是差不多的，这里给出一些例子：
```lua
local function demo () return nil end

-- 等价于
local demo
demo = function () return nil end
```

关于函数的定义需要注意嵌套的情况：

```lua
local demo = function (n)
  if n < 10 then
    sum = sum or 0 + demo(n+1)
  end
  return sum
end
```
这种写法会保错，因为在递归的时候demo还没有定义完全，补救的方法有：
```lua
local demo
demo = function (n)
  if n < 10 then
    sum = sum or 0 + demo(n+1)
  end
  return sum
end
```
或者直接用语法糖的方式, 这和前面的等价：
```lua
local function demo (n)
  if n < 10 then
    sum = sum or 0 + demo(n+1)
  end
  return sum
end
```

不过这种方法在间接递归调用就不好使了，那就需要提前声明如：
```
local f, w
function f () return w() end
function w () return f() end
```

还有一点需要特别注意，就是，如果使用local这样的关键字修饰function的标签，那么在定义的时候就不要在函数的前面填加local！
下面的例子是错误的：
```lua
local f

local g = function ()
  return f()
end

local function f ()
  return 100
end
```
如果这样的话，会新建一个内部的函数f，而之前在函数g中使用的那个f已经变成了未定义的状态！！！


## Lua中的尾调用消除

什么样的函数是尾调用？
```lua
function f (x) g(x) end -- No
function f (x) return g(x) end -- Yes
```
必须符合这样的形式：return func(< args >)

lua实现的尾调用是利用类似goto的原理，完成stack（栈）的零消耗。

还有什么样的是非尾调用？
```lua
return g(x) + 1
return x or g(x)
return (g(x))
-- etc ...
```

## Lua中的协同程序

下面是一个协同程序实现的管道：
```lua
-- filename: coroutine.lua
productor = coroutine.create( function ()
    while true do
        line = io.read()
        send(line)
    end
end)

function consumer (prod)
    while true do
        line = receive(prod)
        io.write(line, '\n')
    end
end

function send (content)
    coroutine.yield(content)
end

function receive (prod)
    status, content = coroutine.resume(prod)
    return content
end

consumer(productor)
```

利用协同程序实现过滤器
```lua
-- filename: filter.lua

function consumer (filter)
    while true do
        local line = receive(filter)
        io.write(line, '\n')
    end
end

function filter (prod)
    return coroutine.create( function ()
        for _i=1, math.huge do
            local _line = receive(prod)
            local line = string.format("%4d: %s", _i, _line )
            send(line)
        end
    end)
end

function receive (prod)
    local status, content = coroutine.resume(prod)
    return content
end

function send (content)
    coroutine.yield(content)
end

productor = coroutine.create( function ()
    while true do
        local line = io.read()
        send(line)
    end
end)

consumer(filter(productor))
```
