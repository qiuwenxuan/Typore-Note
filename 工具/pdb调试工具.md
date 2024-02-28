# [pdb 调试工具](http://space.jaguarmicro.com/pages/viewpage.action?pageId=88486269)

- 由 [Wenxuan Qiu](http://space.jaguarmicro.com/display/~wenxuan.qiu)创建于[十一月 20, 2023](http://space.jaguarmicro.com/pages/viewpreviousversions.action?pageId=88486269)

# [pdb 调试工具](http://space.jaguarmicro.com/pages/viewpage.action?pageId=88486269#pdb-调试工具)

n (next)：往前执行一行

s(step):进去某个函数往前执行一行

c(continue):开始执行，直到遇到下一个断电

l(list):列出当前指向的那一行和附近的行（指向的一行表示该行还没有运行）

a(args):输出当前函数的参数列表

## [一、pdb 2种用法](http://space.jaguarmicro.com/pages/viewpage.action?pageId=88486269#一pdb-2种用法)

pdb：python debugger

### [1、非侵入式方法 （不用额外修改源代码，在命令行下直接运行就能调试）](http://space.jaguarmicro.com/pages/viewpage.action?pageId=88486269#1非侵入式方法-不用额外修改源代码在命令行下直接运行就能调试)

```py
python3 -m pdb filename.py
```

### [2、侵入式方法 （需要在被调试的代码中添加以下代码然后再正常运行代码）](http://space.jaguarmicro.com/pages/viewpage.action?pageId=88486269#2侵入式方法-需要在被调试的代码中添加以下代码然后再正常运行代码)

```py
import pdb;pdb.set_trace()
```

当你在命令行看到下面这个提示符时，说明已经正确打开了pdb

```py
(pdb)
```

## [二、python常用命令](http://space.jaguarmicro.com/pages/viewpage.action?pageId=88486269#二python常用命令)

| 命令          | 解释                                           |
| :------------ | :--------------------------------------------- |
| break 或 b    | 设置断点                                       |
| continue 或 c | 继续执行程序                                   |
| list 或 l     | 查看当前行的代码段                             |
| step 或 s     | 进入函数（进入 for 循环用 next 而不是用 step） |
| return 或 r   | 执行代码直到从当前函数返回                     |
| next 或 n     | 执行下一行                                     |
| up 或 u       | 返回到上个调用点（不是上一行）                 |
| p x           | 打印变量x的值                                  |
| exit 或 q     | 中止调试，退出程序                             |
| help          | 帮助                                           |
| args 或 a     | 输出当前函数的参数列表                         |
|               |                                                |
|               |                                                |

### [1、break/b 设置断点](http://space.jaguarmicro.com/pages/viewpage.action?pageId=88486269#1breakb-设置断点)

```py
b line # 在当前文件的指定行号设置断点
b 26 # 表示在当前文件的26行设置断点

b filename.py:line # 表示在指定文件的26行设置断点
break encoder.py:12 # 在文件encoder.py的12行设置断点

tbreak # 临时添加断点
import torch
import torch.nn as nn
import pdb


class EncoderLayer(nn.Module):
    def __init__(self):
        super().__init__()
        self.conv1 = nn.Conv2d(4, 10, (3, 3))
        self.conv2 = nn.Conv2d(10, 4, (3, 3))
        self.relu = nn.ReLU()

    def forward(self, x):
        x = self.relu(self.conv1(x))
        return self.relu(self.conv2(x))


class Encoder(nn.Module):
    def __init__(self, num_layers):
        super().__init__()
        # encoders 由 num_layers个 EncoderLayer子层组成，每个子层结构相同，但参数不一定相同。
        self.ModelList = nn.ModuleList([EncoderLayer() for _ in range(num_layers)])

    def forward(self, x):
        # ModuleList是一个list，只能通过list的操作方式（如用for循环、下标索引等）进行forward计算。
        for layer in self.ModelList:
            x = layer(x)
        return x


if __name__ == "__main__":
    pdb.set_trace()
    input = torch.rand(5, 4, 30, 30)
    model = Encoder(num_layers=4)
    output = model(input)
```

具体方法：

（1）首先在前面的任意一行设置 pdb.set_trace() ，使得程序停下来。

（2）输入 break 26 就可以了。如图：

注意，不能使用break命令在注释行设置断电，否者报错

```py
> d:\workspacedemo\wenxuan.qiu\autotestproject\auto-test-project\pdb_test.py(33)<module>()
-> input = torch.rand(5, 4, 30, 30)
(Pdb) break 26  # z在26行设置断点
Breakpoint 1 at d:\workspacedemo\wenxuan.qiu\autotestproject\auto-test-project\pdb_test.py:26
(Pdb) break 25
*** Blank or comment  # 不能在注释行设置断点、报错
(Pdb) 
```

这样断点就设置成功了，程序运行到forward()就会停下来。

### [2、s/step、n/next、r/return、c/contiue、u/up执行下一条语句](http://space.jaguarmicro.com/pages/viewpage.action?pageId=88486269#2sstepnnextrreturnccontiueuup执行下一条语句)

包括 s ，n ， r 这3个相似的命令，区别在如何对待函数上

命令1：s/step

```py
s # 执行下一行（能够进入函数体）
```

命令2：n/next

```py
n # 执行下一行（不会进入函数体），一般用于主函数执行下一行
```

命令3：r/return

```py
r # 执行下一行（在函数中时会直接执行到函数返回处），一般用于在函数执行
```

命令4：c/contiue

```py
c # 继续执行语句直到遇到下一个断点
```

命令5：u/up 返回上一个调用点

```py
u # 返回上一个调用点
```

### [3、cl 清除断电](http://space.jaguarmicro.com/pages/viewpage.action?pageId=88486269#3cl-清除断电)

```py
cl # 清除所有断点
cl line # 清除指定行断点
cl filename:line # 清除指定文件下的指定行断点
cl Breakpointno # 根据断点序号清除断点
```

例：

```py
(Pdb) cl # 清除所有断点
Clear all breaks? y
Deleted breakpoint 1 at d:\workspacedemo\wenxuan.qiu\autotestproject\auto-test-project\pdb_test.py:26
(Pdb) break 26 # 清除指定行断点
Breakpoint 2 at d:\workspacedemo\wenxuan.qiu\autotestproject\auto-test-project\pdb_test.py:26
(Pdb) cl 1 # 清除指定文件下的指定行断点,清除Breakpoint 1断点
*** Breakpoint 1 already deleted
(Pdb) cl 2 # 清除Breakpoint 2断点
Deleted breakpoint 2 at d:\workspacedemo\wenxuan.qiu\autotestproject\auto-test-project\pdb_test.py:26
```

### [4、p/print 打印变量值](http://space.jaguarmicro.com/pages/viewpage.action?pageId=88486269#4pprint-打印变量值)

```py
p expression
```

### [5、l/list 打印当前所指位置](http://space.jaguarmicro.com/pages/viewpage.action?pageId=88486269#5llist-打印当前所指位置)

```py
l # 查看当前位置前后11行源代码（多次会翻页）,当前位置在代码中会用-->这个符号标出来，指针所指位置表示还未执行到该步
ll # 查看当前函数或框架的所有源代码
```

### [6、w 打印堆栈信息](http://space.jaguarmicro.com/pages/viewpage.action?pageId=88486269#6w-打印堆栈信息)

```py
w # 打印堆栈信息，最新的帧在最底部。箭头表示当前帧。
```

### [7、q/exit 退出pdb](http://space.jaguarmicro.com/pages/viewpage.action?pageId=88486269#7qexit-退出pdb)

```py
q # 退出pdb
```

### [8、pdb.pm() 程序崩溃后调试](http://space.jaguarmicro.com/pages/viewpage.action?pageId=88486269#8pdbpm-程序崩溃后调试)

前面所述都是在程序开始运行时就插入断点，用pdb进行调试，即**事前调试**。其实 pdb 还可以进行事后调试，即在程序有bug运行奔溃后用python调试器进行查看。

比如 test.py 显然是有 bug 的：

```py
# test.py
def add(n):
    return n+1
add("hello")
```

可以用下面这个命令进行简单调试：

```
python -i test.py # i 选项可以让程序结束后打开一个交互式shell
```

如下：

```py
F:\PycharmProjects\pytorch_practice>python -i test.py
Traceback (most recent call last):
  File "test.py", line 4, in <module>
    add("hello")
  File "test.py", line 2, in add
    return n+1
TypeError: can only concatenate str (not "int") to str
>>>
```

现在我们发现程序结束后出现了 >>> 符号，这就是python调试器。

输入命令：

```py
import pdb
pdb.pm()
```

其中 pdb.pm() 用于程序发生异常导致奔溃后的事后调试，**可以跟踪异常程序最后的堆在信息。**

执行命令后得到：

```py
TypeError: can only concatenate str (not "int") to str
>>> import pdb
>>> pdb.pm()
> f:\pycharmprojects\pytorch_practice\test.py(2)add()
-> return n+1
(Pdb)
```

可以发现，pdb.pm() 已经追踪到了导致程序奔溃的语句：return n+1

此时可以打印 n 的值进行检查：

```py
(Pdb) p n
'hello'
(Pdb) q
>>> quit()

F:\PycharmProjects\pytorch_practice>
```