## python代码规范

在python中可以命名的事物统称为标识符，如：

- 变量的名字

- 方法的名字

- 类的名字等等

### 1.标识符命名规则

1. 只能由数字，中文，字母，下划线组成（不推荐使用中文，数字不能开头，只能使用字母和下划线开头）
2. 大小写敏感（大小写区分不同的标识符）
3. 不可以与关键字冲突

![image-20231026152446584](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231026152446584.png)

### 2.标识符命名规范

针对不同的标识符，有不同的规范

- 变量名
- 类名
- 方法名等

2.1 变量的命名规范

1. 见名知意
2. 下划线命名：first_name（多个单词组合，使用下划线分隔）
3. 英文字母小写
4. 常量和类当中的全局变量名使用全大写：MAX_TOTLE

2.2 类名命名规范

1. 类名规范和java相同，采用大驼峰原则，且不使用下划线"_": MyClass
2. 异常名：由于异常也是类，因此也适用类名规范采用大驼峰如：
   1. class ErrorInvalidArgument(ApiError):


2.3 方法、函数、模块、包命名规范

1. 与变量命名类似，均使用小写字母、下划线"_"分隔多个单词“：calculate_total、my_module、my_package

### 3.运算符的空格规范

只存在一种运算符，运算符前后有一个空格

二元运算符，优先级低的运算符前后没有空格，优先级高的运算符前后有一个空格。

```py
i = i + 1

submitted += 1

x = x*2 - 1

hypot2 = x*x + y*y

c = (a+b) * (a-b)

```

### 4.关键字参数规范

关键字参数或者默认参数值等号需要连着写：

```py
def complex(real, imag=0.0):  # 默认参数值 imag=0.0
    return magic(r=real, i=imag)  # 默认参数值r=real, i=imag
```

### 5.换行

类与类之间前后用两个换行隔开，类当中方法与方法之间用一个换行隔开

```py
# 类与类之间前后用两个空行隔开
Class A:...


Class B:...


# 类中函数与函数之间前后用一个空行隔开
Class C:
    def c_a(self):...
    
    def c_b(self):...


# 函数与函数之间前后用两个空行隔开
def a_run():...


def b_run():...


# 在函数中使用空行来区分逻辑段（谨慎使用）。
def c_run():
    # 逻辑A
    ...
    ...
    
    # 逻辑B
    ...
    ...
```



## **Python的修饰符**

python不像java一样有修饰符如public、protected、private，而是直接在命名方式当中体现

属性和方法命名：(类属性和方法有私有、protected、公有)

- python中默认属性和方法为public公有
- `__xx` 双下划线的表示的是私有类型的属性和方法，只能由这个类本身访问:`__name='Tom'`;`__secret();`
- ``__xx__`定义的是特殊方法。如`__init__()`、`__import__()`方法

类的命名：

- 在Python中，类名不能被标记为私有。私有性质主要适用于类的成员变量和方法，而不适用于类名。类名没有这种私有性质，它们通常以普通的命名方式定义，如

```py
class MyClass:
    # 类定义   
class __MyClass: # 前面加下划线只是一种命名，无私有属性        
```

​	虽然类名没有私有性质，但可以通过其他手段来限制对类的访问，例如在模块中使用命名约定或模块级别的变量和函数，以此来控制哪些类可以被导入和使用。

## **关键字self和cls和标准类**

首先，我们来熟悉一下python标准类：

```py
class MyClass:
    class_variable = "Class Variable" # 类属性
    def __init__(self, name, age): # 在__init__()方法内初始化的类属性
        self.name = name
        self.age = age

    def prints(self): # prints()方法返回变量值
        return self.name + " " + str(self.age)  # 将age转换为字符串

# 创建类对象
obj1 = MyClass("Tom", 12)
obj2 = MyClass("Jack", 20)

# 访问类实例的属性
print(obj1.prints())  # 输出 "Tom 12"
print(obj2.prints())  # 输出 "Jack 20"

```

当然类属性你也可以单独在方法外赋初值，但是如果赋值之后扔经过init初始化，则属性值会被覆盖

```py
class MyClass:
    name = None # 单独拿出来赋值
    age = 0

    def __init__(self, name, age):# 有参构造，会被覆盖
        self.name = name
        self.age = age

    def prints(self):
        print(self.name + " " + self.age)
```

这个时候你发现，无论是在`__init__（self）`方法内有self参数，在其他的类方法prints(self)内也要传self。那么self到底是是什么呢？

其实self 表示当前类的实例，你可以把self看做这个类的本身的一个初始化对象，及self = MyClass(); 可以使用对象.属性/方法 访问到这个类自身的一些资源，==当于递归调用自己调用自己==。

```py
对于构造方法：
def __init__(self, name, age):
    self.name = name # 形参传入到类属性
    self.age = age
    
对于类方法：
def prints(self): # 也可以通过self调用自身的一些方法或属性
    print(self.name + " " + self.age)
```

总结来说，当类方法当中需要访问自身的属性和方法时，需要传入self

而对于cls而言，cls和self一样通常用于类方法中，cls表示类本身，而self代表当前对象的实例；

cls一般用在直接与类绑定的方法内，由于该方法无需创建类对象就可以访问，所以不能使用self而是cls

```py
class MyClass:
    class_variable = "I am a class variable"

    def __init__(self, instance_variable):
        self.instance_variable = instance_variable

    def instance_method(self):
        # 使用self访问实例变量
        print(self.instance_variable)

    @classmethod # 加修饰器表示该方法直接与类绑定，无需创建类对象就可以访问，不是类对象的方法
    def class_method(cls):
        # 使用cls访问类变量
        print(cls.class_variable)

# 创建实例
obj = MyClass("I am an instance variable")

# 调用实例方法
obj.instance_method()  # 使用self

# 调用类方法
MyClass.class_method()  # 使用cls

```

我们可以使用cls来访问类级别的属性：

```py
class MyClass:
    class_variable = 10

    @classmethod
    def change_class_variable(cls, value):#
        cls.class_variable = value


MyClass.change_class_variable(20)  # 使用cls来访问类级别的属性
print(MyClass.class_variable)  # 输出 20

```

cls 还允许在类方法中访问和修改类级别的属性和方法，当然self也可以修改对象属性

```py
class MyClass:
    class_variable = 10

    @classmethod
    def modify_class_variable(cls, value):
        cls.class_variable = value # 使用cls来修改类级别的属性

MyClass.modify_class_variable(20)  # 使用cls来访问类级别的属性
print(MyClass.class_variable)  # 输出 20
```

**python的数据类型**

```
- 数字型

- 整型

  - int(正整数、0、浮点数)
  - intvar（二进制整型）
  
  浮点型float
  布尔型bool
  复数类型complex
  	表示方式1：complexvar = complex(3,-91)
    表示方式2：complexvar = 3-91j

- 字符串str

- 列表list

- 元组tuple

- 集合set

- 字典dict
```



## **__name__属性**

python当中`__name__`是每个 Python 模块必备的属性

当直接执行这个模块，这段代码的 `__name__`变量等于 `'__main__`'，当这段模块被导入目标模块的时候，`__name__` 变量等于目标模块本身的名字。

因此很多时候我们可以看到代码当中有这样一行判断：

```py
if __name__ == '__main__':
    main()
    ...
```

当该模块被直接调用时， __name__变量等于 '__main__'，判断成立，直接执行以下的代码main()；当该模块导入到其他模块调用时，name变量等于其他的模块名，判断不成立；无法执行该判断下的代码。可以控制里面的代码只在本身模块内执行，在其他导入模块内不被执行。

## with关键字

with关键字可以自动调用一个对象的`上下文管理器`，使得程序在进入和离开 with 代码块时能够正确地获取和释放资源。

上下文管理器是一个实现 `__enter__` 和 **`__exit__`** 方法的类，IO文件内通常含有这两个类，而`__exit__`类似于close()方法，因此使用with关键字可自动释放IO文件资源

```py
with open('./test_runoob.txt', 'w') as file:
    file.write('hello world !')
    
等价于：
file = open('./test_runoob.txt', 'w')
file.write('hello world !')
file.close()
```

由于使用 with 语句确保在嵌套块的末尾调用 `__exit__` 方法，这个概念类似于 try...finally 块的使用，及末尾调用finally语句块，因此我们也可以用with关键字替代try...finally用于处理异常

```py
file = open('./test_runoob.txt', 'w')
try:
    file.write('hello world')
finally:
    file.close()
    
等价于：
with open('./test_runoob.txt', 'w') as file:
    file.write('hello world !')
```

## 函数和方法的区别

在python当中，函数（Function）和方法（Method）有一定的区别：

1. 函数是模块级别，不依赖于特定的对象，函数可以全局访问；
2. 而方法是类的方法，是在内部定义的，方法必须结果类对象创建调用
3. 函数可以直接使用函数名调用即可
4. 方法必须通过对象来调用

为什么java当中所有的函数统一成为方法呢，因为java当中一切皆对象，所有的函数都是在对象当中产生的。



## print()方法

输出不换行格式

```py
print(x, end="")  # end="" 可使输出不换行。
```

## import

import可以用来导入包和模块

模块是一个以.py结尾的文件，内含很多方法函数名

包包含若干个模块，本质上也还是一个模块，内含很多py文件

```py
import 模块名/包名

from 模块名 import 函数/方法
from 包名 import 包内模块

from 模块名 import * 不会导入以下划线开始的对象（私有对象）。
```



## Requests库

Requests 是⽤Python语⾔编写，基于urllib，采⽤Apache2 Licensed开源协议的 HTTP 库。

### 1、安装requests库

进入命令行win+R执行

命令：pip install requests

项目导入：import requests

### 2、requests请求方法

**GET请求：** 用于`从服务器获取数据`。例如，获取网页内容。

**POST请求：** 用于`向服务器提交数据`，通常用于提交表单或发送JSON数据。

**PUT请求：** `用于更新服务器上的资源`，通常用于更新数据。

**DELETE请求：** 用于`删除服务器上的资源`。

**PATCH请求：** 用于`部分更新`服务器上的资源。

**HEAD请求：**和GET请求类似，但`只会返回响应头部信息`

```py
import requests
requests.post('http://httpbin.org/post')
requests.put('http://httpbin.org/put')
requests.delete('http://httpbin.org/delete')
requests.head('http://httpbin.org/get')
requests.options('http://httpbin.org/get')
```

#### 1.基本GET请求

```py
import requests
 
response = requests.get('http://httpbin.org/get')  # 使用基本的get请求获取返回信息
print(response.text)
```

返回值：

```py
{
  "args": {}, 
  "headers": {
    "Accept": "*/*", 
    "Accept-Encoding": "gzip, deflate", 
    "Connection": "close", 
    "Host": "httpbin.org", 
    "User-Agent": "python-requests/2.18.4"
  }, 
  "origin": "183.64.61.29", 
  "url": "http://httpbin.org/get"
}
```

#### 2.带参数GET请求

将name和age传入进去

```py
import requests
response = requests.get("http://httpbin.org/get?name=germey&age=22")
print(response.text)


返回值：
{
  "args": {
    "age": "22", 
    "name": "germey"
  }, 
  "headers": {
    "Accept": "*/*", 
    "Accept-Encoding": "gzip, deflate", 
    "Connection": "close", 
    "Host": "httpbin.org", 
    "User-Agent": "python-requests/2.18.4"
  }, 
  "origin": "183.64.61.29", 
  "url": "http://httpbin.org/get?name=germey&age=22"
}
```

或者使用params参数

```py
import requests
 
data = {
 'name': 'germey',
 'age': 22
}
response = requests.get("http://httpbin.org/get", params=data)
print(response.text)
```

#### 3.解析json

将返回值以json的形式展示：

```py
import requests
import json
 
response = requests.get("http://httpbin.org/get")
print(type(response.text))  # 查看返回的response.text类型
print(response.json())  # 方法1：将response转换成json格式
print(json.loads(response.text))  # 方法2：将response转换成json格式
print(type(response.json()))  # 查看response.json()转化后的类型
```

#### 4.转为二进制格式

将返回值.content就ok

```py
import requests
 
response = requests.get("https://github.com/favicon.ico")
print(type(response.text), type(response.content))
print(response.text)
print(response.content)
```

返回值为二进制

#### 5.添加headers

有些网站访问时必须要带有浏览器信等信息，否则发生如下报错：

```py
import requests
 
response = requests.get("发现 - 知乎")
print(response.text)
```

返回值：

```py
<html><body><h1>500 Server Error</h1>
An internal server error occured.
</body></html>
```

当通过将浏览器等基本信息传入headers时，才能正确访问

```py
import requests
 
headers = {
 'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Safari/537.36'
}
response = requests.get("发现 - 知乎", headers=headers)
print(response.text)
```

返回正确的网站源代码

#### 6.基本POST请求

```py
import requests
 
data = {'name': 'germey', 'age': '22'}
response = requests.post("http://httpbin.org/post", data=data)
print(response.text)
```

返回：

```json
{
  "args": {}, 
  "data": "", 
  "files": {}, 
  "form": {
    "age": "22", 
    "name": "germey"
  }, 
  "headers": {
    "Accept": "*/*", 
    "Accept-Encoding": "gzip, deflate", 
    "Connection": "close", 
    "Content-Length": "18", 
    "Content-Type": "application/x-www-form-urlencoded", 
    "Host": "httpbin.org", 
    "User-Agent": "python-requests/2.18.4"
  }, 
  "json": null, 
  "origin": "183.64.61.29", 
  "url": "http://httpbin.org/post"
}
```

### 3、响应response

#### 1.response属性类型

```py
import requests

response = requests.get('简书 - 创作你的创作')
print(type(response.status_code), response.status_code)  # <class 'int'>
print(type(response.headers), response.headers)  # <class 'requests.structures.CaseInsensitiveDict'>
print(type(response.cookies), response.cookies)  # <class 'requests.cookies.RequestsCookieJar'>
print(type(response.url), response.url)  # <class 'str'>
print(type(response.history), response.history)  # <class 'list'>
return：

<class 'int'> 200
<class 'requests.structures.CaseInsensitiveDict'> {'Date': 'Thu, 01 Feb 2018 20:47:08 GMT', 'Server': 'Tengine', 'Content-Type': 'text/html; charset=utf-8', 'Transfer-Encoding': 'chunked', 'X-Frame-Options': 'DENY', 'X-XSS-Protection': '1; mode=block', 'X-Content-Type-Options': 'nosniff', 'ETag': 'W/"9f70e869e7cce214b6e9d90f4ceaa53d"', 'Cache-Control': 'max-age=0, private, must-revalidate', 'Set-Cookie': 'locale=zh-CN; path=/', 'X-Request-Id': '366f4cba-8414-4841-bfe2-792aeb8cf302', 'X-Runtime': '0.008350', 'Content-Encoding': 'gzip', 'X-Via': '1.1 gjf22:8 (Cdn Cache Server V2.0), 1.1 PSzqstdx2ps251:10 (Cdn Cache Server V2.0)', 'Connection': 'keep-alive'}
<class 'requests.cookies.RequestsCookieJar'> <RequestsCookieJar[<Cookie locale=zh-CN for 简书 - 创作你的创作>]>
<class 'str'> 简书 - 创作你的创作
<class 'list'> [<Response [301]>]
```

### 4、高级操作

#### 1.文件上传

使用 Requests 模块，上传文件也是如此简单的，文件的类型会自动进行处理：

实例：

```py
import requests
 
files = {'file': open('cookie.txt', 'rb')}
response = requests.post("http://httpbin.org/post", files=files)
print(response.text)
```

这是通过测试网站做的一个测试，返回值如下：

```json
{
  "args": {}, 
  "data": "", 
  "files": {
    "file": "#LWP-Cookies-2.0\r\nSet-Cookie3: BAIDUID=\"D2B4E137DE67E271D87F03A8A15DC459:FG=1\"; path=\"/\"; domain=\".baidu.com\"; path_spec; domain_dot; expires=\"2086-02-13 11:15:12Z\"; version=0\r\nSet-Cookie3: BIDUPSID=D2B4E137DE67E271D87F03A8A15DC459; path=\"/\"; domain=\".baidu.com\"; path_spec; domain_dot; expires=\"2086-02-13 11:15:12Z\"; version=0\r\nSet-Cookie3: H_PS_PSSID=25641_1465_21087_17001_22159; path=\"/\"; domain=\".baidu.com\"; path_spec; domain_dot; discard; version=0\r\nSet-Cookie3: PSTM=1516953672; path=\"/\"; domain=\".baidu.com\"; path_spec; domain_dot; expires=\"2086-02-13 11:15:12Z\"; version=0\r\nSet-Cookie3: BDSVRTM=0; path=\"/\"; domain=\"百度一下，你就知道\"; path_spec; discard; version=0\r\nSet-Cookie3: BD_HOME=0; path=\"/\"; domain=\"百度一下，你就知道\"; path_spec; discard; version=0\r\n"
  }, 
  "form": {}, 
  "headers": {
    "Accept": "*/*", 
    "Accept-Encoding": "gzip, deflate", 
    "Connection": "close", 
    "Content-Length": "909", 
    "Content-Type": "multipart/form-data; boundary=84835f570cfa44da8f4a062b097cad49", 
    "Host": "httpbin.org", 
    "User-Agent": "python-requests/2.18.4"
  }, 
  "json": null, 
  "origin": "183.64.61.29", 
  "url": "http://httpbin.org/post"
}
```

#### 2.获取cookie

当需要cookie时，直接调用response.cookie:(response为请求后的返回值)

```py
import requests
 
response = requests.get("百度一下，你就知道")
print(response.cookies)
for key, value in response.cookies.items():
 print(key + '=' + value)
```

输出结果：

```py
<RequestsCookieJar[<Cookie BDORZ=27315 for .baidu.com/>]>
BDORZ=27315
```

## Json数据格式化

json是一种各个语言通用的用于传输数据的格式，如java和python的数据格式不同，但是可以通过相同的json格式传递数据；我们把java和python比作比作各自的方言，json格式就好比普通话，他们可以通过json传递对方都能够接受的信息格式。

json的数据格式可以是：

```py
{"name":"admin","age":18}
或者：
{["name":"admin","age":18],["name":"root","age":16],["name":"张三","age":20]}
```

我们类比可以发现，json格式在python当中无非就是一个字典，或者是一个列表嵌套字典两种格式

我们可以通过json库调用两个方法转换：

```py
import json

data = [{"name": "老王", "age": 16}, {"name": "张三", "age": 20}]

data_str = json.dumps(data)  # 将json数据转化为str字符串
print(type(data_str), end="")
print(":"+data_str)
data_json = json.loads(data_str)  # 将python数据转化为json格式（在python中显示为list或dict格式）
print(type(data_json), end=":")
print(data_json)

输出：
<class 'str'>:[{"name": "\u8001\u738b", "age": 16}, {"name": "\u5f20\u4e09", "age": 20}]
<class 'list'>:[{'name': '老王', 'age': 16}, {'name': '张三', 'age': 20}]
```

这里我们注意的一个点是，通过json.loads(data_str)方法可以将json格式的数据转化为list或dict类型，因此我们就可以使用这两个数据类型的方法操作这些数据

我们发现输出内容中通过json.dumps(data)json转为字符串方法，中文变成了转义字符

我们可以在里面加一个参数可以将中文正常显示出来:

```py
data_str = json.dumps(data, ensure_ascii=False)  # 添加参数ensure_ascii=False，将中文正常显示

输出：
<class 'str'>:[{"name": "老王", "age": 16}, {"name": "张三", "age": 20}]
<class 'list'>:[{'name': '老王', 'age': 16}, {'name': '张三', 'age': 20}]
```



## assert断言

assert接一个表达试[expression]和[message],message可以省略

```py
assert expression, message
```

当expression表达式为True时,则程序继续执行；如果为 `False`，则引发 `AssertionError` 异常。

`message` 是一个可选的字符串，用于在断言失败时提供附加的错误信息。

```py
def divide(a, b):
    assert b != 0, "除数不能为零"  # 如果b!=0判断为false,则抛出Assertion异常并输出“除数不能为零”
    return a / b
```



## python字符转义

在Python中，字符串的转义通常通过在特定字符前添加反斜杠（\）来实现。以下是一些常见的转义字符：

- `\\`：反斜杠，用于表示一个反斜杠字符。
- `\'`：单引号，用于表示一个单引号字符。
- `\"`：双引号，用于表示一个双引号字符。
- `\n`：换行符，表示字符串中的换行。
- `\t`：制表符，表示字符串中的水平制表符。

如下所示：

```py
escaped_string = 'This is a string with a \\ backslash, a \'single quote\', and a \"double quote\". \nThis is a new line. \tThis is a tab.'
print(escaped_string)
```

这将输出：

```py
This is a string with a \ backslash, a 'single quote', and a "double quote".
This is a new line. 	This is a tab.
```

在Python中，反斜杠是用于转义字符的，默认情况下，Python字符串是不解释转义字符的原始字符串，但在一些特殊情况下，你可能需要使用转义字符。

windows内的文件路径使用的都是\,会导致文件路径发生转义，如下：

![image-20231113154549717](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231113154549717.png)

`\test`转义为制表符`\t`

因此程序中我们一般使用返斜杠来表示文件路径：/

```py
D:/workspacedemo/wenxuan.qiu/sw_itest_clone/tests/pytestCase/test_blkdev.py
```

![image-20231113154705461](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231113154705461.png)



## python推导式

Python 推导式是一种独特的数据处理方式，可以从一个数据序列构建另一个新的数据序列的结构体。

- 列表(list)推导式
- 字典(dict)推导式
- 集合(set)推导式
- 元组(tuple)推导式

### 列表推导式

格式：

```py
arrs = [arr for a in list <if condition>]
```

- arr：接受迭代变量a,也可以是有返回值的函数接受a
- for a in list：迭代list，将a值传入到arr内
- if condition：条件语句，可以过滤列表中不符合条件的值。

实例:

```py
#  过滤掉长度小于或等于3的字符串列表，并将剩下的转换成大写字母：
names = ['Bob','Tom','alice','Jerry','Wendy','Smith']
new_names = [name.upper()for name in names if len(name)>3]
print(new_names)
['ALICE', 'JERRY', 'WENDY', 'SMITH']
```

```py
#  计算 30 以内可以被 3 整除的整数：
multiples = [i for i in range(30) if i % 3 == 0]
print(multiples)
[0, 3, 6, 9, 12, 15, 18, 21, 24, 27]
```

### 字典推导式

字典推导基本格式：

```py
{ key_expr: value_expr for value in collection }
或
{ key_expr: value_expr for value in collection if condition }
```

实例

```py
# 使用字符串及其长度创建字典：
listdemo = ['Google','Runoob', 'Taobao']
将列表中各字符串值为键，各字符串的长度为值，组成键值对
newdict = {key:len(key) for key in listdemo}

{'Google': 6, 'Runoob': 6, 'Taobao': 6}
```

### 集合推导式

集合推导式基本格式：

```py
{ expression for item in Sequence }
或
{ expression for item in Sequence if conditional }
```

实例

```py
# 计算数字 1,2,3 的平方数：
setnew = {i**2 for i in (1,2,3)}
setnew
{1, 4, 9}
```



```py
判断不是 abc 的字母并输出：
a = {x for x in 'abracadabra' if x not in 'abc'}
type(a)

a={'d', 'r'}
<class 'set'>
```



### 元组推导式（生成器表达式）

元组推导式可以利用 range 区间、元组、列表、字典和集合等数据类型，快速生成一个满足指定需求的元组。

元组推导式基本格式：

```py
(expression for item in Sequence )
或
(expression for item in Sequence if conditional )
```

元组推导式和列表推导式的用法也完全相同，只是元组推导式是用 **()** 圆括号将各部分括起来，而列表推导式用的是中括号 **[]**，另外元组推导式返回的结果是一个`生成器对象`。

实例

```py
# 例如，我们可以使用下面的代码生成一个包含数字 1~9 的元组：
a = (x for x in range(1,10))

<generator object <genexpr> at 0x7faf6ee20a50> # 返回的是生成器对象

tuple(a)    # 使用 tuple() 函数，可以直接将生成器对象转换成元组
(1, 2, 3, 4, 5, 6, 7, 8, 9)
```



## 迭代器和生成器

### 迭代器

迭代是 Python 最强大的功能之一，是访问集合元素的一种方式。

迭代器是一个可以记住遍历的位置的对象。

迭代器对象从集合的第一个元素开始访问，直到所有的元素被访问完结束。迭代器只能往前不会后退。

迭代器有两个基本的方法：**iter()** 和 **next()**。

字符串，列表或元组对象都可用于创建迭代器：

```py
list=[1,2,3,4]
it = iter(list)    # 创建迭代器对象
print (next(it))   # 输出迭代器的下一个元素
1
print (next(it))
2
```

迭代器对象可以使用常规for语句进行遍历：

```py
list=[1,2,3,4]
it = iter(list)    # 创建迭代器对象
for x in it:
    print (x, end=" ")
    
输出结果：
1 2 3 4
```

使用next()函数遍历

```py
import sys         # 引入 sys 模块
 
list=[1,2,3,4]
it = iter(list)    # 创建迭代器对象
 
while True:
    try:
        print (next(it))
    except StopIteration:
        sys.exit()
        
输出结果：
1
2
3
4
```

### 类创建迭代器

把一个类作为一个迭代器使用需要在类中实现两个方法 `__iter__()` 与 `__next__() `

如果你已经了解的面向对象编程，就知道类都有一个构造函数，Python 的构造函数为 `__init__()`, 它会在对象初始化的时候执行。

`__iter__()` 方法返回一个特殊的迭代器对象， 这个迭代器对象实现了 `__next__()` 方法并通过 StopIteration 异常标识迭代的完成。

`__next__()` 方法（Python 2 里是 next()）会返回下一个迭代器对象。

创建一个返回数字的迭代器，初始值为 1，逐步递增 1：

```py
class MyNumbers:
  def __iter__(self):
    self.a = 1
    return self
 
  def __next__(self):
    x = self.a
    self.a += 1
    return x
 
myclass = MyNumbers()
myiter = iter(myclass)
 
print(next(myiter))
print(next(myiter))
print(next(myiter))
print(next(myiter))
print(next(myiter))
1
2
3
4
5
```

### StopIteration

StopIteration 异常用于标识迭代的完成，防止出现无限循环的情况，在 __next__() 方法中我们可以设置在完成指定循环次数后触发 StopIteration 异常来结束迭代。

在 20 次迭代后停止执行：

```py
class MyNumbers:
  def __iter__(self):
    self.a = 1
    return self
 
  def __next__(self):
    if self.a <= 20:
      x = self.a
      self.a += 1
      return x
    else:
      raise StopIteration
 
myclass = MyNumbers()
myiter = iter(myclass)
 
for x in myiter:
  print(x)
```

### 生成器

在 Python 中，使用了 **yield** 的函数被称为生成器（generator）。

**yield** 是一个关键字，用于定义生成器函数，==生成器函数是一种特殊的函数，可以在迭代过程中逐步产生值，而不是一次性返回所有结果==。

跟普通函数不同的是，生成器是一个返回迭代器的函数，只能用于迭代操作，更简单点理解生成器就是一个迭代器。

当在生成器函数中使用 **yield** 语句时，函数的执行将会暂停，并将 **yield** 后面的表达式作为当前迭代的值返回。

然后，每次调用生成器的 **next()** 方法或使用 **for** 循环进行迭代时，函数会从上次暂停的地方继续执行，直到再次遇到 **yield** 语句。这样，生成器函数可以逐步产生值，而不需要一次性计算并返回所有结果。

调用一个生成器函数，返回的是一个迭代器对象。

下面是一个简单的示例，展示了生成器函数的使用：

```py
def countdown(n):
    while n > 0:
        yield n
        n -= 1
 
# 创建生成器对象
generator = countdown(5)
 
# 通过迭代生成器获取值
print(next(generator))  # 输出: 5
print(next(generator))  # 输出: 4
print(next(generator))  # 输出: 3
 
# 使用 for 循环迭代生成器
for value in generator:
    print(value)  # 输出: 2 1
```

以上实例中，**countdown** 函数是一个生成器函数。它使用 yield 语句逐步产生从 n 到 1 的倒数数字。在每次调用 yield 语句时，函数会返回当前的倒数值，并在下一次调用时从上次暂停的地方继续执行。

通过创建生成器对象并使用 next() 函数或 for 循环迭代生成器，我们可以逐步获取生成器函数产生的值。在这个例子中，我们首先使用 next() 函数获取前三个倒数值，然后通过 for 循环获取剩下的两个倒数值。

生成器函数的优势是它们可以按需生成值，避免一次性生成大量数据并占用大量内存。此外，生成器还可以与其他迭代工具（如for循环）无缝配合使用，提供简洁和高效的迭代方式。

```py
# 使用yield实现斐波那契
import sys
 
def fibonacci(n): # 生成器函数 - 斐波那契
    a, b, counter = 0, 1, 0
    while True:
        if (counter > n): 
            return
        yield a
        a, b = b, a + b
        counter += 1
f = fibonacci(10) # f 是一个迭代器，由生成器返回生成
 
while True:
    try:
        print (next(f), end=" ")
    except StopIteration:
        sys.exit()
        
输出结果：
0 1 1 2 3 5 8 13 21 34 55
```



## 读文件和写文件

这里我以读取yaml文件为例

读取yml文件 `yaml.safe_load(file)`

```py
with open(input_file_path, 'r') as file:
	file_data = yaml.safe_load(file) # yaml.safe_load()函数会读取ymal文件的全部信息并转化为python识别的类型
```

创建并写入yml文件 `yaml.dump(file_output_data, file)` 

```py
file_output_data={...}
with open(output_file_path, 'w') as file:
	yaml.dump(file_output_data, file) # 将python数据类型的数据转化为yaml格式写入file文件内
```



## json数据格式转换

首先介绍一下json库的两个方法：如

**`json.loads()`** 和 **`json.dumps()`**

`json.loads()`函数用于`将 JSON 格式的字符串解析为 Python 的数据结构`，比如字典或列表。

```py
import json
json_string = '{"name": "John", "age": 30, "city": "New York"}'
python_dict = json.loads(json_string)
```

这样解析有什么好处呢，在python中其实json数据格式刚好与python的集合字典嵌套类似，我们需要把它转化为python自己本身的数据类型才能使用数据类型自带的一些方法；



`json.dumps()` 函数用于将 Python 的数据结构转换为 JSON 格式的字符串。

```py
import json
python_dict = {"name": "John", "age": 30, "city": "New York"}
json_string = json.dumps(python_dict)
```

在这个例子中，`json.dumps()` 将一个 Python 字典转换成了一个 JSON 格式的字符串。



## 使用命令行执行py文件传入参数

使用python解释器执行script.py文件，接受两个参数，命令如下

```py
python script.py 参数1 参数2
```

我们需要在script.py脚本导入`sys`库

```py
import sys
```

**从 `sys.argv` 读取参数**：

`sys.argv` 是一个列表，其中 `sys.argv[0]` 是脚本名称，`sys.argv[1]` 是第一个参数，`sys.argv[2]` 是第二个参数，依此类推。

```py
# 假设我们需要两个参数
arg1 = sys.argv[1]
arg2 = sys.argv[2]
```



## 创建管理进程subprocess

### subprocess.run() 创建阻塞进程

- subprocess.run()函数用于执行一个命令并创建一个进程，阻塞当前程序的运行知道进程执行完毕
- 它返回一个 `CompletedProcess` 对象，其中包含了执行结果的相关信息，例如退出码、标准输出和标准错误输出。

### subprocess.Popen() 创建并发进程

- 函数用于执行一个命令并创建一个并发进程，而非阻塞调用，意味着 当前python程序会继续执行，而命令在后台运行。
- 它返回一个 `Popen` 对象，可以通过该对象与后台进程进行交互（例如获取输出、提供输入或等待进程结束）。
- 由于该命令不会暂停程序执行，因此我们在执行玩命令后需要使用`wait()`关闭该进程

```py
def main(args):
    for file in get_numbered_files():  # 遍历文件名
        if file.startswith('1'):
            print(f'{file}运行中……')
            # 对于以 '1' 开头的文件，传递命令行参数并使用 subprocess.run
            subprocess.run(['python', file]+args)  # 创建一个阻塞进程
        else:
            print(f'{file}运行中……')
            # 对于其他文件，不传递参数并使用 subprocess.Popen
            process = subprocess.Popen(['python', file]) # 创建一个并发进程
            process.wait()

        print(f'{file}运行完毕！')

```



## run_cli()方法运行robot用例

  robotframework框架下有一个run_cli()方法,用于将Robotframework命令以python的格式执行

如下所示：py文件的入口函数：

```
from robot import run_cli
run_cli(sys.argv[1:]) # 获取命令以robot的规则运行
```

  如使用python解释器运行这个入口py文件时，输入命令：

```py
python D:\ProjectDemo\sw_itest\tests\robotFramework\DPE\pybot-cli.py -s ovs-inline.inline-offload.ovs-match-fwd --loglevel DEBUG --include ok   D:\Pro
jectDemo\sw_itest\tests\robotFramework\DPE


```

以python解释器运行py文件，但参数使用robot的参数形式，传入到文件run_cli(sys.argv[1:])中获取robot参数执行robot命令

如：

```py
python # 
D:\ProjectDemo\sw_itest\tests\robotFramework\DPE\pybot-cli.py # 以python解释器运行指定文件名
-s ovs-inline.inline-offload.ovs-match-fwd # -s 或 --suite，用于指定要运行的测试套件
--loglevel DEBUG # 该选项用于设置日志的详细程度，常见的日志级别包括 TRACE, DEBUG, INFO, WARN, ERROR。
--include ok   # 使用 --include 选项可以运行那些具有特定标签的测试用例，如图表示运行带有ok标签的测试用例
D:\ProjectDemo\sw_itest\tests\robotFramework\DPE

```



## 读取文件和写入文件与验证是否写入成功

### 读取文件

```py
def read_yml(input_file_path):
    with open(input_file_path, 'r', encoding='utf-8') as file:
        input_data = yaml.safe_load(file)
        return input_data
```

### 写入文件

```py
def write_yml(output_data, output_file_path):
    with open(output_file_path, 'w', encoding='utf-8') as file:
        yaml.dump(output_data, file, allow_unicode=True)  # 写入的时候，allow_unicode=True表示关闭汉字转码
```

### 验证是否写入成功

```py
#  验证文档是否生成,后面需要删除
with open(output_file, 'r', encoding='utf-8') as file:
    content = yaml.safe_load(file)  # 读取文件内容，如果存在则成功写入
    if content == yaml_data:
        print("文件成功写入")
    else:
        print("文件写入有误")
```



## Pthon项目目录

- bin 执行文件
- conf 配置文件，含config.yaml
- core 业务核心脚本文件
- db 数据库文件
- lib 脚本，第三方文件
- log 日志文件
- res 资源文件（图片，ui,资源文件）
- tests 测试文件

![image-20231127104542748](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231127104543.png)

![img](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/v2-ec4a9b3e52f7054306ee76916f426b94_1440w.webp)

## 字典dict()

### 字典键的特性

字典值可以是任何的 python 对象，既可以是标准的对象，也可以是用户定义的，但键不行。

两个重要的点需要记住：

1）不允许同一个键出现两次。创建时如果同一个键被赋值两次，后一个值会被记住，如下实例：

### 实例

```
tinydict = {'Name': 'Runoob', 'Age': 7, 'Name': '小菜鸟'}  
print ("tinydict['Name']: ", tinydict['Name'])
```

以上实例输出结果：

```
tinydict['Name']:  小菜鸟
```

2）`键必须不可变，所以可以用数字，字符串或元组充当`，而用列表就不行，如下实例：

### 实例

```
tinydict = {['Name']: 'Runoob', 'Age': 7}  
print ("tinydict['Name']: ", tinydict['Name'])
```

以上实例输出结果：

```
Traceback (most recent call last):
  File "test.py", line 3, in <module>
    tinydict = {['Name']: 'Runoob', 'Age': 7}
TypeError: unhashable type: 'list'
```

## random()

```
random.sample(itr,num) # 从itr可迭代对象中随机选择num个对象

```



## split()函数

```
str.split(sep,num) # 以sep为符号切分字符串函数，切分的最大块速为num+1块
str.split() #默认切分以空格为分隔符的字符串
如：sentence = "I love Python programming language"
	words = sentence.split(' ', 2) # Output: ['I', 'love', 'Python programming language']
	
如：csv_data = "apple,orange,banana,grape"
fruits = csv_data.split(',')
print(fruits)  # Output: ['apple', 'orange', 'banana', 'grape']
```



## pdb 调试工具

n (next)：往前执行一行

s(step):进去某个函数往前执行一行

c(continue):开始执行，直到遇到下一个断电

l(list):列出当前指向的那一行和附近的行（指向的一行表示该行还没有运行）

a(args):输出当前函数的参数列表

### 一、pdb 2种用法

pdb：python debugger

#### 1、非侵入式方法 （不用额外修改源代码，在命令行下直接运行就能调试）

```py
python3 -m pdb filename.py
```

#### 2、侵入式方法 （需要在被调试的代码中添加以下代码然后再正常运行代码）

```py
import pdb;pdb.set_trace()
```

当你在命令行看到下面这个提示符时，说明已经正确打开了pdb

```py
(pdb)
```

### 二、python常用命令

| 命令          | 解释                                           |
| :------------ | :--------------------------------------------- |
| break 或 b    | 设置断点                                       |
| continue 或 c | 跳转到下一个断点                               |
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

#### 1、break/b 设置断点

```py
b line # 在当前文件的指定行号设置断点
b 26 # 表示在当前文件的26行设置断点

b filename.py:line # 表示在指定文件的26行设置断点
break encoder.py:12 # 在文件encoder.py的12行设置断点

tbreak # 临时添加断点
```

```py
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

#### 2、s/step、n/next、r/return、c/contiue、u/up执行下一条语句

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



### 3、cl 清除断电

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



#### 4、p/print 打印变量值

```py
p expression
```



#### 5、l/list 打印当前所指位置

```py
l # 查看当前位置前后11行源代码（多次会翻页）,当前位置在代码中会用-->这个符号标出来，指针所指位置表示还未执行到该步
ll # 查看当前函数或框架的所有源代码
```



#### 6、w 打印堆栈信息

```py
w # 打印堆栈信息，最新的帧在最底部。箭头表示当前帧。
```



#### 7、q/exit 退出pdb

```py
q # 退出pdb
```



#### 8、pdb.pm() 程序崩溃后调试

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



## Python保留关键字

```py
>>> import keyword
>>> keyword.kwlist
[>>> import keyword
>>> keyword.kwlist
['False', 'None', 'True', 'and', 'as', 'assert', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']
]

```

python的`保留关键字`有：

'False', 'None', 'True', 'and', 'as', 'assert', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield'

## 多行语句

Python 通常是一行写完一条语句，但如果语句很长，我们可以使用反斜杠 `\` 来实现多行语句，例如：

```py
total = item_one + \
        item_two + \
        item_three
```

而在 [], {}, 或 () 中的多行语句，不需要使用反斜杠 \，例如：

```py
total = ['item_one', 'item_two', 'item_three',
        'item_four', 'item_five']
```

## 同一行显示多条语句

Python 可以在同一行中使用多条语句，语句之间使用分号 `;`分割，以下是一个简单的实例：

```py
import sys; x = 'runoob'; sys.stdout.write(x + '\n')
```

## import导入

在 python 用 **`import`** 或者 **`from...import`** 来导入相应的模块。

将整个模块(somemodule)导入，格式为： **`import somemodule`**

从某个模块中导入某个函数,格式为： **`from somemodule import somefunction`**

从某个模块中导入多个函数,格式为： **`from somemodule import firstfunc, secondfunc, thirdfunc`**

将某个模块中的全部函数导入，格式为： **`from somemodule import *`** 类似于**`import somemodule`**

```py
import sys
print('================Python import mode==========================')
print ('命令行参数为:')
for i in sys.argv:
    print (i)
print ('\n python 路径为',sys.path)


from sys import argv,path  #  导入特定的成员
 
print('================python from import===================================')
print('path:',path) # 因为已经导入path成员，所以此处引用时不需要加sys.path
```

## getopt模块

getopt 模块是专门处理命令行参数的模块，用于获取命令行选项和参数，也就是 **`sys.argv`**。命令行选项使得程序的参数更加灵活。支持短选项模式 **`-`** 和长选项模式 **`--`**。

### getopt.getopt 方法

getopt.getopt 方法用于解析命令行参数列表，语法格式如下：

```py
getopt.getopt(args, options[, long_options])
```

方法参数说明：

- **args**: 要解析的命令行参数列表。
- **options**: 以字符串的格式定义，**options** 后的冒号 **:** 表示该选项必须有附加的参数，不带冒号表示该选项不附加参数。
- **long_options**: 以列表的格式定义，**long_options** 后的等号 **=** 表示如果设置该选项，必须有附加的参数，否则就不附加参数。



获取命令行参数语句：

```py
args = sys.argv[1:]
```

在解释这个语句之前，我们需要了解以下python命令行脚本执行

## Python 命令行参数

当你从命令行运行一个Python脚本时，你可以传递一些参数给这个脚本。这些参数可以是文件名、数据值或者其他配置信息。例如，在命令行运行：

```py
python myscript.py arg1 arg2 arg3
```

这里，`myscript.py` 是你的Python脚本，而 `arg1`, `arg2`, `arg3` 是传递给脚本的命令行参数。

当然可以。在解释这个语句之前，我们需要了解一些背景信息。

`sys.argv`

在Python中，这些命令行参数可以通过 `sys` 模块的 `argv` 属性来访问。`sys.argv` 是一个列表，其中包含了命令行中指定的所有参数。在上面的例子中，`sys.argv` 将是：

```py
['myscript.py', 'arg1', 'arg2', 'arg3']
```

注意，列表的第一个元素（`sys.argv[0]`）总是脚本的名称，接下来的元素是传递给脚本的参数。

因此`args = sys.argv[1:]`表示获取除第一个脚本名称元素外的所有元素，即获取命令行的所有参数

短选项实例：

```py
import sys 
import getopt 
  
  
def site(): 
    name = None
    url = None
  
    argv = sys.argv[1:] 
  
    try: 
        opts, args = getopt.getopt(argv, "n:u:")  # 短选项模式
      
    except: 
        print("Error") 
  
    for opt, arg in opts: 
        if opt in ['-n']: 
            name = arg 
        elif opt in ['-u']: 
            url = arg 
      
  
    print( name +" " + url) 
  
site()
```

代码输入：

```
python3 test.py -n RUNOOB -u www.runoob.com
```

输出结果：

```
RUNOOB www.runoob.com
```

长选项实例：

```py
import sys 
import getopt 
  
def site(): 
    name = None
    url = None
  
    argv = sys.argv[1:] 
  
    try: 
        opts, args = getopt.getopt(argv, "n:u:",  
                                   ["name=", 
                                    "url="])  # 长选项模式
      
    except: 
        print("Error") 
  
    for opt, arg in opts: 
        if opt in ['-n', '--name']: 
            name = arg 
        elif opt in ['-u', '--url']: 
            url = arg 
      
  
    print( name + " " + url) 
  
site()
```

输出结果同上

## 判断变量类型

内置的 type() 函数可以用来查询变量所指的对象类型。

```py
>>> a, b, c, d = 20, 5.5, True, 4+3j
>>> print(type(a), type(b), type(c), type(d))
<class 'int'> <class 'float'> <class 'bool'> <class 'complex'>
```

此外还可以用 isinstance 来判断：

```py
>>> a = 111
>>> isinstance(a, int)
True
>>>
```

isinstance 和 type 的区别在于：

- type()不会认为子类是一种父类类型。
- isinstance()会认为子类是一种父类类型。

如下：

```py
>>> class A:
...     pass
... 
>>> class B(A):
...     pass
... 
>>> isinstance(A(), A)
True
>>> type(A()) == A 
True
>>> isinstance(B(), A)
True
>>> type(B()) == A
False
```

## python转义字符

Python 使用反斜杠 `\` 转义特殊字符，如果你不想让反斜杠发生转义，可以在字符串前面添加一个 **`r`**，表示原始字符串：

```py
>>> print('Ru\noob')
Ru
oob
>>> print(r'Ru\noob')
Ru\noob
>>>
```

