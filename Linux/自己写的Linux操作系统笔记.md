 笔记格式：

> 主章节格式：#第一章+水平分割线
>
> 第一级分支：##01、Linux基础命令
>
> 第二级分支：###1、Linux目录结构
>
> 第三级分支：####2.1 ls命令
>
> 后面的分支统一用 1.   2.  3.
>
> 然后就是1.1 2.1 3.1
>
> 案例用 Q选项框
>
> 代码用代码框

Linux快捷键：

| 快捷键        | 功能                         |
| ------------- | ---------------------------- |
| ctrl+alt      | 虚拟机显示鼠标               |
| ctrl+c        | 强制停止程序，退出命令输入   |
| tab           | 命令/文件名补全              |
| ctrl+l/clear  | 清空终端内容（清屏）         |
| history       | 查看历史命令                 |
| ctrl+d        | 退出或登出（不能用于vi/vim） |
| history       | 查看历史命令                 |
| ！+命令的前缀 | 自动匹配上一次匹配的前缀命令 |
| ctrl+r        | 输入内容去匹配历史命令       |
| ctrl+a        | 调到命令开头                 |
| ctrl+e        | 跳到命令结尾                 |
| ctrl+键盘左键 | 向左跳一个单词               |
| ctrl+键盘右键 | 向右跳一个单词               |
|               |                              |

# 第二章 Linux基础命令

------



![image-20230409150359614](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409150359614.png)

### 1、Linux目录结构

linux的目录结构是一个树形结构

windows系统可以有很多个盘符：C盘，D盘，E盘

Linux没有盘符这个概念，所有的文件都存放在一个根目录 / 下面，即只有一个顶级目录`/`

![image-20230409150930430](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409150930430.png)

![image-20230409150933666](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409150933666.png)

### 2、Linux路径描述方式



#### 2.1 Linux与windows文件层次结构

- 在Linux系统当中，路径之间的层次关系，使用：/ 来表示
- 在windows系统当中，路径之间的层次关系，使用：\ 来表示

Linux目录下的路径描述方式：

- 开头的/表示的是根目录，后面的/表示层级关系

![image-20230409151359749](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409151359749.png)

windows目录下的路径表示方式：

- D:表示D盘，\表示层级关系

![image-20230409151556895](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409151556895.png)

#### 2.2 相对路径和绝对路径

- 绝对路径：以根目录为起点，描述路径的一种写法，路径描述以/开头
- 相对路径：以当前目录为起点，描述路径的一种写法

> 案例：在根目录下进入directory目录

绝对路径写法：

![image-20230409171511001](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409171511001.png)

相对路径写法：

![image-20230409171517100](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409171517100.png)

特殊路径符：

```
.	 表示当前目录    
.. 	 表示上一级目录
~    表示HOME目录
```

![image-20230409191638008](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409191638008.png)

**`cd directory`与`cd ./directory`**效果相同，都表示当前目录下进入下一级目录

![image-20230409191940862](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409191940862.png)

连退两级目录 **`cd../../`**

![image-20230409192139089](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409192139089.png)

`cd ~`表示`cd home`

![image-20230409192907674](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409192907674.png)

### 3、Linux命令入门

```makefile
command [-options] [parameter]
```

- command:命令本身
- -options:[可选，非必填]命令的一些`选项`，可以通过选项控制命令的行为细节
- parameter:[可选，非必填]命令的`参数`，多数用于命令的指向目标等

语法中的 `[]` 表示可选的意思

![image-20230409160440384](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409160440384.png)

#### 3.1 ls命令 列出当前目录内容

```
ls [-a -l -h][Linux路径]
```

- -a -l -h是可选选项
- Linux路径是此命令可选的参数

当不使用选项和参数时，直接使用ls命令本体表示：以平铺的形式，列出**`当前工作目录`**下的内容

一般默认打开的是用户自带的home目录（每一个用户都有一个home目录，home目录为默认的工作目录）

每一个用户在Linux的专属目录，即默认创建在 /home/用户名

Linux命令行在执行的时候需要一个工作目录，打开行程序（终端）默认设置的工作目录为用户的home目录

![image-20230409161803962](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409161803962.png)

ls命令参数:

```
ls [-a -l -h]
```

-a 表示all的意思，表示可以展示出隐藏的内容，一般在Linux当中隐藏文件都是以 . 开头

![image-20230409163648410](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409163648410.png)

-l 选项，表示：以列表（竖向排列）的形式展示内容，并展示更多信息

![image-20230409163834835](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409163834835.png)

-h选项，表示以易于阅读的形式，列出文件的大小，如K、M、G(-h选项必须搭配-l一起使用，不能单独使用)

![image-20230409164051708](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409164051708.png)

命令的选项可以组合使用，比如：ls -lah,等同于ls -l -a -h

一般常用的为`ls -l`组合

#### 3.2 cd和pwd命令 进入，查看目录

1.cd切换目录（英文：Change Directory）

```
语法： cd[Linux路径]
```

- cd命令无需选项，只有参数，表示要切换到哪个目录下
- cd命令直接执行，不写参数，表示回到用户的HOME目录

![image-20230409164613278](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409164613278.png)

2.pwd(查看当前所在工作目录，英文：Print Work Directory)

```
pwd
```

- `pwd`命令，无选项无参数，直接输出`pwd`即可

![image-20230409170513533](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409170513533.png)

#### 3.3 `mkdir`命令 创建文件夹

`mkdir` （make directory）创建一个新的目录（文件夹）

语法：

```
mkdir [-p] Linux路径
```

Linux路径参数必填，表示Linux路径，即要创建的文件夹的路径，相对路径或绝对路径均可

![image-20230409194240521](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409194240521.png)

-p选项可选，表示自动创建不存在的父目录，适用于`创建连续多层级`的目录

如果想一次性创建多个层级的目录，如下图：直接创建会显示报错，需要加上 `-p` 选项

![image-20230409201922431](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409201922431.png)

![image-20230409201653160](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409201653160.png)

#### 3.4 touch命令 创建文件

```
语法：touch （参数1）Linux路径 （参数2）Linux路径
```

- touch命令无选项，参数必填，表示要创建的文件路径（可以同时创建多个文件夹）

![image-20230409203029143](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409203029143.png)

> 创建多个文件夹：

![image-20230409214932004](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409214932004.png)

小插曲：如何分辨文件夹和文件

`ls -l` 命令，`d`开头表示文件夹，`-`表示文件

![image-20230409203232934](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409203232934.png)

#### 3.5 cat命令 查看文件内容

```
语法： cat Linux路径
```

- cat同样没有选项，只有必填参数，参数表示被查看的文件路径

![image-20230409204931118](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409204931118.png)

#### 3.6 more命令 查看文件内容

```
语法： more Linux路径
```

- 必填路径参数，无选项
- 与cat命令不同的是，more支持文件翻阅，适用于查看内容比较多的文件

![image-20230409205518022](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409205518022.png)

![image-20230409205345131](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409205345131.png)

进入more命令指定的页面后，q退出，空格翻页

#### 3.7 cp命令 复制文件、文件夹

cp命令来自于英文copy

```
语法： cp [-r] 参数1（路径1） 参数2（路径2）
```

- -r选项 可选用于复制文件夹
- 参数1：Linux路径，表示被复制的文件或文件夹
- 参数2：Linux路径，表示要复制到的地方

> 案例1：cp命令的使用

![image-20230409210825666](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409210825666.png)

![image-20230409210950335](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409210950335.png)

将test01/test01.txt文件复制到根目录下test02.txt,并用cat查看test.02文件

> 案例2：cp命令复制文件夹

![image-20230409211300456](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409211300456.png)

![image-20230409211313818](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409211313818.png)

#### 3.8 mv命令 移动文件、文件夹

mv命令来自英文单词：move

```
语法： mv 参数1（路径1） 参数2（路径2）
```

- 参数1：Linux路径，表示被移动的文件或文件夹，即移动并删除该文件
- 参数2：Linux路径，表示要移动到的文件或文件夹，如果目标不存在，则创建该文件或文件夹确保目标存在（不存在即相当于直接改名）

> 案例1：将test01.txt移动到cast文件夹内

![image-20230409212927962](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409212927962.png)

![image-20230409212805321](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409212805321.png)

> 案例2：更改原目录下的test02.txt文件名称

mv命令如果目标不存在，则创建该文件或文件夹确保目标存在并删除原文件或文件夹，此时test02文件移动到系统创建的新文件test03,可以看成重命名。

![image-20230409213248168](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409213248168.png)



#### 3.9 rm命令 删除文件、文件夹

rm-(英文：remove）

```
语法： rm [-r -f] 参数1 参数2 ... 参数N
```

- -r ：由于删除文件、文件夹
- -f :   表示force,强制删除（不会弹出提示确认信息）
  - 普通用户删除内容不会弹出提示，只有root管理员用户删除内容会有提示
  - 所以一般普通用户用不到-f选项

- 参数1、参数2、...参数N表示要删除的文件或文件夹路径，按空格隔开（即可以一次删除多个文件或文件夹）
- 此外，rm命令还支持模糊查询

> 案例1：删除文件

![image-20230409214628323](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409214628323.png)

> 案例2：删除文件夹

![image-20230409214708592](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409214708592.png)

> 强制删除多个文件和文件夹

![image-20230409215108497](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409215108497.png)

rm命令支持通配符`*`，用来做模糊查询

![image-20230409215902944](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409215902944.png)

> 删除所有以test开头的文件或文件夹

![image-20230409220123918](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409220123918.png)

#### 3.10 which&find命令 查找命令和文件名

我们在前面学习Linux命令可知，其实命令本体上是一个个二级制可执行程序，和windows系统当中的exe文件类似，其路径也存在于Linux文件目录内

```
which Linux命令
```

- 由于已知Linux命令是可执行程序，我们可以通过使用which命令查找Linux命令的文件路径

> 效果展示:

![image-20230409221102811](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409221102811.png)

1.find命令 - 按文件名查找文件

```
find 起始路径 -name "被查找文件名"
```

同时，find命令也支持通配符查找

![image-20230409215902944](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409215902944.png)

> 全盘查找test文件或文件夹

![image-20230409222112665](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409222112665.png)

2.find命令 - 按文件大小查找文件

```
find 起始路径 -size +|-n[kMG]
```

- +|-表式大于或小于
- n表示数字大小
- kMG表示大小单位，k(小写)表示kb,M表示MB,G表示GB

![image-20230409223058726](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409223058726.png)

> 案例：查找全盘大于100M的文件并验证大小

![image-20230409223223191](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409223223191.png)

![image-20230409223353188](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230409223353188.png)

#### 3.11 grep命令 通过关键字过滤文件行

```
grep [-n] "关键字" 文件路径
```

- -n ： 表示在结果中显示匹配的行的行号
- 关键字：`必填`表示过滤的关键字，如果带有空格或者其他的特殊符号，建议使用`“ ”`将关键字包围
- 文件路径：`必填`表示要过滤内容的文件路径，`并可作为内容输入端口`

> grep关键字过滤文件test01当中的文本行

![image-20230410091859775](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410091859775.png)

> 带行号过滤[-n]

![image-20230410092028887](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410092028887.png)



#### 3.12 `wc命令` 文件内容的统计

```
wc [-c -m -l -w] 文件路径
```

- -c   统计bytes数量
- -m  统计字符数量
- -l    统计行数
- -w  统计单词数量
- 文件路径（参数）`可作为内容输入端口`

> 由于选项都是可选的，因此可以直接接文件名，显示的是文件的`行数+字符数+bytes大小`

![image-20230410092924715](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410092924715.png)

文件大小确实是52bytes

![image-20230410093225539](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410093225539.png)

> 加选项的wc命令案例：

![image-20230410093122226](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410093122226.png)

#### 3.13 管道符 `|`

管道符符号：`|`

```
（命令行A） | （命令行B）
```

- 将A的结果作为B命令行的输入

![image-20230410094321305](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410094321305.png)

> 用ls -l命令按行展示，然后输入给wc -l 命令统计并输出文件行数

![image-20230410094555060](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410094555060.png)

> 命令行的三层嵌套：

![image-20230410094928279](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410094928279.png)

#### 3.14 echo命令 输出内容

```
echo "输出的内容"
```

- 类似于print语句，在终端上输出内容



> 输出内容：

![image-20230410095522508](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410095522508.png)

如输出Hello Linux中间存在空格，容易被认为是两个文件，且空格等特殊符号不好输出，因此我们输出语句时一般加上`双引号`

```
echo `命令行`
```

- 输出反引号==``==内的命令行结果

> 如果不加``号，系统会默认输出文本结果,加上反引号就会输出反引号内命令行的结果

![image-20230410100024365](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410100024365.png)

#### 3.15 重定向符

重定向符包括`>` `>>`

- `>` 表示将左侧命令的结果，覆盖写入到符号右侧的指定文件当中
- `>>` 表示将左侧命令的结果，追加写入到符号右侧指定的文件中

![image-20230410101206402](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410101206402.png)

> 只要能产生结果的命令都可以用重定向符输入到文件当中

![image-20230410101419659](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410101419659.png)

#### 3.16 tail命令 查看文件尾部内容 跟踪最新更改

```
tail [-f -num] Linux路径
```

- [-f] 表示持续跟踪最新修改（fallow）
- [-num] 数字 表示查看尾部多少行，默认为10行
- Linux路径必填，表示被跟踪的文件路径

> 默认显示尾部10行文件内容

![image-20230410103134389](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410103134389.png)

> 显示5行命令文件内容

![image-20230410103229195](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410103229195.png)

> -f 持续跟踪文件修改内容

首先利用-f选项查看test01后五条内容

![image-20230410103333334](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410103333334.png)

另外创建一个终端2![image-20230410103400387](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410103400387.png)

在终端02的test01文件重定向输入一条内容

![image-20230410103543491](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410103543491.png)

就可以看到终端01内显示跟踪的输出多了一条最新输入的文本

![image-20230410103646740](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410103646740.png)

tail -f命令会一直持续跟踪并输出最新修改的文件内容，我们可以通过快捷键ctrl+c停止终端1跟踪

#### 3.17 vi编辑器

##### 1.vi编辑器介绍

![image-20230410104551903](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410104551903.png)

##### 2.vi/vim编辑器三种工作模式

![image-20230410104617280](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410104617280.png)

##### 3.vi/vim 命令模式

```
vi/vim 文件路径
```

- 如果文件路径存在，则进入并编辑此文件
- 如果文件路径不存在，则创建新文件，以确保能进入并编辑该文件

进入三种模式的命令：

1. 首先vim进入文件默认是在命令模式下
2. 按`i`键进入输入模式，该模式可以用于编辑文件
3. 按`esc`进入命令模式，该模式可以使用很多命令行和快捷键
4. 输入完成后，按`：`会进入底线命令模式且在低端出现输入端口：`+w`(保存)`q`（退出）

![image-20230410111119069](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410111119069.png)

> 案例：在Hello.txt文件内输入并保存内容

输入命令行：![image-20230410111548312](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410111548312.png)

进入vim编辑器，并按i进入输入模式![image-20230410111639772](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410111639772.png)

输入字符![image-20230410111643528](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410111643528.png)

esc键进入命令模式，然后输入：`wq`进入底线模式并保存退出![image-20230410111746038](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410111746038.png)

文件输入成功：![image-20230410111809655](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410111809655.png)

##### 4.命令模式快捷键

![image-20230410112206615](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410112206615.png)

三种基本进入输入模式的快捷键：

- `i` 在当前光标进入输入模式
- `a` 在当前光标之后进入输入模式
- `o` 在当前光标的下一行进入输入模式

##### 5.底线模式快捷键

![image-20230410113328086](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410113328086.png)

这里重点解释一下`set pasete`命令

当你使用`：set pasete`命令，此时你再进入输入模式时，底层会显示![image-20230410113625044](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410113625044.png)

表示粘贴内容到vim时将保持文本格式进行粘贴。

# 第三章 Linux权限管理

------



### 1、Linux的root用户（超级管理员）

![image-20230410153407970](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410153407970.png)

#### 1.1 root用户与普通用户

root用户具有最大的系统操作权限，而普通用户在许多地方权限是受限的。

普通用户只能在当前home目录下进行操作，在其他文件下看会显示权限不够

> 普通用户在根目录下创建文件夹，会显示权限不够

![image-20230410154220655](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410154220655.png)

> 而root用户可以在根目录下面创建文件夹

![image-20230410154857612](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410154857612.png)

#### 1.2 `su`和`exit`命令 切换用户

```
su [-] [用户名]
```

- [-]符号可选，表示是否在切换用户后加载环境变量（建议带上）
- 参数：用户名，表示要切换的用户，用户名可以省略，默认表示切换到root用户
- 使用普通用户转其他用户需要输入密码
- 使用root用户切换到其他用户无需密码，可以直接切换
- 直接输入 `su` 默认进入root用户

> root用户与student用户的切换

root可以直接转student用户

![image-20230410155401980](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410155401980.png)

sudent转root需要密码 `su` 默认转root

![image-20230410155810246](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410155810246.png)

切换用户后，可以通过`exit`命令和`ctrl+d`快捷键回退到上一个用户	

![image-20230410161537263](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410161537263.png)



#### 1.3 `sudo`命令 临时获取最大权限

![image-20230410161952646](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410161952646.png)

因为如果我们长期使用root用户难免会产生不可避免的操作错误，因此更多时候我们都是在普通用户下使用`sudo`命令获取root权限进行操作

```
sudo 其他命令
```

- 在其他命令之前，带上`sudo`命令即可以为这条语句临时赋予root权限
- 在用户使用`sudo`命令之前，需要在root内为普通用户配置`sudo认证`

> 给student用户配置`sudo`认证

```
	vi /ect/sudoers
	或
	visudo
```

配置认证需要在root用户权限下

vi修改根目录的 `/etc/sudoers` 文件

![image-20230410191714267](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410191714267.png)

或可以直接写成`visudo`命令修改权限文件

![image-20230410191824166](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410191824166.png)

即进入权限文件后，在文件最后用输入模式输入用户名和权限

```
[用户名] ALL=(ALL)			NOPASSWD:ALL
```

![image-20230410191906637](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410191906637.png)

配置完成之后即可以在普通用户下使用`sudo`命令获取root权限

> 配置认证之后，使用`sudo`命令在student用户权限下在根目录创建文件夹：

![image-20230410192442281](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410192442281.png)



### 2、用户和用户组

#### 2.1 用户和用户组的关系

![image-20230410193137413](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410193137413.png)

#### 2.2 Linux用户与用户组权限

![image-20230410193242392](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410193242392.png)

#### 2.3 用户组管理

以下命令需要root用户执行

```
创建用户组：
	groupadd 用户组名
删除用户组：
	groupdel 用户组名
```

![image-20230410193652778](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410193652778.png)

> 创建用户组和删除用户组

![image-20230410195231911](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410195231911.png)

#### 2.4 用户管理

以下命令需root用户执行

##### 	1.创建用户

```
创建用户
useradd  用户名 [-g] [用户组] [-d][用户路径]
```

- [-g]选项， `指定用户组`，如果不指定-g 用户组，系统会创建一个同名的用户组，如果系统当中已经存在同名的用户组，则必须使用-g
- [-d]选项，`指定用户HOME路径`，不指定则默认在`/home/`用户名路径下

> 直接创建用户test，系统会默认创建一个test同名组，并且用户的路径会默认存放在根目录下的home/文件夹当中

![image-20230410195410597](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410195410597.png)

> 给用户test02指定组和路径

![image-20230410195902656](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410195902656.png)

##### 	2.删除用户

```
删除用户
userdel [-r] 用户名
```

- 选项[-r] 删除用户的HOME目录，不使用-r删除用户时，HOME目录将被保留

> 直接删除用户，其home路径下的目录依然存在

![image-20230410201205645](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410201205645.png)

加了-r ，home路径一起被删除

![image-20230410201358107](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410201358107.png)

##### 	3.查看用户所属组

```
id [用户名]
```

- 用户名（参数）：被查看的用户，如果不提供则查看自身，普通用户只能查看自身
- 在root用户下可以指定查看任何用户名的信息

> 查看自己 `id` 的组的信息（任何用户都可以使用该命令）

![image-20230410201516418](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410201516418.png)



##### 4.修改用户所属组

```
usermod -aG 用户组 用户名
```

- 将指定用户加入到指定的用户组中

> 将默认的test4用户加入到itcast组当中

![image-20230410202201876](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410202201876.png)



##### 5.`getent passwd` 查看系统所有用户

```
getent passwd
```

查看系统所有用户：

![image-20230410202446848](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410202446848.png)

组信息：

![image-20230410202601864](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410202601864.png)

![image-20230410202546300](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410202546300.png)

##### 6.`gentent group`查看系统所有组

```
gentent group
```

> 查看系统所有组：

![image-20230410203002725](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410203002725.png)

### 3、权限控制信息

#### 3.1 查看权限细节

![image-20230410203833848](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410203833848.png)

#### 3.2 认知权限信息

![image-20230410204505214](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410204505214.png)

- 第一位表示文件类型（-表示文件，d表示文件夹，l表示软连接）
- 2~4位表示所属用户的权限（右侧2列表）
- 5~7表示所属用户组权限（右侧3列表）
- 8~10表示所属其他用户权限，包括root用户访问权限

![image-20230410204831069](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410204831069.png)

#### 3.3 `rwx`权限认知

![image-20230410204941582](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410204941582.png)

文件和文件夹对`rwx`权限的差别

![image-20230410204954498](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410204954498.png)

#### 3.4 `chmod`命令 修改权限

##### 1.普通方式修改权限

`chmod`命令可以修改文件、文件夹的权限的权限信息，注意：==只有文件、文件夹的所属用户或root用户可修改==

```
chmod [-R] 权限(u=...,g=...,o=...) 文件或文件夹
```

- 选项[-R] 表示对文件夹内的全部内容应用同样的修改后的权限操作

![image-20230410210434596](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410210434596.png)

> 将test文件夹修改其权限使得内部的所有文件也被同样修改

![image-20230410211001302](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410211001302.png)

由于用这种方法修改权限过于冗长，我们一般使用另一种快捷的方式修改其权限

![image-20230410211341614](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410211341614.png)

##### 2.快捷方式修改权限

```
chmod [xxx] 文件/目录名
```

xxx代表三个权限数字

> 使用`chomd` 751 test给文件配置权限

![image-20230410211714027](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410211714027.png)

其中751代表什么含义呢？

![image-20230410211411492](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410211411492.png)

由于每三位看成一组权限，我们可以将其看成二进制表示，r为最高位表示4,w为第二位表示2，x为第三位表示1

#### 3.5 `chown`命令 修改权限

使用`chown`命令，可以修改文件，文件夹的所属用户和用户组

普通用户无法修改所属为其他用户或组，==此命令只能适用于root用户执行，所属用户也没有权限修改其用户和用户组==

假设所属用户也能修改文件自身的用户或用户组，那么该文件的用户和用户组就会修改成非该用户，所属用户的权限又变成修改后的用户的权限了，互相矛盾。

```
chown [-R] [用户][:][用户组] 文件或文件夹
```

![image-20230410212642895](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410212642895.png)

![image-20230410213322583](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410213322583.png)

> 示例：root用户权限下修改test文件所属用户和用户组为student

![image-20230410213207546](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410213207546.png)

# 第四章 Linux实用操作

------



### 1、各类快捷键

#### 1.1 ctrl+c强制停止

![image-20230410213852809](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410213852809.png)

#### 1.2 ctrl+d 退出或登出

![image-20230410214052169](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410214052169.png)

#### 1.3  历史命令搜索

##### 1.history命令

![image-20230410214306609](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410214306609.png)

##### 2.命令前缀`!`

自动执行最近一次匹配前缀的命令

![image-20230410214405057](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410214405057.png)

即我们历史执行了python命令，然后`！+py`...系统会自动从历史记录中从下到上执行第一个以p开头的命令，类似于模糊查询

#####  3.ctrl+r 匹配最近历史命令

![image-20230410215425764](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410215425764.png)

![image-20230410215518208](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410215518208.png)

#### 1.4 光标移动快捷键

![image-20230410215629652](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410215629652.png)

#### 1.5 清屏

![image-20230410215734936](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230410215734936.png)

### 2、yum命令 软件安装 

`yum`：RPM（Linux可执行文件后缀名）包软件管理器，用于自动化安装配置Linux软件，并可以主动解决依赖问题。

```
yum [-y] [install | remove | search] 软件名称
```

- [-y]选项 表示无需确认直接执行

```
安装软件：yum [-y] install [软件名]

卸载软件：yum [-y] remove [软件名]

搜索安装包：yum search [软件名]
```

> 用 yum命令下载完并在Linux中搜索该软件

![image-20230412092035234](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412092035234.png)

![image-20230412092017359](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412092017359.png)

![image-20230411135241304](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230411135241304.png)

### 3、`systemctl`命令 控制软件启动关闭

`systemctl`命令可以控制很多内置系统或注册为系统应用的第三方软件

```
systemctl start | stop | status | enable | disable 服务名
```

![image-20230411143356274](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230411143356274.png)

系统内置服务：

![image-20230411143406959](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230411143406959.png)

> 案例：查看系统内置软件防火墙的状态

![image-20230411143626897](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230411143626897.png)

> 关闭并查看防火墙状态

![image-20230412093408633](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412093408633.png)

> 关闭防火墙开机自启动

![image-20230412093510973](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412093510973.png)

> `systemctl命令`控制第三方软件

![image-20230412094157769](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412094157769.png)

对于有些第三方软件来说，系统下载它之后会自动注册为内置系统软件，因此这类第三方软件也可以用systemctl命令控制。

> 如控制第三方软件ntp

先下载第三方软件ntp,然后其内置系统应用名称为ntpd,`systemctl`命令行控制其启动并查看其状态

![image-20230412093939327](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412093939327.png)

### 4、软连接及其创建 	

软连接相当于windows的快捷方式，通过在系统当中创建软连接，将文件、文件夹连接到其他位置

![image-20230412102742654](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412102742654.png)

```
ln -s 参数1 ~参数2
```

- -s 选项，创建软连接
- 参数1，被链接的文件或文件夹
- 参数2，要连接去的目的地（没有则系统自动创建同名文件）

> 实例创建软连接指向文件：在根目录下创建软连接指向/etc/yum.conf文件

![image-20230412103022544](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412103022544.png)

![image-20230412103115894](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412103115894.png)

> 创建软连接指向文件夹

![image-20230412103334551](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412103334551.png)

> 分别查看软连接文件和源文件

查看根目录下的软连接：

![image-20230412103748452](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412103748452.png)

查看原目录下的软连接：

![image-20230412103851009](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412103851009.png)

结果发现两者内容完全一样，软连接指向源文件位置信息

### 5、日期和时间

#### 5.1 date命令

```
date [-d] [+格式化字符串]
```

- [-d]可选选项，一般用来做日期计算
- [-格式化字符串] 如下选项；用来显示不同格式的字符串

![image-20230412104339460](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412104339460.png)

> 几种日期的格式显示：
>
> 如果日期+时间，由于时间和日期之间有一个空格，系统会认为有一个额外的参数报错，因此我们需要把日期和时间用双引号表示一个整体

![image-20230412104343638](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412104343638.png)

> [-d] 选项的使用：[-d]也可以和格式化字符串一并使用

![image-20230412105221582](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412105221582.png)

#### 5.2 修改Linux时区

##### 1.手动修改时区

![image-20230412110235749](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412110235749.png)

```
rm -f /ect/localtime
sudo ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

> 修改Linux系统的时区

首先我们的时区为美国时区：

![image-20230412110512757](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412110512757.png)

我们使用root权限，先删除本地时区信息

![image-20230412110813752](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412110813752.png)

然后通过ln命令将/usr/share/zoneinfo/Asia/Shanghai文件日期信息软连接到我们的/etc/localtime文件夹

![image-20230412110931821](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412110931821.png)

这个时候时区已经变成东八区修改完成了

##### 2.自动修改时区

下载`ntp`程序自动校准系统时间

![image-20230412212809900](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412212809900.png)

> 下载`ntp`软件并用`systemctl`命令自启动

![image-20230412213043868](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412213043868.png)

其中由于该软件校准时间间隔比较大，我们也可以使用ntp提供的手动校准命令`ntpdate -u ntp.aliyun.com` 访问阿里云自动校准时间服务器完成校准；

![image-20230412213516638](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412213516638.png)

### 6、IP地址和主机名

#### 6.1 IP地址

![image-20230412214044763](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412214044763.png)

> 查看本主机的`ip`地址

一般而言centos网卡一般称为`ens33`

![image-20230412214249941](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412214249941.png)

![image-20230412214037661](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412214037661.png)

#### 6.2 主机名

![image-20230412214032876](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412214032876.png)

> 查看主机名：

![image-20230412214439967](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412214439967.png)

#### 6.3 修改主机名

![image-20230412214448851](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412214448851.png)

#### 6.4 域名解析

![image-20230412215027331](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412215027331.png)

域名解析的基本流程：

针对于windows系统和Linux系统其访问流程基本相似

首先Linux和windows会优先查看本机的记录（私人地址本）

![image-20230412215525032](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412215525032.png)

如果没找到对应的ip地址映射，我们可以联网访问DNS(域名服务器)查看是否有记录，如果存在映射则访问对应的ip地址网站，否则输出404

![image-20230412215324101](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412215324101.png)

> 给Linux主机名和ip地址做映射，使得Linux通过主机名就能够得到主机ip地址

首先用记事本打开文件hosts：

![image-20230412220421573](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412220421573.png)

然后修改hosts文件内容，做好IP地址与主机名的映射

![image-20230412220456035](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412220456035.png)

验证Linux映射是否成功

打开finalshell连接管理器，修改主机从ip地址10.18.105.171改成主机名CentOS

![image-20230412220818812](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230412220818812.png)

然后发现居然也能链接Linux，说明计算机已经完成了主机名和ip域名的映射； 		

### 7、配置Linux固定ip地址

首先，为什么要固定ip地址

![image-20230413083606700](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413083606700.png)

固定Linux ip地址

打开虚拟机的虚拟网络编辑器

更改VMnet8下的子网为192.168.88.0 子网掩码改为255.255.255.0

![image-20230413084119613](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413084119613.png)

然后在net模式下打开nat设置

![image-20230413084858293](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413084858293.png)

然后更改nat网关为192.168.88.2，保存即可

![image-20230413085106773](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413085106773.png)

然后我们vim编辑目录下的配置文件

![image-20230413090542897](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413090542897.png)

将获取IP地址设为静态获取，并在末尾添加网关等

![image-20230413090548226](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413090548226.png)

保存并退出，重新启动network并查看其ens33的IP地址

### 8、 网络请求和下载

##### 1.使用ping命令测试服务器是否联通

ping命令用来检查指定的网络服务器是否是可联通的状态

```
ping [-c num] ip或主机名
```

- [-c]选项 ，检查的次数，不使用-c选项将无限次数的持续检查
- ip或主机名，被检查的服务器的ip地址或主机名地址

![image-20230413194748854](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413194748854.png)

##### 2.使用wget命令进行网络文件下载

`wget`是非交互式的文件下载器，可以在命令行内下载网络文件

```
wget [-b] url
```

- [-b]选项，后台下载，并将日志写入到当前工作目录的wget-log文件，如需查看后台记录可以使用tail命令跟踪日志文件
- 参数：url表示下载链接

![image-20230413194815764](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413194815764.png)

无论下载是否完成都会在后台生成下载的文件，如下载未完成要定时清理未下载完成的文件。

##### 3.使用curl命令发起网络请求

curl命令

```
curl [-O](大写) url
```

- 选项-O :用于文件下载，当url是下载链接时，使用此选项下载并保存文件
- 参数[url] 要发起请求的网络地址

![image-20230413201511834](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413201511834.png)

curl向网站发起网络请求，类似于windows访问web服务器得到网站源码，通过浏览器渲染得到完整的网页，但是Linux命令行没有渲染网页的功能，得到的是网站前段界面的源码

![image-20230413202242610](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413202242610.png)

![image-20230413202207554](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413202207554.png)

curl如果后面接的是安装包连接，需要加标签-O，其作用类似于wget命令下载文件

<img src="C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413201719057.png" alt="image-20230413201719057" style="zoom:80%;" />

### 9、端口

端口是设备与外界通讯交换的出入口，端口分为：物理端口和虚拟端口两大类

- 物理端口：又可以称为接口，是可见的端口，如USB接口等
- 虚拟端口：是指计算机内部的端口，是不可见的，用来操作系统和外部进行交互使用的

![image-20230413202502705](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413202502705.png)

#### 9.1 虚拟端口的概念

​		由于计算机之间的通信，只能通过ip锁定计算机，但是无法锁定具体的程序

​		通过端口这一个概念可以锁定计算机上具体的程序，确保程序之间进行沟通

​		类似于两个IP地址只能表示两台计算机之间互相通信，但是不能标识哪一个具体的应用程序，而只要确定两个互相通信的端口便能知道是计算机上具体哪一个应用在通信

![image-20230413203046565](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413203046565.png)

#### 9.2 Linux的三类端口

![image-20230413203917734](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413203917734.png)

#### 9.3 `nmap`命令查看指定ip的端口占用

语法：

```
nmap 查看的ip地址
```

![image-20230413204012401](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413204012401.png)

> 首先安装nmap程序，并查询本机（127.0.0.1表示本机）的端口占用情况

![image-20230413204004276](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413204004276.png)

![image-20230413204246202](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413204246202.png)

#### 9.4 netstat命令查看指定端口的占用

```
netstat -anp | grep 端口号     //因为netstat -anp会列举出所有端口的占用情况，因此我们可以用管道符过滤查看指定的端口占用情况，如果该端口被占用则输出信息。如果不输出表示该端口不存在或者没有被占用
```

![image-20230413204856695](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413204856695.png)

> 下载netstat工具

![image-20230413205356544](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413205356544.png)

> 查看指定端口580的占用情况

![image-20230413205439602](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413205439602.png)

> 没有输出案例表示端口不存在或者未被占用

![image-20230413205602256](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413205602256.png)

### 10、进程管理

#### 10.1 进程的概念

操作系统为了更好的管理调度应用程序，给每一个正在运行的程序分配了一个进程，且每一个进程都有一个独有的进程号

![image-20230413211150182](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413211150182.png)

#### 10.2 `ps`命令 查看进程

语法：

```
ps [-e -f]
```

- 选项：-e 表示显示全部的进程
- 选项： -f 以完全格式化的形式展示信息（展示全部信息）
- 一般来说，固定用法都是 `ps -ef`

![image-20230413211403827](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413211403827.png)

进程信息的含义

![image-20230413211644590](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413211644590.png)

> 查看指定进程tail案例
>

![image-20230413211706995](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413211706995.png)

比如我们先使用tail命令，终端就会一直堵塞

![image-20230413212730175](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413212730175.png)

此时我们另外创建一个终端，然后使用ps命令和管道符过滤tail关键字的进程，如下：

![image-20230413212716089](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413212716089.png)

管道符 | grep 可以过滤任何字符，如可以过滤带有30001进程号的的进程信息

#### 10.3 kill命令 杀死进程

语法：

```
kill [-9] 进程id
```

![image-20230413213230179](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413213230179.png)

> 由于上面tail命令并未终止，我们使用kill命令和kill -9命令查看进程是否被关闭

首先使用`ps -ef |grep tail`命令查看tail进程id

![image-20230413213913271](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413213913271.png)

kill + 进程号 给tail进程发指令完整自我终结

![image-20230413214015926](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413214015926.png)

kill -9 +进程号 是系统使用强制手段终结了tail进程，两者不一样

![image-20230413214058247](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413214058247.png)



### 11、主机状态监控

#### 11.1 top命令 查看主机资源占用

```
top
```

##### 1.top命令选项

![image-20230413215758065](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413215758065.png)

展示界面：

![image-20230413215603428](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413215603428.png)

##### 2.top命令内容详解

![image-20230413215657535](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413215657535.png)

![image-20230413215738069](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413215738069.png)

##### 3. top交互式选项

当top运行时，我们可以通过以下按键交互式显示top所展示界面

![image-20230413215940792](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413215940792.png)

#### 11.2 磁盘信息监控

##### 1.df命令查看硬盘使用情况

```
df [-h] //-h选项显示单位
```

![image-20230413220806688](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413220806688.png)

##### 2.iostat命令查看cpu和磁盘的相关信息

语法：

```
iostat [-x] [num1] [num2]
*选项[-x] 显示更多信息
*num1 数字 代表刷新间隔，num2 数字 代表刷新次数
```

![image-20230413221521350](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413221521350.png)

![image-20230413221907016](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413221907016.png)

> 案例：使用iostat命令带两个数字参数1 3，表示一秒刷新一次，总共刷新三次

![image-20230413221942338](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413221942338.png)

##### 3.sar命令 查看网络状态监控

```
固定语法：
	sar -n DEV num1 num2     //由于sar命令比较复杂，因此我们使用固定的简单语法即可
选项：[-n] 查看网络，DVE表示查看网络接口
num1 刷新间隔（默认为1）num2 查看次数（默认无限次数）
```

![image-20230413222452200](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413222452200.png)

### 12、环境变量

什么是环境变量？

![image-20230413223854682](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413223854682.png)

#### 12.1`env`命令 查看系统的环境变量

```
env（environment环境变量）
```

![image-20230413225048231](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413225048231.png)

> 输出环境变量：

![image-20230414104332690](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414104332690.png)

#### 12.2 环境变量PATH

环境变量path记录了系统执行任何命令的搜索路径，即当执行任何命令的时候，都会优先搜索path上的各个路径，直到搜索到要执行的程序的本体

![image-20230413225111962](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230413225111962.png)

#### 12.3 `$`符取环境变量

环境变量是以键值对的形式进行存储的 即path=/usr/...，`key=path` `value=/usr/...`

我们通过$+键就可以取到键对应的值 ，然后用echo打印输出到终端

```
语法：
	echo $PATH
	echo ${PATH}+拼接的字符
```

![image-20230414104545598](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414104545598.png)

> 用$符打印环境变量

![image-20230414104737236](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414104737236.png)

![image-20230414104817885](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414104817885.png)

> 在输出的值后面拼接字符ABC

![image-20230414104948319](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414104948319.png)

#### 12.4 自行设置环境变量

![image-20230414105619716](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414105619716.png)

##### 1.临时设置环境变量

语法：

```
export 变量名=变量值
```

设置一个临时环境变量ITHEIMA(环境变量名一般大写)，重启操作系统环境变量就会消失

![image-20230414105740243](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414105740243.png)

##### 2.永久设置环境变量

1. 针对当前用户生效，配置在当前用户的：~/bashrc文件当中
2. 针对所有用户生效，配置在当前系统的：/etc/profile文件中

配置完文件后需要用 source + 配置文件，使文件立即生效，或者重新登录finalshell即可生效

###### 	1.当前用户设置永久环境变量

```
vi ~/.bashrc
source .bashrc
```

> 在当前用户下设置永久环境变量MYNAME=itheima

首先进入vi ~/.bashrc文件，在文件尾部添加一条设置环境变量的命令

![image-20230414111031265](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414111031265.png)

然后退出编辑，用source命令+文件名 使文件立即生效

![image-20230414111152030](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414111152030.png)

此时用$符查看对应添加的环境变量

![image-20230414111323637](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414111323637.png)

​	2.所有用户设置永久环境变量

```
vi /etc/profile
source /etc/profile
```

> 在所有用户下设置环境变量MYNAME=itcast

vi /etc/profile 进入并修改文件

![image-20230414111917653](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414111917653.png)

查看环境变量是否设置成功

![image-20230414111944944](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414111944944.png)



> 创建新用户并查看其环境变量是否可用

![image-20230414112133612](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414112133612.png)

#### 综合案例

> 自己创建一个程序添加到系统环境变量当中，使得任意工作目录下都能执行

1.在root下创建文件夹myenv

![image-20230414114301892](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414114301892.png)

在文件夹内编辑一个简单的可执行程序mkhaha

![image-20230414114330021](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414114330021.png)

![image-20230414114333672](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414114333672.png)

查看mkhaha文件权限,发现并无可执行权限，需要给该文件夹中心权限x

![image-20230414114505391](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414114505391.png)

给程序执行权限

![image-20230414114646243](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414114646243.png)

在myenv目录执行该文件

![image-20230414114725231](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414114725231.png)

为了让mkhaha可执行文件在任意工作目录下都能运行，我们需要将其添加到PATH的环境变量下

进入vi /etc/profile把mkhaha添加到全局环境变量内

```
添加到PATH环境变量格式：
	export PATH=$PATH:+路径		//注意：export PATH=$PATH:+路径表示将原有的$PATH的值都加入到该路径前面，否则不加$path就会直接替换原有的path路径
```

![image-20230414115042447](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414115042447.png)

![image-20230414114903867](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414114903867.png)

查看现有的PATH值，可以看到mkhaha路径也已经添加到PATH环境变量之后

![image-20230414115533115](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414115533115.png)

该可执行文件已经可以在任意工作目录下执行（我们只需要在环境变量中添加文件的工作目录/root/myenv，系统会直接访问内部与mkhaha同名文件，而不是在环境变量内添加可执行文件路径/root/myenv/mkhaha）

![image-20230414115707828](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414115707828.png)

###  13、Linux和主机文件的上传和下载

#### 13.1 通过finalshell软件底部的应用栏进行拖拽的形式上传和下载

从Linux下载到的文件会存储在桌面的fsdownlown的文件夹当中

![image-20230414154745412](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414154745412.png)

![image-20230414154747774](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414154747774.png)

#### 13.2 通过`rz、sz`命令的方式首先上传和下载

首先下载软件lrzsz

![image-20230414154848904](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414154848904.png)

##### 1.`rz`进行文件上传到Linux

![image-20230414155038478](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414155038478.png)

使用rz命令自动调用windows主机的文件目录，选择好下载到Linux的文件，点击右上角的按钮可撒查看下载情况

![image-20230414155911720](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414155911720.png)

##### 2.`sz`进行文件下载到windows本地

> 查看并将Hello.txt文件下载到windows本地的fsdownload文件夹

![image-20230414160108259](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414160108259.png)

命令行输出自动执行下载任务

![image-20230414160153835](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414160153835.png)

### 14、压缩和解压

#### 14.1 常见压缩格式

Linux市面上常用zip、tar、gzip三种压缩格式

![image-20230414160542929](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414160542929.png)

#### 14.2 tar命令 压缩、解压文件

![image-20230414160959902](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414160959902.png)

语法：

```
tar [-c -v -x -f -z -C] 参数1 参数2 ... 参数N   //一般-f标签放在标签的最后一位，后面直接接创建或解压的文件名
```

![image-20230414161003212](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414161003212.png)

![image-20230414194410646](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414194410646.png)

##### 1.tar命令用于压缩文件

![image-20230414165615632](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414165615632.png)

```
使用.tar格式简单封装文件，文件内存不会压缩，仅仅起到一个压缩的功能
	tar -cvf test.tar 1.txt 2.txt 3.txt //将1.txt 2.txt 3.txt文件压缩到test.tar文件当中
使用.gz格式压缩文件，文件内容会极大地压缩，便于压缩后进行传输
	tar -zcvf test.tar.gz 1.txt 2.txt 3.txt//需要在命令选项上加入-z的选项，且创建的压缩文件必须使用.gz后缀名结尾
```

使用`.tar`格式简单封装文件，文件内存不会压缩，仅仅起到一个压缩的功能

![image-20230414165911930](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414165911930.png)

使用`.gz`格式将文件1.txt 2.txt 3.txt压缩成一个压缩文件test1.tar.gz

![image-20230414170910176](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414170910176.png)

两者压缩文件大小对比，可见采用.gz格式能够很好地压缩文件大小

![image-20230414170937650](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414170937650.png)

##### 2.tar命令由于解压文件

语法：

```
常用的解压组合：

	tar -xvf test.tar -C +指定文件目录 //解压文件test.tar到指定目录,因为C标签后面需要接解压的文件路径，因此我们一般把-C标签单独的提出来在后面加上文件路径

	tar -xvf test.tar -C /home/itheima

	tar -zxvf test.tar.gz -C /home/itheima //以Gzip模式解压test.tar.gz,只需要在标签内加一个-z标签即可，一般加在标签第一个
```

![image-20230414171359289](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414171359289.png)

```
解压：
	tar -xvf 文件目录 [-C 文件路径]  //将被解压的文件目录压缩到-C标签指定的文件目录，不指定默认解压至当前文件夹
```



> 案例：将test1.tar.gz的压缩包解压到/test内

![image-20230414192052490](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414192052490.png)

查看/test文件夹

![image-20230414192158208](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414192158208.png)

#### 14.3 zip命令 压缩、解压文件

##### 1.zip命令 压缩文件

语法：

```
zip [-r] 参数1（压缩后的文件名）参数2 参数2（被压缩的多个文件...）
```

- 当被压缩的文件包含文件夹时，需要使用-r选项 和rm cp命令的-r效果一致

![image-20230414192449422](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414192449422.png)

> 将test文件夹和几个文件都压缩在test.zip压缩包内

![image-20230414193211124](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414193211124.png)

##### 2.zip命令解压文件

语法：

```
unzip 参数1（被解压文件）[-d] （参数2 添加-d选项后指定压缩到的文件路径）
	*[-d]指定要解压去的位置，同tar的-C选项
	*参数1，被解压的zip压缩包文件
```

![image-20230414193556187](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414193556187.png)

> 解压文件test.zip到-d指定路径/test 如果被解压的文件路径有同名的内容，会覆盖原来文件的内容

![image-20230414193614883](C:\Users\17377\AppData\Roaming\Typora\typora-user-images\image-20230414193614883.png)
