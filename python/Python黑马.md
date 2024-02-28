# 一、你好Python

------

## 1、第一个Python程序

打开CMD（命令提示符）程序，输入Python并回车

然后，在里面输入代码回车即可立即执行

![image-20230523150431410](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230523150431410.png)

## 2、python解释器

![image-20230523150554852](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230523150554852.png)

Python解释器，是一个计算机程序，用来翻译Python代码，并提交给计算机执行。

所以，它的功能很简单，就2点：

1. 翻译代码
2. 提交给计算机运行

解释器存放在：<Python安装目录>/python.exe

![image-20230523150720745](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230523150720745.png)

我们在CMD（命令提示符）程序内，执行的python，就是上图的python.exe程序哦

![image-20230523150738990](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230523150738990.png)

使用Python解释器程序，就能执行Python代码了

![image-20230523150814983](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230523150814983.png)

由于这种命令行的格式只能一次执行一条python代码，我们可以将多条代码，写入一个以”.py”结尾的文件中，使用python命令去运行它。

![image-20230523171149267](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230523171149267.png)

如，在Windows系统的D盘，我们新建一个名为：test.py的文件，并通过记事本程序打开它，输入如下内容：在“命令提示符”程序内，使用python命令，运行它，如图：

![image-20230523171201871](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230523171201871.png)

# 二、python基础语法

------

## 1、数据类型

![image-20230523171834290](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230523171834290.png)

- 数字（Number）
  - 整数（int），如：10、-10
  - 浮点数（float），如：13.14、-13.14
  - 复数（complex），如：4+3j，以j结尾表示复数
  - 布尔（bool）即真和假，True表示真，False表示假。
- 字符串（String）`在python中用str表示字符串`
- 列表（List），有序可变序列
- 元组（Tuple），有序不可变序列
- 集合（Set），无序不重复序列
- 字典（Dictionary），无序Key-Value集合

type()方法查看变量、字面量的数据结构

![image-20230523172433941](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230523172433941.png)



## 2、注释

注释：在程序代码中对程序代码进行解释说明的文字。

单行注释：以 #开头，#右边 的所有文字当作说明，而不是真正要执行的程序，起辅助说明作用

![image-20230523172131226](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230523172131226.png)

多行注释： 以 一对三个双引号 引起来 ( """注释内容""" )来解释说明一段代码的作用使用方法

## 3、数据类型转换

### 1.常见的数据类型转换

![image-20230523172858306](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230523172858306.png)

### 2.注意事项

1. 任何类型，都可以通过str()，转换成字符串
2. 字符串内必须真的是数字，才可以将字符串转换为数字

## 4、标识符

在Python程序中，我们可以给很多东西起名字，比如：

- 变量的名字
- 方法的名字
- 类的名字等

这些名字，我们把它统一的称之为标识符，用来做内容的标识。

所以，标识符：是用户在编程的时候所使用的一系列名字，用于给变量、类、方法等命名。

### 1.标识符名门规则

Python中，标识符命名的规则主要有3类：

- 内容限定（标识符命名中只允许出现：）
  - 英文
  - 中文
  - 数字
  - 下划线（_）
- 大小写敏感
  - ![image-20230523173352278](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230523173352278.png)
- 不可使用关键字

### 2.标识符命名规范

标识符的命名规范：

- 见名知意
- 下划线命名法
- 英文字母全小写

多个单词组合变量名，要使用下划线做分隔。

![image-20230523173733742](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230523173733742.png)

命名变量中的英文字母，应全部小写：

![image-20230523173802221](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230523173802221.png)

## 5、运算符

### 1.算术（数学）运算符

![image-20230525155841841](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230525155841841.png)

### 2.赋值运算符

![image-20230525174754687](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230525174754687.png)

### 3.复合赋值运算符

![image-20230525174829497](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230525174829497.png)

> 注意：
>
> c /= a 等效于 c = c / a(等号左边的数在前)
>
> c **= a等效于 c = c * *a

## 6、input()输入函数

1. input()语句的功能是，获取键盘输入的数据
2. 可以使用：input(`提示信息`)，用以在使用者输入内容之前显示提示信息。
3. 要注意，无论键盘输入什么类型的数据，获取到的数据`永远都是字符串类型`

![image-20230526091127947](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230526091127947.png)

## 7、布尔类型bool

布尔（bool）表达现实生活中的逻辑，即真和假

- True表示真
- False表示假。

True本质上是一个数字记作1，False记作0。

## 8、比较运算符

比较运算符：

![image-20230526092247504](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230526092247504.png)

布尔类型的数据，不仅可以通过定义得到，也可以通过比较运算符进行内容比较得到。`使用比较运算符进行比较运算得到布尔类型的结果`。

![image-20230526092609311](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230526092609311.png)

![image-20230526092611831](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230526092611831.png)

# 三、python数据类型

------

## 1、字符串类型str

### 1.字符串类型三种定义方式

- 双引号定义： "字符串"
- 单引号定义： '字符串'
- 三引号定义：  """字符串"""（在注释前加上变量名="""注释"""是一样的结果）

![image-20230525175151985](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230525175151985.png)

![image-20230525175154768](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230525175154768.png)

![image-20230525175157002](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230525175157002.png)

> 拓展：
>
> 三引号定义法，和多行注释的写法一样，同样支持换行操作。使用变量接收它，它就是字符串不使用变量接收它，就可以作为多行注释使用。



### 2.字符串的引号嵌套

- 单引号定义法，可以内含双引号`name='"黑马程序员"'`
- 双引号定义法，可以内含单引号`name="'黑马程序员'"`
- 可以使用转移字符（\）来将引号解除效用，变成普通字符串`name='\ '黑马程序员\ ''`

### 3.字符串的拼接

字符串可以完成`字符串与字符串之间的拼接`，但不能完成`字符串与非字符串的拼接`。

![image-20230525180027581](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230525180027581.png)	

![image-20230525180031396](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230525180031396.png)

### 4.字符串格式化

#### 4.1最普通格式化写法

- 字符串变量占位：

![image-20230525180305293](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230525180305293.png)

- 多个数字类型占位（变量需要用括号（）括起来）：

​	`定义两个num类型使用%s占位方法，自动转成字符串占位`

![image-20230525180352195](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230525180352195.png)

![image-20230525180440469](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230525180440469.png)

Python中，其实支持非常多的数据类型占位

最常用的是如下三类：

![image-20230525180715969](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230525180715969.png)

![image-20230526085552043](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230526085552043.png)

#### 4.2 字符串格式化 快速写法

通过语法：f"内容{变量}"的格式来快速格式化

![image-20230526090208057](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230526090208057.png)

#### 4.3 表达式格式化

![image-20230526090431866](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230526090431866.png)

综上所述：

```python
name = "传智播客"
year = 1990
price = 19.99
print("我是%s,我成立于%d,我今天的股票结果是%5.2f"%(name, year, price))
print(f"我是{name},我成立于{year},我今天的股票价格是{price}")

##
我是传智播客,我成立于1990,我今天的股票结果是19.99
我是传智播客,我成立于1990,我今天的股票价格是19.99
```

### 5.格式化的精度控制

我们可以使用辅助符号"m.n"来控制数据的宽度和精度

- m，控制宽度，要求是数字（很少使用）,`设置的宽度小于数字自身，不生效.`
- n，控制小数点精度，要求是数字，会进行小数的四舍五入

![image-20230526085839785](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230526085839785.png)

# 四、流程控制语句

------

## 1、if判断语句

### 1.if-else语句

![image-20230526092804101](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230526092804101.png)

```python
if age >= 18:
	print("你已经成年，游玩需要补票10元。")
else:
	print("你未成年，可以免费游玩。")
print("祝你游玩愉快。")
```

### 2.if elif else 语句

![image-20230526093301009](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230526093301009.png)

```python
if age >= 18:
	print("你已经成年，游玩需要补票10元。")
elif age < 18:
	print("你未成年，可以免费游玩。")
else:
	print("祝你游玩愉快。")
```

## 2、while循环语句

![image-20230801163320165](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801163320165.png)

![image-20230801164559661](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801164559661.png)

```py
99乘法表:
    i = 1
    while i <= 9:
        j = 1
        while j <= i:
            print(f'{j} * {i} = {i * j}', end=' ')
            j += 1
        print()
        i += 1
```

## 3、for循环

![image-20230801170153147](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801170153147.png)

![image-20230801165553330](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801165553330.png)

```py
# 定义字符串name
    name = 'itheima'
    # for循环处理字符串
    for x in name: 
           print(x)
x可以先定义在循环外面，当一个外部变量；也可以不定义，直接在循环内当一个临时变量
    x = 1
    for x in 'itheima': 
           print(x)
```

### 1.range语法

```yacas
语法1：range(num)
获取一个从0开始，到num结束的数字序列（不含num本身）
如range(5)取得的数据是：[0, 1, 2, 3, 4]

语法2：range(num1,num2)//相当于[num1,num2),不含最右端
获得一个从num1开始，到num2结束的数字序列（不含num2本身）
如，range(5, 10)取得的数据是：[5, 6, 7, 8, 9]

语法3：range(num1,num2,step)
获得一个从num1开始，到num2结束的数字序列（不含num2本身）
数字之间的步长，以step为准（step默认为1）
如，range(5, 10, 2)取得的数据是：[5, 7, 9]
```

# 五、函数

## 1、函数定义语法

![image-20230801171443191](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801171443191.png)

![image-20230801171531412](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801171531412.png)

```py
def add(x, y):
    return x + y

print(add(1, 4))

```

## 2、None类型

![image-20230801171819983](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801171819983.png)

![image-20230801171910825](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801171910825.png)

## 3、函数的说明文档

![image-20230801172005076](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801172005076.png)

![image-20230801172015420](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801172015420.png)

## 4、函数多返回值

如果一个函数要有多个返回值，该如何书写代码

![image-20230802153543582](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802153543582.png)

## 5、函数多种传参方法

![image-20230802153804766](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802153804766.png)

### 1.位置传参

位置参数：调用函数时根据函数定义的参数位置来传递参数

![image-20230802154159125](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802154159125.png)

 注意：传递的参数和定义的参数的顺序及个数必须一致

### 2.关键字参数

关键字参数：函数调用时通过“键=值”形式传递参数.

![image-20230802154311090](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802154311090.png)

注意：

​	函数调用时，如果有位置参数时，位置参数必须在关键字参数的前面，但关键字参数之间不存在先后顺序

### 3.缺省参数

==缺省参数==：缺省参数也叫默认参数，用于定义函数，为参数提供默认值，调用函数时可不传该默认参数的值（注意：所有位置参数必须出现在默认参数前，包括函数定义和调用）

作用: 当调用函数时没有传递参数, 就会使用默认是用缺省参数对应的值.

![image-20230802155248695](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802155248695.png)

### 4.不定长参数

不定长参数：不定长参数也叫可变参数. 用于不确定调用的时候会传递多少个参数(不传参也可以)的场景.

不定长参数的类型:     ①位置传递     ②关键字传递

#### 4.1 位置传递（接受变量，输出为元组）

![image-20230802155641490](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802155641490.png)

传进的所有参数都会被args变量收集，它会根据传进参数的位置合并为一个`元组(tuple)，`args是元组类型，这就是位置传递

#### 4.2 关键字传值（接收字典类型）

![image-20230802155751009](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802155751009.png)

注意：参数是“键=值”形式的形式的情况下, 所有的“键=值”都会被kwargs接受, 同时会根据“键=值”组成字典.

## 6、匿名函数

![image-20230802161340565](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802161340565.png)

匿名函数定义：函数体![image-20230802162728367](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802162728367.png)

注意事项：![image-20230802163311052](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802163311052.png)

# 六、数据容器

Python的数据容器分类：列表（list）、元组（tuple）、字符串（str）、集合（set）、字典（dict）

## 1、List列表

### 1.list列表定义

![image-20230801180540101](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801180540101.png)

![image-20230801180737049](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801180737049.png)

![image-20230801180920912](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801180920912.png)

注意：列表可以一次存储多个数据，且可以为不同的数据类型，支持嵌套

### 2.list列表的下标索引

正向索引：

![image-20230801181054946](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801181054946.png)

![image-20230801181147411](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801181147411.png)

反向索引：

![image-20230801181122883](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801181122883.png)

![image-20230801181153317](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801181153317.png)

### 3.列表修改方法

![image-20230801191141779](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801191141779.png)

- 插入元素 insert()
- 追加元素 append() //将指定元素，追加到列表的尾部

![image-20230801191441591](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801191441591.png)

- 插入容器 extend(list)
- 删除元素 del list[0]  list.pop[0]

![image-20230801191914401](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801191914401.png)

- 删除元素 list.remove()
- 清空元素 list.clear()
- 统计元素 list.count() //不含重复值

![image-20230801192930577](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801192930577.png)

- 输出列表的长度 len(list)

![image-20230801193839945](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801193839945.png)

## 2、Tuple元组

### 1.元组的定义

![image-20230801194340742](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801194340742.png)

元组嵌套：

![image-20230801194353109](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801194353109.png)

注意事项：

![image-20230801194413925](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801194413925.png)

### 2.元组的方法

![image-20230801195635791](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801195635791.png)

![image-20230801195639512](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801195639512.png)

- index(元素) //输出索引
- count(元素) //输出数量

元组由于不可修改的特性，所以其操作方法非常少。

### 3.注意事项![image-20230801201344412](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801201344412.png)

### 4.元组的遍历

![image-20230801201428333](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801201428333.png)

## 3、字符串

![image-20230801202235715](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801202235715.png)

## 4、序列的切片

```
list[起始下标:结束下标:步长] [起始:结束)
```

![image-20230801202557327](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801202557327.png)

![image-20230801202736586](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801202736586.png)

## 5、Set序列

### 1.set的定义

![image-20230801203107260](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801203107260.png)

![image-20230801203127669](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801203127669.png)

### 2.注意事项

![image-20230801203139189](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801203139189.png)

### 3.集合方法

![image-20230801203202109](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801203202109.png)

### 4.集合的特点

![image-20230801203252340](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801203252340.png)

## 6、字典Dict

![image-20230801203621512](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801203621512.png)

![image-20230801203651004](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801203651004.png)

![image-20230801203708776](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801203708776.png)

## 7、数据容器特点对比

![image-20230801203912403](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801203912403.png)

![image-20230801204042720](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801204042720.png)

![image-20230801204106625](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230801204106625.png)

# 七、文件操作

![image-20230802171020579](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802171020579.png)

![image-20230802180604467](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802180604467.png)

- r : 只读
- w:覆写文件内容，会覆盖原有文件内容

## 1、文件的读取

![image-20230802171202264](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802171202264.png)

![image-20230802171257050](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802171257050.png)

![image-20230802180814260](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802180814260.png)

![image-20230802180847676](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802180847676.png)

![image-20230802180910784](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802180910784.png)

![image-20230802181035451](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802181035451.png)

![image-20230802181052680](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802181052680.png)

![image-20230802181101071](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802181101071.png)

## 2、文件的写入

![image-20230802181302439](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802181302439.png)

![image-20230802181321845](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802181321845.png)

![image-20230802181330647](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802181330647.png)

### 3、文件的追加

![image-20230802181421268](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802181421268.png)

# 八、异常、模块与包

## 1、错误和异常

错误和异常的区别：

- Error(错误)：程序无法处理，通常指程序中出现严重的问题，如：栈溢出
- Exception(异常)：程序本身可以处理的异常，可以向上抛出或捕获异常

为什么要捕获异常：

​		捕获异常的作用在于：提前假设某处会出现异常，做好提前准备，当真的出现异常的时候，可以有后续手段。

## 2、异常的捕获

### 1.捕获常规异常

![image-20230802193547809](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802193547809.png)

例：需求：尝试以`r`模式打开文件，如果文件不存在，则以`w`方式打开。

![image-20230802193556542](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802193556542.png)

### 2.捕获指定异常

![image-20230802193737879](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802193737879.png)

![image-20230802193754574](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802193754574.png)

### 3.捕获多个异常

![image-20230802193837724](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802193837724.png)

### 4.捕获异常并输出信息

![image-20230802193904306](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802193904306.png)

### 5.捕获所有异常

![image-20230802193925068](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802193925068.png)

### 6.else异常

else表示的是没有异常要执行的代码

![image-20230802194336056](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802194336056.png)

### 7.finally

finally表示的是无论是否异常都要执行的代码，例如关闭文件。

![image-20230802194421305](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802194421305.png)

## 3、异常的传递

![image-20230802195206687](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802195206687.png)

利用异常具有传递性的特点, 当我们想要保证程序不会因为异常崩溃的时候, 就`可以在main函数中设置异常捕获,` 由于无论在整个程序哪里发生异常, 最终都会传递到main函数中, 这样就可以确保所有的异常都会被捕获

## 4、模块

![image-20230802195354342](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802195354342.png)

导入方法：

![image-20230802195401948](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802195401948.png)

### 1.基本语法

`import + 模块名`

`from 模块名 + import 功能名`

![image-20230802195420885](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802195420885.png)

from 模块名 import 功能名

![image-20230802195452415](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802195452415.png)

![image-20230802195457239](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802195457239.png)

as定义别名

![image-20230802195516117](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802195516117.png)

![image-20230802195544922](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802195544922.png)

### 2.自定义模块

![image-20230802200443900](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802200443900.png)

![image-20230802200506054](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802200506054.png)

注意事项：当导入多个模块的时候，且模块内有同名功能. 当调用这个同名功能的时候，调用到的是后面导入的模块的功能

![image-20230802200511573](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802200511573.png)

![image-20230802200549028](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802200549028.png)

## 5、包

![image-20230802200626187](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802200626187.png)

### 1.新建包

![image-20230802200654361](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802200654361.png)

### 2.导入包

方式1：常规导入，**直接导入整个包的所有的功能函数。**

```
import time # 用import直接导入 python的time模块
```

![image-20230811153537952](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230811153537952.png)

导入多个包的所有功能函数

![image-20230811153619269](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230811153619269.png)

方式2：**from … import …导入整个包的部分功能函数。**

- 第一个导入的是:导入random模块（包）的randint函数。
- 第二个导入的是：导入time模块（包）的time以及localtime函数，中间用，隔开。

![image-20230811153713983](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230811153713983.png)

- 当然也可以通过from … import * 这个也是直接导入包的所有功能。相当于import …
- ![image-20230811153742005](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230811153742005.png)

![image-20230802200841742](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802200841742.png)

总结

![image-20230802200904823](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20230802200904823.png)

# 九、类和继承

## 1、类和对象

创建一个学生类：

```py
class Student:
    
    name = None
    gender = None
    nationality = None
    native_place = None
    age = None


stu_1 = Student()

stu_1.name = "周俊杰"
stu_1.gender = "男"

```

## 2、成员变量和成员方法

类中定义的属性（变量），我们称之为：成员变量

类中定义的行为（函数），我们称之为：成员方法

```py
class Student:
    
    name = None  # 类属性/成员变量
    
def say_hi(self, name):  # 类方法/成员方法
    print(f"大家好，我是{self.name}!")
```

可以看到，在方法定义的参数列表中，有一个：`self关键字`

self关键字是成员方法定义时必须传入的参数值，必须传入self;

当我们使用类对象调用方法时，self会自动被python传入

self关键字，尽管在参数列表中，但是传参的时候可以忽略它。

```py
class Student:
    name = None
   
    def say_hi1(self):
        print(f"大家好")

    def say_hi2(self, msg):
        print(f"大家好，{msg}!")

stu_1.say_hi1()  # 只含self无需传参
stu_1.say_hi2("很高兴认识大家！")  # 调用的时候只需要传入msg
```



## 3、构造方法`__init__()`

Python类可以使用：__init__()方法，称之为构造方法。

在创建类对象（构造类）的时候，将传入参数自动传递给`__init__方法`使用,init方法会自动执行。

```py
class Student:
    # age = None  # 若构造方法内含有初始化属性可以省略
    # name = None
    # gender = None

    def __init__(self, age, name, gender):  # 创建构造方法，里面初始化了三个参数age, name, gender
        self.age = age
        self.name = name
        self.gender = gender
```

构造方法内初始化了成员属性参数，单独定义的属性可以省略（如age = None）

## 4、魔术方法

上文学习的`__init__` 构造方法，是Python类内置的方法之一。

这些内置的类方法，各自有各自特殊的功能，这些内置方法我们称之为：魔术方法

![image-20231030174106506](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231030174106506.png)

### 1.`__str__()`字符串方法

首先创建一个类内部没有定义`__str__()`方法

```py
class Student:
    age = None
    name = None
    gender = None

    def __init__(self, age, name, gender):
        self.age = age
        self.name = name
        self.gender = gender
        
print(stu_1)
print(str(stu_1))

输出结果：
<__main__.Student object at 0x000001C0F7AEA400>
<__main__.Student object at 0x000001C0F7AEA400>
```

得到的print(对象)输出结果是其内存地址，一般而言对我们意义不大

当我们在类当中实现`__str__()`方法后，使用print()语句输出的是定义的信息

```py
    def __str__(self):
        return f'student类对象，name={self.name},age={self.age}'
    
输出结果：
student类对象，name=周俊杰,age=16
student类对象，name=周俊杰,age=16
```

### 2.`__it__()`小于比较方法

```py
stu_1 = Student(16, "周俊杰", "男")
stu_2 = Student(18, "林俊杰", "男")
print(stu_1 < stu_2)
```



当不定义比较方法时，执行对象比较语句：`print(stu_1 < stu_2)`

```py
    def __lt__(self, other):  # other表示另外一个用于比较的对象
        return self.age < other.age  # 当前对象的年龄小于比较对象的年龄，返回True
```

