# Python自动化框架笔记

# **一、单元测试框架**

## 1、测试框架类型

Unittest、Pytest框架

- Unittest框架
  - Python内置的标准框架类（无需额外安装）。类似于java的JUnit、.net的NUnit、C++的CppUnit

- Pytest框架
  - 丰富、灵活的测试框架，语法简单，可以结合allure生成酷炫的测试报告，目前主流的测试框架。

## **2、单元测试的覆盖类型**

### **1.语句覆盖**

- 通过设计一定量的测试用例，保证被测试的方法每一行代码都会被执行一遍

- 语句覆盖是一个最基础的覆盖方式，但也是最弱的，不能全部依赖语句覆盖

```py
def demo_method(a, b, x):
    if (a > 1 and b == 0):
        x = x / 2
    if (a == 2 or x > 1):
        x = x + 1
    return x

测试用例：
    当a=3、b=0、x=4即可全部覆盖用例。
漏洞：
    如果开发人员将代码的条件or写错成and，用例（a=3、b=0、x=4）仍然可以通过，无法发现代码的错误。
```

### **2.判断覆盖**

- 设计用例覆盖每一个判断条件 

```py
def demo_method(a, b, x):
    if (a > 1 and b == 0):
        x = x / 2
    if (a == 2 or x > 1):
        x = x + 1
    return x
漏洞：
    判断语句大部分都仅仅判断其整个最终结果，忽略了每个条件的取值情况，必然会影响部分测试结果。
```

| Test Cases | a b x | a>1 && b==0 | a==2 \|\| x>1 |
| ---------- | ----- | ----------- | ------------- |
| Case1      | 2 0 2 | T           | T             |
| Case2      | 1 2 1 | F           | F             |
| Case3      | 3 0 3 | T           | F             |
| Case4      | 2 1 1 | F           | T             |

### **3.条件覆盖**

- 条件覆盖与判断覆盖类似，判断覆盖关注整个判断语句，而条件覆盖则关注某个判断条件。


  测试条件1：（a>1 && b==0）

| Test Cases | a b  | a>1  | b==0 |
| ---------- | ---- | ---- | ---- |
| Case1      | 2 0  | T    | T    |
| Case2      | 1 1  | F    | F    |
| Case3      | 0 0  | F    | T    |
| Case4      | 2 1  | T    | F    |

  测试条件2：(a==2 || x>1)

| Test Cases | a x  | a==2 | x>1  |
| ---------- | ---- | ---- | ---- |
| Case1      | 2 2  | T    | T    |
| Case2      | 1 1  | F    | F    |
| Case3      | 0 2  | F    | T    |
| Case4      | 2 1  | T    | F    |

  ```py
def demo_method(a, b, x):
    if (a > 1 and b == 0):
        x = x / 2
    if (a == 2 or x > 1):
        x = x + 1
    return x
缺陷：
    虽然能够足够全面的覆盖到我们所有的代码，但由例子可知，两个测试条件分别独立，需要设计用例数为4X4条用例，用例个数随条件的增多呈指数增长，测试的成本过高
  ```

  ### **4.路径覆盖 (使用最多)**

  ![image-20231017145220919](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017145220919.png)

  - 覆盖所有可能执行的用例

  测试用例

| Test Cases | a b x | ExcecutePath |
| ---------- | ----- | ------------ |
| Case1      | 2 0 3 | 135          |
| Case2      | 1 0 1 | 124          |
| Case3      | 3 0 3 | 134          |
| Case4      | 2 1 1 | 125          |

  # **二、Unittest框架**

  ## **1、Unittest组件**

  - test fixture
  - test case
  - test suite
  - test runner

  ![image-20231017145321730](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017145321730.png)

  官网代码案例：https://docs.python.org/3/library/unittest.html#unittest.TextTestRunner

  ```py
import unittest # 导入python自带的Unittest包

class TestStringMethods(unittest.TestCase):

    def test_upper(self):
        self.assertEqual('foo'.upper(), 'FOO')

    def test_isupper(self):
        self.assertTrue('FOO'.isupper())
        self.assertFalse('Foo'.isupper())

    def test_split(self):
        s = 'hello world'
        self.assertEqual(s.split(), ['hello', 'world'])
        # check that s.split fails when the separator is not a string
        with self.assertRaises(TypeError):
            s.split(2)

if __name__ == '__main__':
    unittest.main()
  ```

  ## **2、Unittest编写规范**

  - 测试模块需先 `import unittest`
  - 测试类必须继承 `unittest.TestCase`，对测试类名字没有要求
  - 测试用例（方法）必须以`“test_ ”`开头

  ## **3、测试框架结构**

  ### **1.setUp、tearDown方法**

  - 当定义了这两个方法后，setUp和tearDown方法会在每个测试方法执行时前后自动执行

  ```py
class TestStringMethods(unittest.TestCase):
    def setUp(cls) -> None:  # -> None 表示返回值为None(空),setUp方法
        print("setUp...")

    def tearDown(cls) -> None:  # tearDown方法
        print("tearDown...")

    def test_a1(selfs):
        print("a1...")

    def test_a2(selfs):
        print("a2...")
  ```

  执行结果：

  > setUp...
  >
  > a1...
  >
  > tearDown...
  >
  > setUp...
  >
  > a2...
  >
  > tearDown...

  ### **2.setUpClass、tearDownClass方法**

  - 类级别方法setUpClass用来为测试准备环境、tearDownClass用来清理环境。
  - 定义了这两个方法后，当继承了unittest.TestCase的测试类执行时开头和结尾会自动调用这两个方法。
  - 注意由UpClass、tearDownClass是于set类级别的方法，需要在前面加上类方法装饰器

  ```py
class TestStringMethods(unittest.TestCase):
    def setUp(cls) -> None:  # -> None 表示返回值为None(空),setUp方法
        print("setUp...")

    def tearDown(cls) -> None:  # tearDown方法
        print("tearDown...")

    @classmethod #由于setUpClass是类级别的方法，需要在前面加上类方法装饰器
    def setUpClass(cls) -> None:  # setUpClass方法
        print("setUpClass...")

    @classmethod
    def tearDownClass(cls) -> None:  # tearDownClass方法
        print("tearDownClass...")

    def test_a1(selfs):
        print("a1...")

    def test_a2(selfs):
        print("a2...")

  ```

  运行结果：

  > setUpClass...
  >
  > setUp...
  >
  > a1...
  >
  > tearDown...
  >
  > setUp...
  >
  > a2...
  >
  > tearDown...
  >
  > tearDownClass...

  setUpClass和tearDownClass的作用实例：

  ```py
import unittest # ①先导入自带unittest类


class Search: # 创建一个Search类
    def search_func(self):
        print("创建search对象")
        return True


class TestSearch(unittest.TestCase): # ②测试类要继承父类unittest.TestCase

    def test_search1(self): # 测试方法1，创建一个类对象，并断言
        search1 = Search() 
        assert True == search1.search_func() # 断言：检查返回值是否为True，如果为False，会引发 
AssertionError 异常

    def test_search2(self):# 测试方法2，创建一个类对象，并断言
        search2 = Search()
        assert True == search2.search_func()

    def test_search3(self):# 测试方法3，创建一个类对象，并断言
        search3 = Search()
        assert True == search3.search_func()


if __name__ == '__name__': # 
    unittest.main()

  ```

  运行结果：

  > 创建search对象
  >
  > 创建search对象
  >
  > 创建search对象

  以上代码由于每个测试方法都需要初始化一个类对象，我们可以将类对象的创建初始化一起放在setUpClass方法内，每次调用测试类自动初始化一次

  ```py
import unittest


class Search:
    def search_func(self):
        print("创建search对象")
        return True


class TestSearch(unittest.TestCase):

    @classmethod # 类方法装饰器表示直接通过类名称便可访问到，当然也可以创建类对象访问，类对象传入的参数不是self而是cls
    def setUpClass(cls) -> None: # 创建setUpClass方法初始化对象，
        cls.search = Search() # 相当于定义本类属性Search()对象为search
        
    @classmethod
    def tearDownClass(cls) -> None: # 创建setUpClass方法,一般用来清理环境
        print("tearDownClass...")
        
    def test_search1(self):
        # search1 = Search()
        assert True == self.search.search_func()

    def test_search2(self):
        # search2 = Search()
        assert True == self.search.search_func()

    def test_search3(self):
        # search3 = Search()
        assert True == self.search.search_func()


if __name__ == '__name__':
    unittest.main()

  ```

  > setUpClass...
  >
  > 创建search对象
  >
  > 创建search对象
  >
  > 创建search对象
  >
  > tearDownClass...

  ## **4、断言基本方法**

  ![image-20231017145702642](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017145702642.png)

  断言方法格式：

  ​	当断言成功时，输出msg信息。断言失败时，输出报错信息

  ```py
断言成功：
def test_search1(self):
    print("断言相等")
    self.assertEqual(1, 1, msg=None) # msg可以省略
    
断言失败：
def test_search1(self):
    print("断言相等")
    self.assertEqual(1, 2, msg=None)
  ```

  > setUpClass...
  >
  > 断言相等
  >
  > tearDownClass...
  >
  > \-----------------------
  >
  > FAILED (failures=1)
  >
  > 2 != 1
  >
  > 预期:1
  >
  > 实际:2

  ```py
self.assertEqual(self.a, self.b, "断言参数a==b")
self.assertNotEqual(self.a, self.b, "断言参数a!=b")

self.assertIn(self.str1, self.str2, "断言参数str1==str2")
self.assertNotIn(self.str1, self.str2, "断言参数str1!=str2")

self.assertTrue(self.result, "断言参数result==True")
self.assertFalse(self.result, "断言参数result==False")

self.assertIs(obj1, obj2, "断言参数obj1和obj2为同一个对象")
self.assertIsNot(obj1, obj2, "断言参数obj1和obj2不为同一个对象")

self.assertNone(self.c, "断言参数c为空")
self.assertNotNone(self.c, "断言参数c不为空")

self.assertIsInstance(searchs, Search, "断言参数search为Search类的实例")
self.assertNotIsInstance(searchs, Search, "断言参数search不为Search类的实例")
  ```

  

  ## **5、unittest执行测试用例**

  ### **1.编写测试用例原则**

  - unittest会自动识别以test开头的函数的测试用例
  - 测试用例的执行顺序是以test后面的字母顺序执行的。如先执行test_a，test_b，test_c

  ### 2.执行测试用例的方法

  - 方法1：unittest.main()

  ```py
if __name__ == '__main__':
    unittest.main()
  ```

  - 方法2：加入容器分别执行

  ![image-20231017145826742](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017145826742.png)

  ```py
if __name__ == '__main__':
    suite=unittest.TestSuite() # 使用unittest.TestSuite()方法创建一个容器对象suite
    
    suite.addTest(TestClass("test_01")) # 使用容器对象addTest方法将测试类TestClass的测试方法test_01添加到容器当中，一次只能添加一条
    
    unittest.TextTestRunner().run(suite) # 使用TextTestRunner().run(容器名)运行整个容器内的测试用例
  ```

  - 方法3：指定某个测试类执行用例

  ```py
suite1 = unittest.TestLoader().loadTestsFromTestCase(TestSearch)  # 加载某个测试类到容器suite1内
suite = unittest.TestSuite([suite1, suite2]) # 将两个容器集成到一个大容器内
unittest.TextTestRunner(verbosity=2).run(suite) # 集成执行大容器
  ```

  ### **3.ideal执行用例存在的问题**

  ![image-20231017145913349](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017145913349.png)

  我们可以使用终端控制台执行python文件：精准执行了一条添加在容器内的用例

  ![image-20231017145927826](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017145927826.png)

  # **三、Pytest框架**

  Pytest是什么？

  - pytest能够支持简单的单元测试和复杂的功能测试；
  - pytest可以结合Requests实现接口测试；
  - 结合Selenium、Appium实现自动化功能测试；
  - 使用pytest结合Allure集成到Jenkins中可以实现持续集成。
  - pytest支持315种以上的插件；

  ## **1、安装pytest及其插件**

  pytest生态是由pytest本身和pytest插件共同构成的

  - pytest:框架本体
  - pytest-html:生成HTML测试报告
  - pytest-xdist:并行话执行测试用例
  - pytest-rerunfailures:失败重跑
  - pytest-ordering:为用例排序
  - allure-pytest:生成allure测试报告

  ### **1.安装pytest**

  pip是Python包管理工具,使用pip工具进行安装

  ```py
pip install pytest 
  ```

  ### **2.安装pytest插件**

  ```py
pip install pytest pytest-html pytest-xdist pytest-rerunfailures pytest-ordering allure-pytest 
  ```

  ## **2、Pytest规范**

  ### 1.pytest用例规范

    1. 文件：必须以test_ 开头或_test结尾
    2. 类：以Test开头或者继承unittest.TestCase的任意名称的类
    3. 方法：以test_开头的方法
    4. 注意：测试类中不可添加__init()__构造函数，pytest不识别含__init()__的类内的测试方法

  ![image-20231017150111306](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017150111306.png)

  ### **2.pytest输出规范**

  ![image-20231017150121912](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017150121912.png)

  Text测试报告-》HTML测试报告

    1. 报告头
       1. 平台和版本信息
       2. 根目录、配置文件、特殊选项
       3. 插件列表
    2. 收集情况
       1. 测试用例的数量
    3. 执行状态
       1. 用例的执行结果
       2. 测试执行进度

  ## **3、python运行pytest**

  - pthon解释器

  - - 使用python test_*文件名.py 执行方法当中的pytest.main()
    - 使用python -m pytest test_*文件名.py方式执行pytest.main()

  - 使用pytest方法执行p

  对于一个简单的测试用例

  ```py
def inc(x):
    return x + 1

def test_anwer():
    assert inc(4)
  ```

  ### **1.使用pytest执行**

  ```py
pytest test_sample.py
  ```

  ![image-20231017150452054](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017150452054.png)

  ### **2.使用python解释器执行**

  ```py
方法1：python test_*文件名.py
方法2：python -m pytest test_*文件名.py
  ```

  在使用这两个命令之前，我们先来熟悉一下pytest.main(）函数及其模块

  使用python解释器执行文件，系统会以main()函数作为入口执行代码程序，调用pytest.main()执行所有的满足pytest规则的方法。

  为了确保测试用例只在本模块调用执行，我们需将pytest.main()方法放在`if __name__ == '__main__':`模块内

  pytest.main()也可以传入命令行参数达到在终端使用pytest命令行一样的效果,如下：

  ```py
if __name__ == '__main__':  # 只在本模块运行
    pytest.main()  # 1、直接调用，运行当前目录下所有满足pytest条件的用例
    pytest.main(['test_command_param.py::test_function', '-vs'])  # 2、运行子模块下的方法，并使用标签，每个值需要用引号隔开 相当于 pytest test_command_param.py::test_function -vs
    pytest.main(['test_command_param.py', '-vs', '-m', 'double'])  # 3、运行某个标签下的所有用例 相当于 pytest test_command_param.py -vs -m=double
  ```

  #### **2.1 使用python test_\*文件名.py**

  首先，在代码中添加一个模块，将命令和标签以列表的形式传到pytest.main(参数)内

  ```py
if __name__ == '__main__':  # 只在本模块运行
    pytest.main()  # 1、直接调用，运行当前目录下所有满足pytest条件的用例
    pytest.main(['test_command_param.py::test_function', '-vs'])  # 2、运行子模块下的方法，并使用标签，每个值需要用引号隔开
    pytest.main(['test_command_param.py', '-vs', '-m', 'double'])  # 3、运行某个标签下的所有用例
  ```

  调用python解释器

  ```py
python +文件名.py
  ```

  ![image-20231017162653356](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017162653356.png)

  #### **2.2 使用python -m pytest test_\*文件名.py**

  我们还可以使用以下命令去实现，效果等价于python + test_*.py文件名方式

  ```py
python -m pytest test_*.py
  ```

  ![image-20231017162729558](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017162729558.png)

  综上所述，实际执行用例环境是在python环境下，因此常用python解释器去执行测试用例，及python -m pytest test或python +文件名.py的方式，因为而不常用pytest框架的方式，使用python解释器方便指定版本。

  ### **3.pytest运行多条用例**

  ![image-20231017151200003](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151200003.png)

  - 执行包下面所有的用例：pytest
  - 执行单独一个pytest模块：pytest 文件名.py

  ![image-20231017151213129](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151213129.png)

  - 运行某个模块里面的某个类：pytest 文件名.py::类名 (-v显示细节)

  ![image-20231017151253495](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151253495.png)

  - 运行某个模块里面某个类的方法：pytest 文件名.py::类名::方法名（模块其实就是一个文件里面有很多类）

  ![image-20231017151318005](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151318005.png)

  > pytest常见提示关键字：
  >
  > PASSED :表示断言通过
  >
  > FAILED:表示断言失败
  >
  > ERROR:表示代码命名等不符合规范，且不能运行
  >
  > WARNING ：警告，不影响正常运行

  ## **4、setup和teardown**

  ![image-20231017151339816](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151339816.png)

  setup/teardown与setup_method/teardown_method功能相同，相当于一个简单的缩写。

  这里注意函数与方法的区别：函数（function）相对于方法（method）而言函数定义在类外，方法定义在类里面作为类独有的方法。这里setup_function和setup_method就是属于在函数或方法范围上的不同。

  ```py
def test_case1():
    print("case1")


def setup_function():
    print("资源准备：setUp function...")


def teardown_function():
    print("资源销毁：tearDown function...")

  ```

  运行结果：

  ![image-20231017151402844](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151402844.png)

  及最高级是setup_module与teardown_module（模块级）整个程序只前后运行一次，最低级别的是setup、teardown（方法级）每个方法前后调用。

  ```py
def test_case1():
    print("case1.")


def test_case2():
    print("case2.")


def setup_module():
    print("资源准备：setUp module...")


def teardown_module():
    print("资源准备：teardown module...")


def setup_function():
    print("资源准备：setUp function..")


def teardown_function():
    print("资源销毁：tearDown function..")

  ```

  运行结果：

  ![image-20231017151424908](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151424908.png)

  ## **5、pytest基本命令行参数**

  ### 更多筛选用例参数

  [Pytest脚本的运行_pytest怎么执行指定文件或目录-CSDN博客](https://blog.csdn.net/redrose2100/article/details/121782015)

  ![image-20231017151442319](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151442319.png)

  ```py
--help            查看帮助
-x                用例一旦失败（fail/error）,就立即停止执行。一般用在冒烟测试（单独测试几个核心的功能的测试，一旦发现错误，版本立即打回给开发）          
--maxfail=mun    允许失败的个数=mun,当失败个数超过num停止执行。相比较-x没有那么苛刻
-m                标记用例
-k                执行包含某个关键字的测试用例
-v                显示用例执行详细信息
-s                打印输出日志（-vs一块使用）
-collection-only  测试平台，pytest自动导入功能
-h 显示所有参数帮助 
-n X 使用X个进程，并行化执行用例(1核执行时间为10s，两核执行时间缩短一半5s)/-n auto 自动选择进程数执行用例（取决于你的电脑cpu）
--html=Path 生成HTML测试报告和一个样式文件，并保存在Path路径下/--html=Path --self-contained-html HTML文件自包含样式文件，只会生成一个HTML文件
reruns X 测试用例失败后，重新X次
  ```

  ### **1.-x 用例失败停止执行**

  用例一旦执行错误，立刻停止执行

  错误用例在最后一个：

  ![image-20231017151615669](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151615669.png)

  ![image-20231017151620040](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151620040.png)

  错误用例在第一个：

  ![image-20231017151627599](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151627599.png)

  ![image-20231017151632379](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151632379.png)

  ### **2.--maxfail=mun 设置允许失败的用例个数num**

  当我们使用--maxfail=mun命令设置最大允许失败的用例个数为2，我们把失败的用例放在第一条时，发现不再和-x一样直接退出，而是完整的执行完整个测试类，直到找到两个失败的用例才退出。

  ![image-20231017151647310](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151647310.png)

  ![image-20231017151651225](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151651225.png)

  而我们将失败次数改为1时，本质上和-x没区别，当第一条用例失败时直接退出

  ![image-20231017151702710](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151702710.png)

  ### **3.-m 标记用例**

  用例用mark分组后，使用-m标签选择指定组执行用例

  ```py
-m=double
-m double
-m "double"
-m 'double'
  ```

  ![image-20231017151723345](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151723345.png)

  ### **4.-k 执行方法名包含某个关键字的用例**

  ```py
pytest test_command_param.py -v --maxfail=3 -k "double"
  ```

  ![image-20231017151742540](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151742540.png)

  ![image-20231017151747093](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151747093.png)

  执行一个测试文件内的两个方法

  ```
pytest -k "test_method1 or test_method2"
# 这个命令将运行 test_method1 和 test_method2，无论它们位于哪个文件中。
  ```

  如果你的测试方法分布在不同的类中，你也可以在 `-k` 表达式中包含类名。例如：

  ```
pytest -k "TestClass1 and test_method1 or TestClass2 and test_method2"
# 这将运行 TestClass1 中的 test_method1 和 TestClass2 中的 test_method2
  ```

  

  ### **5.-v 输出测试详情**

  如果不使用-v，则用例成功用"."标识，失败用"F"表示，不方便阅读

  ![image-20231017151800262](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151800262.png)

  使用-v，会显示详细的用例方法是否通过

  ![image-20231017151808993](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151808993.png)

  ### **6.-s 开启内容输出**

  pytest框架采用终端的方法默认关闭测试用例的输出内容，使用-s会打开并输出内容到控制台，一般与-v一起使用用于调试代码

  ```py
pytest test_command_param.py -vs
  ```

  ![image-20231017151834276](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151834276.png)

  ![image-20231017151837983](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151837983.png)

  ### **7.--collected-only 只收集用例**

  ![image-20231017151851880](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151851880.png)

  ### **8.--help显示帮助**

  ```py
pytest --help
  ```

  ![image-20231017151908490](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151908490.png)

  我们还可以使用过滤符grep来选择目的标签查找帮助：

  ```py
pytest --help|findstr 标签 # windows的过滤关键字为findstr
pytest --help|grep 标签 # linux和mac系统
  ```

  使用help来查找mark帮助文档：

  ![image-20231017151927757](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151927757.png)

  ### **9.--lf(--last-failed) 只执行上次失败的用例**

  ![image-20231017151939820](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151939820.png)

  我们先执行一条失败的用例

  ![image-20231017151947954](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151947954.png)

  然后使用命令--lf -v,发现只执行了上次失败的那条用例test_double3

  ![image-20231017151956707](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017151956707.png)

  ### **10.--ff(--failed-first) 先执行上次失败的用例，再执行剩余的用例**

  ![image-20231017152006268](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152006268.png)

  ### **11.-n X 使用X个进程，并行化执行用例**

  (1核执行时间为10s，两核执行时间缩短一半5s),-n auto 自动选择进程数执行用例（取决于你的电脑cpu，四核n=4 8核n=8）

  [pytest多进程/多线程执行测试用例 - 网名余先生 - 博客园 (cnblogs.com)](https://www.cnblogs.com/micheryu/p/16441492.html)

![image-20231017152038446](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152038446.png)

  ### **12.--html=Path 生成HTML测试报告和一个样式文件**

  --html=Path 生成HTML测试报告和一个样式文件，并保存在Path路径下

![image-20231017152147810](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152147810.png)

  --html=Path --self-contained-html HTML文件自包含样式文件，只会生成一个HTML文件

  ![image-20231017152154903](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152154903.png)



  ## 6、pytest.ini配置化文件

  在执行测试用例时会先执行pytest.ini配置文件，我们可以把所有的命令都统一放在pytest.ini文件进行配置,当执行命令pytest时候，会先自动加载pytest.ini文件并执行里面的配置

  ```py
[pytest] 
addopts = -v -s -n 1 --html=report.html --self-contained-html 
  ```

  下次执行直接使用pytest命令，系统自动加载pytest.ini配置文件里的命令，无需在命令行中额外加命令

  ## **7、Mark标签**

  ![image-20231017152305625](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152305625.png)

  ### **1.Mark.标签名 设置标签组**

  首先我们要再测试用例上使用解释器定义mark标签

  ![image-20231017152317492](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152317492.png)

  然后，再使用-m标签进行标签选择执行，有三种选择方式都可

  ```py
-m=double
-m double
-m "double"
-m 'double'
  ```

  ![image-20231017152337581](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152337581.png)

  另外，以上都是我们自定义的一个标记，pytest识别不出会给我们抛出警告，并不影响运行

  ![image-20231017152344921](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152344921.png)

  我们可以在pytest.ini配置文件中配置标签名，这样pytest就能自动识别出我们的标签不会给警告了

  在pytest.ini配置标签名

  ![image-20231017152357788](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152357788.png)

  再执行用例发现不再报警告

  ![image-20231017152414446](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152414446.png)

  ### **2.自定义mark**

  mark首先在pytest.ini配置文件里面定义，才可以使用

  定义mark配置信息：将mark自定义分为三类：test1、test2、test3,

  即markers = 类型名 ： 类型注释

  ![image-20231017152433978](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152433978.png)

  你可以用mark标记不同的测试用例，通过 pytest -m 类型名 筛选需要执行哪类测试用例

  - 选择这类测试用例： pytest -m 类型名
  - 不选择这类测试用例： pytest "-m not" 类型名 由于not会当成类型名，因此我们要用” “包裹 

  ![image-20231017152505665](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152505665.png)

  执行标签用例

  ![image-20231017152515087](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152515087.png)

  ### **3.Mark.skip 跳过测试用例**

  ![image-20231017152626079](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152626079.png)

  使用场景

  ![image-20231017152632970](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152632970.png)

  ```py
方法1：添加装饰器：
    @pytest.mark.skip 为方法级别的跳过，也可以用作类级别的跳过
    @pytest.mark.skipif 为方法级别的跳过，也可以用作类级别的跳过
方法2：代码中添加跳过后面的代码
    pytest.skip(reason)
  ```

  #### **3.1 skip跳过**

  ```py
@pytest.mark.skip(reason="代码没有实现") # 在用例前面添加skip装饰器，reason上输出跳过提示
def test_zero():
    assert 0 == func(0)
  ```

  ![image-20231017152713637](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152713637.png)

  #### **3.2 skipif代码跳过**

  ```py
# skipif的用法
@pytest.mark.skipif(条件表达式, reason="")
# 当条件表达式返回True时，会跳过用例

@pytest.mark.skipif(1 == 1,reason='满足条件跳过此用例') # 跳过用例并输出reason信息
def test_pytest_1():
    print('case4')

@pytest.mark.skipif(1 == 2,reason='不满足条件不会跳过此用例')
def test_pytest_2():
    print('case5')
  ```

  实例，当平台操作系统为win，且python版本高于3.6时跳过用例

  ```py
print(sys.platform)  # 输出操作系统平台 win为windows,darwin为mac


# 判断如果为win操作系统，跳过该用例并输出信息
@pytest.mark.skipif(sys.platform == 'win32', reason="does not run windows")
def test_case1():
    assert True


# 判断如果为mac操作系统，跳过该用例并输出信息
@pytest.mark.skipif(sys.platform == 'darwin', reason="does not run mac")
def test_case2():
    assert True


# 判断python版本信息小于3.6，跳过用例给出信息
@pytest.mark.skipif(sys.version_info < (3, 6), reason="requires python3.6 or lower")
def test_case3():
    assert True

  ```

  ![image-20231017152748253](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152748253.png)

  #### **3.3 skip代码内判断跳过**

  创建一个检查登录方法，和一个测试方法

  ```py
def check_login():  # 检查登录方法
    return False


def test_function():  # 测试方法
    print("start...")

    if not check_login():  # 如果登录不成功，跳过后续步骤，并返回reason信息
        pytest.skip("登录未成功...") # 用代码实现步骤跳过

    print("end...")
  ```

  执行测试用例：当if判断错误时调用pytest.skip跳过后续代码，并输出跳过reason

  ![image-20231017152816084](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152816084.png)

  ### **4.Mark.xfail  标签**

  期望测试用例为失败并标记，失败不会影响其他测试用例执行

  ![image-20231017152832940](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152832940.png)

  xfail相比较于skip来说，虽然使用的格式类似，但是xfail不会跳过用例而是执行测试用例，成功输出xpass，失败输出xfail

  xfail相比于skip，reason任何时候都会输出

  ```py
# 用法1：加上装饰器@pytest.mark.xfail
@pytest.mark.xfail(reason="bug case1") 
def test_case1():
    print("test_xfail1方法执行...")
    assert 1 == 2

# 我们也可以定义xfail变量为pytest.mark.xfail，下次调用直接使用@xfail便捷调用
xfail = pytest.mark.xfail

@xfail(reason="bug case2")
def test_case2():
    print("test_xfail方法执行...")
    assert 1 == 1
  ```

  ![image-20231017152846960](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152846960.png)

  xfail内部代码跳过,与skip跳过不同的是，xfail没有判断条件，执行完xfail无论如何都会跳过后面的代码

  ```py
def test_xfail():
    print("*****开始测试*****")
    pytest.xfail(reason="该功能尚未完成") # xfail在代码内会暂停继续执行下面的代码
    print("测试过程")
    assert 1 == 1
  ```

  ![image-20231017152905636](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152905636.png)

  ## **7、Fixture**

  fixture是pytest中特殊的一部分，也是必须掌握的核心用法

  fixture的作用是： 使测试用例执行更加可靠，结果也更加稳定 可以称之为软件测试的装置、夹具、脚手架，作用和目的： 将多个用例里面的重复部分提取到fixture方法内 ，因此很适合用来做自动化准备测试环境，参数化多个测试用例的数据

  作用：

    1. 在测试用例执行之前，自动化准备相关的测试环境
    
    2. 在测试执行之后，将相关内容进行销毁

  ### **1.fixture案例**

  案例：断言"百度"，"阿里"，"腾讯"网站标题是否包含关键字

  ![image-20231017152939311](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152939311.png)

  访问baidu.com,标题中应该有"百度"

  - 1. 打开浏览器
    2. 输入https://baidu.com
    3. 获取标题
    4. 断言"百度"出现在标题当中

  ```py
""" -打开浏览器 -输入https://baidu.com -获取标题 -断言“百度”出现在标题中 :return: """


def test_baidu():
    # 自动安装webdriver
    _path = ChromeDriverManager(url="https://npm.taobao.org/mirrors/chromedriver").install()
    # 启动浏览器
    driver = webdriver.Chrome(service=Service(_path))
    # 输入网址
    driver.get("https://baidu.com")
    # 获取标题
    title = driver.title
    # 断言标题
    assert "百度" in title


def test_aliyun():
    # 自动安装webdriver
    _path = ChromeDriverManager(url="https://npm.taobao.org/mirrors/chromedriver").install()
    # 启动浏览器
    driver = webdriver.Chrome(service=Service(_path))
    # 输入网址
    driver.get("https://aliyun.com")
    # 获取标题
    title = driver.title
    # 断言标题
    assert "阿里" in title


def test_qq():
    # 自动安装webdriver
    _path = ChromeDriverManager(url="https://npm.taobao.org/mirrors/chromedriver").install()
    # 启动浏览器
    driver = webdriver.Chrome(service=Service(_path))
    # 输入网址
    driver.get("https://qq.com")  # 获取标题
    title = driver.title  # 断言标题
    assert "腾讯" in title

  ```

  由于我们写的案例目的是断言网站标题含有目的文字，而打开浏览器等操作不属于我们设计用例的一部分，属于测试用例的准备工作。

  - 对于 测试用例的准备工作 ，挪出测试用例代码
  - 对于重复性的代码， 使用函数进行重用
  - 这样的工作应该交给 fixture 完成

  我们可以使用装饰器  @pytest.fixture  创建一个fixture方法

  ```py
import pytest
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager

@pytest.fixture  # 定义一个fixture夹具名driver 
def driver():
    _path = ChromeDriverManager().install()  # 自动安装webdriver 
    return webdriver.Chrome(service=Service(_path))  # 启动浏览器 

def test_baidu(driver):  # 将fixture夹具名传入测试用例 
    driver.get("https://baidu.com")
    title = driver.title
    assert "百度" in title

def test_aliyun(driver):  # 将fixture方法传入函数 
    driver.get("https://aliyun.com")
    title = driver.title
    assert "阿里" in title

def test_qq(driver):  # 将fixture方法传入函数 
    driver.get("https://qq.com")
    title = driver.title
    assert "腾讯" in title
  ```

  以上的网站都是打开之后关闭再打开，所有的用例之间fixture不能共用，我们可以创建一个fixture完成浏览器启动的共用。

  ### **2.fixture范围**

  我们可以在创建fixture夹具装饰器的时候添加一个范围

  ```py
@pytest.fixture(scope='session') # fixture范围为一个会话 
  ```

  fixture范围： 

    1. function:同一个函数中的测试用例**（最小/默认值）**
    
    2. class:同一个类中的测试用例
    
    3. module:同一个模块（文件）中的测试用例
    
    4. package:同一个包（文件夹）中的测试用例
    
    5. session:整个测试活动**（最大）**

  ### **3.内置fixture**

  ![image-20231017153048132](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017153048132.png)

  ## **8、configtest初始化py文件**

  pytest启动时会自动加载两个文件：pytest.ini配置文件、configtest.py python文件

  configtest.py 有两个特点：

  比测试用例先执行

  在这里 定义fixtures可以在任何文件任何测试用例中执行

  还有什么可以放在configtest.py文件内呢？

  - setup/teardown
  - 常量
  - 初始化
  - pytest的配置
  - pytest的插件

  ## **9、参数化**和数据驱动

  在实现参数化之前我们先来了解一下什么是`数据驱动`

  ==数据驱动就是数据的改变从而驱动自动化测试的执行，最终引起测试结果的改变。==

  简单来说，就是参数化的应用，数据量小的测试用例可以使用代码的参数化来实现数据驱动，数据量大的情况可以使用结构化文件（cvs、yaml、json等）对数据进行存储和在测试用例中进行读取。

  `CSV（逗号分隔值）`文件是一种简单的文本格式，用于存储表格数据，如数字和文本。CSV文件由纯文本组成，每行一个数据记录。每个记录由字段组成，字段之间通常由逗号分隔。这种格式通常用于表示表格数据（比如数据库、电子表格）。

  一个典型的CSV文件可能看起来是这样的：

  ```
Copy codeColumn1,Column2,Column3
Data1,Data2,Data3
Data4,Data5,Data6
  ```

  在这个示例中：

  - 第一行是标题行，包含每列的名称。
  - 随后的每一行代表一条记录。
  - 每一行中的每个值（字段）用逗号分隔。

  CSV文件的关键特性包括：

  - **简单性**：它们是纯文本文件，可以由任何文本编辑器查看和编辑。
  - **通用性**：几乎所有的表格处理软件都能识别和处理CSV文件，例如Microsoft Excel、Google Sheets和各种编程语言的库。
  - **灵活性**：尽管“CSV”代表逗号分隔值，但实际上可以使用其他字符作为分隔符，如制表符（生成所谓的TSV文件）或分号。

  根据您的应用程序和地区设置，CSV文件的确切格式可能略有不同。例如，在一些欧洲地区，分号（;）而不是逗号（,）用作字段分隔符，因为逗号已用作小数点。

  

  #### CSV文件格式

  **优点**:

    1. **简单性和可读性**: CSV文件结构简单，易于理解和编辑，即使是非技术人员也能轻松使用。
    2. **广泛支持**: 几乎所有编程语言和数据处理工具都支持CSV格式，无需特殊的解析库。
    3. **高效处理大量数据**: 对于大量的行式数据，CSV可以高效处理。

  **缺点**:

    1. **结构限制**: CSV不适合复杂的数据结构。它难以表示层次化或嵌套的数据。
    2. **缺乏类型信息**: CSV中的所有数据都是文本格式，需要在解析时进行类型转换。
    3. **标准不一致**: CSV的格式（如分隔符）可能因地区和用户而异，这可能导致解析问题。

  #### JSON文件格式

  **优点**:

    1. **灵活的数据结构**: JSON能够轻松表示嵌套或层次化的数据结构，适合复杂的数据需求。
    2. **数据类型识别**: JSON支持基本的数据类型（如数字、字符串、布尔值），因此数据类型在解析时保持不变。
    3. **易于解析**: 许多编程语言提供了原生的JSON解析支持，使得读取和写入JSON数据变得简单。

  **缺点**:

    1. **文件大小**: 对于同样的数据，JSON文件通常比CSV文件大，因为它包含更多的格式化文本（如键名和括号）。
    2. **解析成本**: 解析JSON通常比解析CSV更耗费资源，尤其是在大文件的情况下。
    3. **可读性**: 对于非技术人员来说，复杂的JSON结构可能不如CSV那样直观易读。

  而参数化的设计方法就是将模型中的定量信息变量化，使之成为任意调整的参数。对于变量化参数赋予不同的数值，就可以得到不同的模型。

  参数化可以分为mark标签参数化和fixture参数化

  ### **1.mark参数化**

  mark参数化一般是使用mark.parametrize()进行传参

  - 单参数
  - 多参数
  - 用例重命名
  - 笛卡尔积

  ```py
@pytest.mark.parametrize('参数1，参数2...', [列表1/元组1，列表2/元组2...]) # 参数与数值要一一对应,如果需要传递多个参数，要放在列表当中嵌套列表/元组
def func(参数1，参数2...) # 形参与参数化的参数要一一对应
  ```

  #### **1.1 单参数化**

  由于search_list是一个列表有三个单参数，因此会生成三条用例

  ![image-20231017153208137](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017153208137.png)

  #### **1.2 多参数化**

  多参数化相对于单参数，解释器参数传入多个，值需要传入`列表-列表`嵌套或`列表-元组`嵌套

  ```py
@pytest.mark.parametrize("test_input,expected", [("3+5", 8), ("2+5", 7), ("7+5", 12)]) # 传多个参数，数值里列表需要再嵌套列表/元组
def test_mark_more(test_input, expected): # 形参与参数一一对应，参数与列表值一一对应
    assert eval(test_input) == expected  # eval()方法，将字符串转换成无字符串的表达式，如"3+5"转化成3+5并执行
  ```

  多参数化mark.parametrize(参数...,值),格式比较宽松

  ```py
@pytest.mark.parametrize("test_input,expected", [("3+5", 8), ("2+5", 7), ("7+5", 12)]) # 使用一个字符串包裹多个参数，值用列表-元组嵌套
@pytest.mark.parametrize("test_input","expected", [("3+5", 8), ("2+5", 7), ("7+5", 12)]) # 使用多个字符串表示参数，值用列表-元组嵌套
@pytest.mark.parametrize("test_input,expected", [["3+5", 8], ["2+5", 7], ["7+5", 12]]) # 使用一个字符串包裹多个参数，值用列表-列表嵌套
  ```

  

  ![image-20231017153247384](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017153247384.png)

  #### **1.3 参数重命名**

  ![image-20231017153258360](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017153258360.png)

  我们运行以上例子时，发现参数化的默认测试用例命名是以参数的值来命名的，不具有理解性，我们可以使用ids对参数化的用例进行命名，其中ids个数与传递的数据个数要完全一致

  ```py
@pytest.mark.parametrize("test_input,expected", [("3+5", 8), ("2+5", 7), ("7+5", 12)],
                         ids=["number1", "number2", "number3"])
def test_mark_more(test_input, expected):
    assert eval(test_input) == expected  # eval()方法，将字符串转换成无字符串的表达式，如"3+5"转化成3+5并执行
  ```

  命名之后运行

  ![image-20231017153317595](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017153317595.png)

  ![image-20231017153323715](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017153323715.png)

  #### **1.4 笛卡尔积**

  两个解释器参数化一起出现在一个用例上时，系统将对其进行笛卡尔积传递参数

  ![image-20231017153337464](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017153337464.png)

  ```py
@pytest.mark.parametrize('wd', ['appium', 'selenium', 'pytest'])
@pytest.mark.parametrize('code', ['utf-8', 'gbk', 'gb2312'])
def test_dker(wd, code):
    print(f"wd:{wd},code:{code}")
  ```

  ![image-20231017153348686](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017153348686.png)

  ### **2.fixture实现参数化**

  测试代码的共享我们采用数据驱动的方式实现

  `数据驱动=数据局管理+参数化测试`

  接着以上的案例，我们可以发现代码重复很严重

  ![image-20231017153408915](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017153408915.png)

  参数化测试： 一份代码，传递参数执行多个用例

  fixture支持参数化，于是可以进行参数化测试

  ```py
@pytest.fixture(
    params=[("https://baidu.com", "百度"),
            ("https://aliyun.com", "阿里"),
            ("https://qq.com", "腾讯")])
def test_data(request):
    return request.param  # 将参数化的参数返回给测试用例 


def test_ceshi(driver, test_data):
    url, tit = test_data  # 拆包 
    driver.get(url)
    title = driver.title
    assert tit in title
  ```

  创建一个夹具test_data用于返回参数param，将参数化数据存放在fixture关键字params内，夹具test_data使用request关键字接收

  ![image-20231017153429354](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017153429354.png)

  测试类使用fixture传入参数，当fixture的params为多参数时，需要对传入参数进行拆包；

  ### 3.fixture传参和mark传参的区别

  1.fixture是在fixture方法内直接传参，而mark是使用mark.parametrize关键字传参

  2.fixture是一个全局方法，其他测试方法只需要调用fixture就可以使用参数化；而mark标签只定义在测试方法上，只能对某一测试用例参数化

  3.多参数传值可以使用元组或列表，使用fixture传值需要拆包

  ![image-20231017163857220](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017163857220.png)

  

  ### 4.yaml数据参数化

  #### 4.1 yaml实现list

  ```py
list
    - 10
    - 20
    - 30
  ```

  #### 4.2 yaml实现dict

  ```py
dict
	by:id
    locator:name
    action:click
  ```

  #### 4.3 yaml嵌套

  ```py
列表字典嵌套：
-
	- by:id
    - locator:name
    -action:click
    
对象列表字典嵌套：对象companiens：[{id:1,name:company1,price:200w},{id:2,name:company2,price:500w}]
companiens
	-
    	id:1
        name:company1
        price:200w
    -
    	id:2
        name:company2
        price:500w
  ```

  #### 4.4 加载yaml文件

  ```py
# yaml文件加载
yaml.safe_load(open("./data.yaml"))

# pytest与yaml连用
@pytest.mark.parametrize(["a", "b"], yaml.safe_load(open("./data.yaml")))
def test_param(self, a, b):
    print(a + b)
  ```

  #### 4.5 导入yaml文件参数化传递数据

  首先，我们先下载yaml插件

  ```
pip install pyyaml
  ```

  也可以在python包中直接导入

  ![image-20231017173349113](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017173349113.png)

  在项目里创建一个`.yaml`文件，使用yaml语法在文件中写好数据

  ![image-20231017173455318](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017173455318.png)

  然后在测试类当中传入yaml文件路径,加载yaml文件

  ```py
import pytest
import yaml

@pytest.mark.parametrize(["a", "b"], yaml.safe_load(open("./data.yaml"))) # 传入yaml文件路径并加载
def test_param(a, b):
    print(a + b)
  ```

  #### 4.6 yaml数据驱动案例

  首先创建一个`env.yml`文件，内容为dict类型如下：

  ```
test: 127.0.0.1
  ```

  在用例中我们引入`env.yml`文件判断文件内容

  ```py
@pytest.mark.parametrize("env", yaml.safe_load(open("env.yml")))
def test_yaml(env):
    if "test" in env:
        print("这是测试环境")
    elif "dev" in env:
        print("这是开发环境")
    print(env)  # 只输出key值 test
  ```

  > 输出结果：这是测试环境
  > 		test

  输出`env.yml`全部内容

  ```py
def test_case1(self):
    print(yaml.safe_load(open("env.yml"))) # test: 127.0.0.1
  ```

  以上代码只读取到了key值test,而使用`print(yaml.safe_load(open("env.yml")))`打印输出的却是全部值，由此发现我们使用一个参数`env`读取dict类型yml文件时`@pytest.mark.parametrize("env", yaml.safe_load(open("env.yml")))`只能读取到第一个key值。

  为了让我们能够读取dict类型的yml文件，我们可以在yml文件中使用列表包裹key-value，如下

  ```yaml
-
  test: 127.0.0.1
  ```

  这个时候执行以上代码，就可以输出整个key-value数据

  > 输出：
  >
  > ​	{'test': '127.0.0.1'}

  我们对这个代码进行升级，采用列表嵌套的方式打印value值

  ```py
@pytest.mark.parametrize("env", yaml.safe_load(open("env.yml")))
def test_yaml(env):
    if "test" in env:
        print("这是测试环境")
        print("测试环境的ip是：" + env["test"])
    elif "dev" in env:
        print("这是开发环境")
        print("开发环境的ip是：" + env["dev"])
  ```

  > 输出结果：
  >
  > 测试环境的ip是：127.0.0.1

  ## **10、request 内置fixture**

  **request**：获取当前测试用例的相关信息

  - request.node (当前测试用例名称)
  - request.fixturenames  (当前测试用例使用的fixture名称)
  - request.config.getoption("htmlpath") 生成的html测试报告

  ![image-20231017153515005](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017153515005.png)

  ## **11、pytest处理异常**

  ![image-20231017153524356](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017153524356.png)

  ### **1.try...except捕获异常**

  ![image-20231017153531768](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017153531768.png)

  ![image-20231017153541007](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017153541007.png)

  ### **2.pytest.raise()**

  pytest异常处理框架pytest.raise()底层还是封装了python的try...except方法

  ![image-20231017153549564](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017153549564.png)

  ![image-20231017153554629](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017153554629.png)

  ![image-20231017153558192](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017153558192.png)

  ## 12、pytest常见报错

  ### 1.pytest显示空套件

  ==AttributeError: module ‘allure‘ has no attribute ‘severity_level‘==

  卸载allure-pytest

  ```
 pip uninstall pytest-allure-adaptor
  ```

  重新下载pytest-allure

  ```
pip insatll allure-pytest
  ```

  # 四、Allure测试框架

  - allure是一个轻量级、灵活的、支持多语言的测试报告工具
  - 多平台兼容的，奢华的report框架
  - 可以为dev/qa提供详尽的测试报告、测试步骤、log等
  - 为管理层提供高水平的统计报告
  - java语言开发的，支持pytest、javascript、PHP、ruby等
  - 可以集成到jenkins

  allure官方文档汉化：[Allure使用教程 - 官方文档汉化 - 搬砖路上的大马猴 - 博客园 (cnblogs.com)](https://www.cnblogs.com/struggleMan/p/17519562.html)
  		allure官网：[Allure Report](https://allurereport.org/)

  ## 1、allure基本配置

  ### 1.allure下载

  windows allure下载地址：[Central Repository: io/qameta/allure/allure-commandline (apache.org)](https://repo.maven.apache.org/maven2/io/qameta/allure/allure-commandline/)

  ### 2.allure环境变量

  安装完allure后需要将allure的bin目录配置在环境变量

  ### 3.安装allure-pytest插件

  ```
pip install allure-pytest
  ```

  

  ## 2、allure常用特性

  

  allure常用特性包括：@Feature、@story、@step、@attach

  步骤：

  - import allure
  - 功能上加@allure.feature('功能名称')
  - 子功能上加@allure.story('子功能名称')
  - 步骤上加@allure.step('步骤细节')
  - @allure.attch('具体文本信息')，需要附加信息：可以是数据、文本、图片、网页等等
  - 执行pytest可以对allure加feature/stories限制条件进行过滤

  ```py
pytest 文件名 --allure-feature="大模块" / --allure-stories="小模块" 注意这里的模块名只能用双引号
  ```

  ### 1.模块标记

  #### 1.1 Feature 大模块

  feature相当于一个大的功能模块（一般用来定义类），对应报告的testsuit

  ```py
@allure.feature("登录模块")
class TestLogin:
    ...
  ```

  #### 1.2 story 模块

  相当于feature的子模块，对应报告中的testcase (与feature为父子关系)

  ```py
 @allure.story("登录成功")
    def test_login_success(self):
        pass
  ```

  ```python
import allure
import pytest


@allure.feature("登录模块")
class TestLogin:
    # @allure.title("登录功能")
    @allure.story("登录成功")
    def test_login_success(self):
        with allure.step("步骤1：打开页面"):
            print("打开页面")
        with allure.step("步骤2：登录页面"):
            print("页面登录")
        with allure.step("步骤3：登录密码"):
            print("输入密码")
        assert 1 == 1
        print("用户登录成功！")

    @allure.story("登录失败")
    def test_login_failure(self):
        with allure.step("步骤1：打开页面"):
            print("打开页面")
        with allure.step("步骤2：登录页面"):
            print("页面登录")
        with allure.step("步骤3：输入密码"):
            print("输入密码")
        assert "1" == 1
        print("页面登录失败！")


@allure.feature("搜索模块")
class TestSearch:

    @allure.story("搜索成功")
    def test_login_success(self):
        with allure.step("步骤1：打开页面"):
            print("打开页面")
        with allure.step("步骤2：搜索信息"):
            print("页面搜索")
        assert 1 == 1
        print("搜索信息成功展示！")

    @allure.story("搜索失败")
    def test_login_failure(self):
        with allure.step("步骤1：打开页面"):
            print("打开页面")
        with allure.step("步骤2：搜索信息"):
            print("页面搜索")
        assert "1" == 1
        print("页面登录失败！")
  ```

  往后我们可能涉及很多模块，我们可以使用`--allure-feature="模块名"` / `--allure-stories="模块名"`对模块进行过滤执行。

  我们可以使用`pytest --help| findstr allure`帮助命令 查看命令格式

  ![image-20231017210715119](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017210715119.png)

  - allure模块过滤执行

  ```py
pytest test_allure.py --allure-stories="登录成功" -vs # 选择的stories="登录成功"的子模块执行
pytest test_allure.py --allure-features="登录模块" -vs # 选择的features="登录模块"的模块执行
  ```

  ![image-20231017210358194](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017210358194.png)

  ![image-20231017210814004](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017210814004.png)

  

  ![image-20231018154530282](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231018154530282.png)

  ![image-20231018111148773](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231018111148773.png)

  ### 2.step标识步骤

  - 测试过程的步骤，一般放在具体逻辑方法当中
  - 可以放在关键步骤中显示，在报告中显示对应的步骤

  ```py
@allure.step(步骤名) # 一般放在具体逻辑方法当中

with allure.step(步骤名) # 放在关键步骤中显示
  ```

  ![image-20231018160245356](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231018160245356.png)

  

  ### 3.testcase 添加超链接

  在测试用例上关联链接,可以通过链接关联多个用例

  ```py
import allure

TEST_CASE = "https://www.baidu.com"

@allure.testcase(TEST_CASE, "这是一个链接测试")
def test_with_testcase_link():

    pass

  ```

  ![image-20231018114906735](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231018114906735.png)

  ### 4. severity 定义优先级

  用例标记优先级的方法：

  - 通过pytest.mark标记优先级
  - 通过allure.feature allure.story标记
  - 通过`allure.severity`标记

  @allure.severity装饰器可以标记在类和方法上，级别类型有五种

  ```
Blocker：	中断级别（客户端无法响应，无法执行下一步）
Critical:	 临界缺陷（功能点缺失）
Normal：		普通缺陷（数值计算错误）
Minor:		 次要缺陷（界面UI错误）
Trivial：	轻微缺陷（输入无提示，提示不规范等）
  ```

  ```py
在方法，函数和类上加：
	@allure.severity(@allure.severity_level.TRIVIAL) # 标记为轻微缺陷
执行时：
	pytest-sv 文件名 --allure-severites normal,critical # 可以根据级别筛选执行用例
  ```

  当使用`--allure-severites`筛选执行用例时，如果一个类优先级被选中，那么这个类内所有相同优先级的方法+未定义优先级的方法将被执行。

  ### 5. title 添加标题

  title只能标识测试方法和函数，作用是在测试报告Suites上用title信息替换抽象的类和方法名，使得测试报告更好理解。

  ```py
@allure.title("title:登录模块")
@allure.feature("登录模块")
class TestLogin:
    # @allure.title("登录功能")
    @allure.title("title:登录成功")
    @allure.story("登录成功")
    def test_login_success(self):
        with allure.step("步骤1：打开页面"):
            print("打开页面")
        with allure.step("步骤2：登录页面"):
            print("页面登录")
        with allure.step("步骤3：登录密码"):
            print("输入密码")
        assert 1 == 1
        print("用户登录成功！")

    @allure.title("title:登录失败")
    @allure.story("登录失败")
    def test_login_failure(self):
        with allure.step("步骤1：打开页面"):
            print("打开页面")
        with allure.step("步骤2：登录页面"):
            print("页面登录")
        with allure.step("步骤3：输入密码"):
            print("输入密码")
        assert "1" == 1
        print("页面登录失败！")
  ```

  ![image-20231018160720425](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231018160720425.png)

  ### 6.attach方法插入文本、图片、视频

  ```py
import allure


def test_attach_text():
    allure.attach("这是一个纯文本", attachment_type=allure.attachment_type.TEXT)


def test_attach_html():
    allure.attach("<body>这是一段html body块</body>", name="html测试块", attachment_type=allure.attachment_type.HTML)


def test_attach_photo():
    allure.attach.file("D:\笔记图片\pytest参数化\image-20231018161426190.png", name="这是一个图片",attachment_type=allure.attachment_type.PNG)


def test_attach_video():
    allure.attach.file("./video路径.mp4", name="这是一个视频",attachment_type=allure.attachment_type.MP4)
  ```

  ![image-20231018162925112](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231018162925112.png)

  ![image-20231018162943685](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231018162943685.png)

  ## 3、allure生成测试报告并查看

  首先我们需要了解一下，allure有两种测试报告文件：含json文件的测试报告、含html和cess样式的测试报告。两种文件都是以文件夹的形式生成的。如下：

  ![image-20231018145756722](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231018145756722.png)

  ### 1.使用浏览器查看本地测试报告

  #### 1.1 alluredir生成测试报告

  首先使用命令`--alluredir`指定本地路径生成json格式测试报告

  ```py
pytest test_*.py -vs --alluredir=<路径/文件名> # allure会在指定路径下创建一个同名文件夹
pytest allure_case.py -vs --alluredir=./report
  ```

  如图：

  ![image-20231018142811666](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231018142811666.png)

  生成的文件夹内的文件是一些`json/txt格式的文件`

  #### 1.2 serve打开测试报告

  方法1：打开浏览器在线查看测试报告

  ```python
allure serve <json格式测试报告路径> # 指定json格式的测试报告的路径，allure会通过json文件自动生成美化页面
  ```

  ![image-20231018143604450](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231018143604450.png)

  ### 2.使用tomcat生成并查看测试报告

  #### 2.1 generate生成测试报告

  生成html,css样式测试报告文件，需要json格式测试报告作为输入，再使用`generate命令`在本地生成html与css样式文件

  ```py
allure test_*.py --alluredir=<含json文件生成路径> # 生成含json格式测试报告
allure generate <路径/文件名> -o <含html美化测试报告生成路径># 输入json格式测试报告，使用-o标签在指定路径输出html,css样式文件 不使用-o默认在当前路径生成allure-report测试报告
例：allure generate report -o result1           
  ```

  我们也可以使用-o 标签来指定生成的allure-report文件名和路径

  ```py
allure generate 本地文件路径 -o html文件路径   
allure generate report -o result1

  ```

  将两个报告数据文件合并成一个可视化的html测试框架文件

  ```py
allure generate .\results1\ .\results2\ -o .\results  # -o 标签指定生成的测试报告路径和文件名称
  ```

  ![image-20231018150600624](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231018150600624.png)

  点击index.html即可查看测试报告

  当你在一个项目里面生成第一个测试报告report，后面其他的用例生成测试报告指定相同的report路径，就可以把用例报告追加写入到report文件中

  当然我们也可以使用`--clean-alluredir`命令覆盖之前的测试报告

  ```py
pytest test_case.py --alluredir ./result2 --clean-alluredir # ./result2为本地测试报告路径
  ```

  #### 2.2 远程共享测试报告

  使用本机ip并指定端口号远程打开测试报告文件（必须是html.css样式文件）

  ```py
allure generate ./result1 -o ./result2 # 生成csshtml样式文件
allure open -h 127.0.0.1 -p 8883 ./result2 # 远程打开，本地ip127.0.0.1,端口号为8883
  ```

  ## 4、Allure生成两个测试报告并合并实例

  使用git下载好集合测试框架的代码sw_itest

  在终端内进入test_nvmedev.py上一级文件夹路径sw_itest\tests\pytestCase

  ```py
cd {filename}\sw_itest\tests\pytestCase  # filename为本地存放代码的路径
  ```

  使用python解释器运行test_blkdev.py文件，并指定路径生成测试报告result1,命令如下：

  ```py
python -m pytest test_blkdev.py -vs --alluredir ./results1 --clean-alluredir  # --clean-alluredir表示会覆盖历史生成的测试报告
  ```

  在本地生成的results1文件夹，内含json、txt类型的文件，不能直接打开查看测试报告，需要使用命令将json、txt文件内容整合到网页才能生成可视化的测试报告

  ![image-20231107170409515](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231107170409515.png)

  在网页端查看result1的测试报告，命令如下：

  ```py
allure serve .\results1\     
  ```

  生成网页端可视化测试报告：

  ![image-20231107170721038](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231107170721038.png)

  

  紧接着在终端运行test_nvmedev.py文件下的测试用例并生成测试报告results2，命令如下：

  ```py
python -m pytest test_nvmedev.py -vs --alluredir ./results2 --clean-alluredir
  ```

  生成的本地测试报告文件夹：

  ![image-20231107171134429](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231107171134429.png)

  在网页端查看result2的测试报告，命令如下：

  ```py
allure serve .\results2\  
  ```

  网页端测试报告：

  ![image-20231107171207006](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231107171207006.png)

  

  最后，使用generate命令将两个本地测试报告合并成一个含.html网页文件的测试报告文件夹results，该文件可以直接在本地使用浏览器查看，无需再使用serve命令生成，避免了重复劳动。命令如下：

  ```py
allure generate .\results1\ .\results2\ -o .\results  # -o 标签指定生成的测试报告路径和文件名称
  ```

  生成文件夹结构如下：

  ![image-20231107172059038](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231107172059038.png)

  我们直接在本地点开html文件使用浏览器打开，此时可以发现两个测试报告results1、results2内的信息都合并在一个测试报告当中，如下所示：

  ![image-20231107172403852](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231107172403852.png)

  ![image-20231107172343166](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231107172343166.png)

  当然我们也可以将两个文件生成的测试报告指定为一个本地文件夹，Allure会自动在原先的测试报告上追加数据使之整合成一份测试报告，如果你不希望追加报告，你可以使用标签去覆盖原有的报告文件

  ```py
pytest test_case.py --alluredir ./result --clean-alluredir  # 覆盖原有文件
  ```

  # 五、Robotframework框架

## run_cli()方法

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

以python解释器运行py文件，但参数使用robot的参数形式，传入到文件run_cli(sys.argv[1:])中获取robot参数执行robot命令
```

中文文档链接：[Robot Framework用户手册 — robotframework-userguide-cn 3.0.0 文档](https://robotframework-userguide-cn.readthedocs.io/zh-cn/latest/index.html)

  F5 搜索关键字

  F8 运行测试用例

  Ctrl + Shift + 空格 补全关键字

  

  ## 一、基本语法与结构

  基本介绍：rf是基于python开发的、可拓展的、基于`关键字驱动`的自动化测试框架

  **数据驱动**：数据驱动就是把测试用例里面的数据放在execl、yaml里面，然后通过改变excel或者yaml文件里面的数据，达到控制测试用例的执行过程的目的

  **关键字驱动**：把项目中的一些业务逻辑或基本的操作封装成一个一个的关键字，然后调用不同的关键字组合实现不同的业务逻辑。

  特点：

    1. 编写测试用例方便，可以使用robot、txt、tsv、html格式
    2. 可以自动化生成html报告
    3. 非常强大的扩展库
    4. 根据项目的需求自定义关键字
    5. 支持feiGUI的方式运行，以及和jenkins实现持续集成

  ### 1、Robotframework结构

  1.代码格式

  1. **Settings（设置）：**

     - 包括`*** Settings ***`部分。
     - 用于设置全局配置、导入库、定义测试套件变量等。
     - 例如，导入库（`Library`）导入资源（`Recourse`）、定义变量（`Test Setup`、`Test Teardown`）等。

     ```robotframework
     *** Settings ***
     Library    SeleniumLibrary
     Test Setup    Open Browser    https://example.com    Chrome
     Test Teardown    Close Browser
     ```

  2. **Variables（变量）：**

     - 包括`*** Variables ***`部分。
     - 用于定义全局变量，可以在测试套件和测试用例中使用。
     - 例如，定义变量（`@{Browsers}`、`${Username}`）等。

     ```robotframework
     *** Variables ***
     ${Username}    testuser
     ${Password}    secret
     ```

  3. **Test Cases（测试用例）：**

     - 包括`*** Test Cases ***`部分。
     - 用于编写测试用例。
     - 一个测试用例通常包括测试用例名称、关键字和参数。
     - 例如，编写测试用例（`Login Test`、`Search Test`）等。

     ```robotframework
     *** Test Cases ***
     Login Test
         Open Browser    https://example.com    Chrome
         Input Text    username    ${Username}
         Input Text    password    ${Password}
         Click Button    Login
         Page Should Contain    Welcome, ${Username}
     
     Search Test
         Input Text    Search Box    Robot Framework
         Click Button    Search
         Page Should Contain    Search results for "Robot Framework"
     ```

  4. **Keywords（关键字）：**

     - 包括`*** Keywords ***`部分。
     - 用于定义自定义关键字，可被测试用例重复使用。
     - 关键字由一个名称、可选参数和关键字步骤组成。
     - 例如，定义自定义关键字（`Login`、`Search`）等。

     ```robotframework
     *** Keywords ***
     Login
         [Arguments]    ${username}    ${password}
         Open Browser    https://example.com    Chrome
         Input Text    username    ${username}
         Input Text    password    ${password}
         Click Button    Login
     
     Search
         [Arguments]    ${search_text}
         Input Text    Search Box    ${search_text}
         Click Button    Search
     ```

     综合案例：

     ```
     *** Settings ***
     Library           D:/workspaceDemo/wenxuan.qiu/JiraRobotProject/JiraOperation.py    ${url}    ${username}    ${password}
     
     *** Variables ***
     ${username}       wenxuan.qiu    # 用户名
     ${password}       Njpj#04125617    # 密码
     ${url}            http://jira.jaguarmicro.com    # jira路径
     ${testplanname}   autoTestPlanDemo    # 测试计划名称
     
     *** Test Cases ***
     测试用例：更新jira2
         ${testplankey}    关键字1：通过测试计划名称获得测试计划key    ${testplanname}
         ${testcyclename}    关键字2：通过测试计划key获取第一个测试周期名称    ${testplankey}
         ${testrunid}    关键字3：通过测试周期名称获取第一个测试运行id    ${testplankey}    ${testcyclename}
         ${res}    关键字4：通过测试运行id更新对应测试用例    ${testrunid}    1
         log    ${res}
     
     *** Keywords ***
     关键字1：通过测试计划名称获得测试计划key
         [Arguments]    ${testplanname}
         ${testplankey}    JiraOperation.Get Testplan Key By Testplan Name    ${testplanname}
         [Return]    ${testplankey}
     
     关键字2：通过测试计划key获取第一个测试周期名称
         [Arguments]    ${testplankey}
         ${testcyclename}    JiraOperation.Get First Test Cycle Name By Testplan Key    ${testplankey}
         [Return]    ${testcyclename}
     
     关键字3：通过测试周期名称获取第一个测试运行id
         [Arguments]    ${testplankey}    ${testcyclename}
         ${testrunid}    JiraOperation.Get First Run Id In Test Run By Cycle Name And Testplan Key    ${testplankey}    ${testcyclename}
         [Return]    ${testrunid}
     
     关键字4：通过测试运行id更新对应测试用例
         [Arguments]    ${testrunid}    ${status}
         ${res}    JiraOperation.Update Testrun By Run Id    ${testrunid}    ${status}
         [Return]    ${res}
     ```

     #### 2.分隔格式

     2.1 空间分隔

     当 Robot Framework 解析数据时，它首先将数据拆分为行,关键字与参数之间使用空格分隔

     一个关键字由多个字母组成，他们之间使用一个空格分隔，表示一个关键字，如`Should Be Equal`

     关键字与参数之间，一般采用两个或多个空格分隔，但建议使用 使用四个空格使分隔符更易于识别。使用空格排版使得界面更加美观

     ```py
     *** Settings ***
     Documentation     Example using the space separated format.
     Library           OperatingSystem
     
     *** Variables ***
     ${MESSAGE}        Hello, world!
     
     *** Test Cases ***
     My Test
         [Documentation]    Example test.
         Log    ${MESSAGE}
         My Keyword    ${CURDIR}
     
     Another Test
         Should Be Equal    ${MESSAGE}    Hello, world!
     
     *** Keywords ***
     My Keyword
         [Arguments]    ${path}
         Directory Should Exist    ${path}
     ```

     2.2 管道分隔

     ​		空间分隔格式的最大问题是在视觉上 将关键字与参数分开可能很棘手。这是一个问题 特别是如果关键字需要大量参数和/或参数 包含空格。在这种情况下，管道分隔的变体可以 工作得更好，因为它使分隔符更明显。

     一个文件可以同时包含空格分隔线和竖线分隔线。 管道分隔线由强制性前导管道字符识别， 但管线末端的管道是可选的。必须始终在 管道两侧至少有一个空格或制表符。

     ```py
     | *** Settings ***   |
     | Documentation      | Example using the pipe separated format.
     | Library            | OperatingSystem
     
     | *** Variables ***  |
     | ${MESSAGE}         | Hello, world!
     
     | *** Test Cases *** |                 |               |
     | My Test            | [Documentation] | Example test. |
     |                    | Log             | ${MESSAGE}    |
     |                    | My Keyword      | ${CURDIR}     |
     | Another Test       | Should Be Equal | ${MESSAGE}    | Hello, world!
     
     | *** Keywords ***   |                        |         |
     | My Keyword         | [Arguments]            | ${path} |
     |                    | Directory Should Exist | ${path} |
     ```

     实际测试数据中的 | 必须使用反斜杠进行转义,如

     ```py
     | *** Test Cases *** |                 |                 |                      |
     | Escaping Pipe      | ${file count} = | Execute Command | ls -1 *.txt \| wc -l |
     |                    | Should Be Equal | ${file count}   | 42                   |
     ```

  


  ### 2、数据类型

  Robot Framework支持多种基本数据类型，如：字符串、整数、浮点数、布尔值

  除了这些基本的数据结构之外，还包含`列表list`、`字典dict`结构

    1. **列表（List）**：列表是一组有序的值，用中括号括起来，并用逗号分隔，例如：`[1, 2, 3, 'a', 'b']`。
    
    2. **字典（Dictionary）**：字典是一组键-值对的集合，用大括号括起来，例如：`{'name': 'Alice', 'age': 30}`。
    
    3. **变量（Variable）**：变量用于存储和引用数据。Robot Framework的变量包括标量变量、列表变量和字典变量。
       - 标量变量：用`${}`括起来，例如：`${name}`。
       - 列表变量：用`@{}`括起来，例如：`@list`。
       - 字典变量：用`&{}`括起来，例如：`&dict`。
    
    4. **None（空值）**：`None`表示一个空值或未定义的变量。
    
    5. **关键字（Keyword）**：关键字是自定义的可重复使用的测试步骤，可以接受参数并执行特定的操作。

  标量变量、列表变量和字典变量的区别：

    1. **标量变量（Scalar Variables）**：标量变量用`${}`括起来，用于存储单个值，通常是字符串、数字或布尔值；如：`${username} = 'Alice'`
    2. **列表变量（List Variables）**：列表变量用`@{}`括起来，用于存储多个值，这些值按顺序排列在一个列表中，列表中的值可以是任何类型，包括字符串、数字，通常用于存储一组相关的数据；如：`@fruits = ['apple', 'banana', 'cherry']`
    3. **字典变量（Dictionary Variables）**：字典变量用`&{}`括起来，用于存储一组键值对，其中每个键关联一个值。例子：`&person = {'name': 'Alice', 'age': 30, 'is_student': False}`

  ### 3、list和dict类型

  Robotframework复杂数据类型有`list类型，dict类型`

  1.列表list

  列表1：  `${list1} Create List 参数1 参数2 参数3` 输出类型：`${list1} = ['百里', '北凡', '新耀']`

  使用log打印

  **Create List**: 创建一个新的列表，可以添加元素

  ```
${my_list}=  Create List  item1  item2  item3
log    ${my_list}

输出结果：
item1  item2  item3
  ```

  

  列表2：`@{list2} = [ 百里 | 北凡 | 新耀 ]，`使用|竖线分隔，使用`Log many`逐个打印，`一般用于循环`

  **Create List**: 创建一个新的列表，可以添加元素

  ```
@{my_list}=  Create List  item1  item2  item3
log	many	@{my_list}
输出结果：
item1
item2
item3
  ```

  

  ![image-20231108161122702](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231108161122702.png)

  2.字典dict

  **Create Dictionary**: 创建一个新的字典，可以指定键-值对

  ```
${my_dict}=  Create Dictionary  key1=value1  key2=value2
  ```

  **Set To Dictionary**: 将一个键值对添加到字典中

  ```
Set To Dictionary  ${my_dict}  key3  value3
  ```

  **Get Dictionary Keys**: 从字典中获取所有的key。

  ```
${value}=  Get Dictionary  ${my_dict}  key2
  ```

  **Get Dictionary Values**: 从字典中获取所有的values。

  ```
${value}=  Get Dictionary  ${my_dict}  key2
  ```

  **Get From Dictionary** :从字典中获取指定key对应的值

  ```
${value}=  Get From Dictionary  ${my_dict}  non_existent_key  default_value
  ```

  ### 4、流程控制语句

  1.if简单语句

  ```py
Run Keyword If    1<2   Log　　111111
  ```

  2.if语句赋值

  ```py
${rst}    Set Variable If    1 < 2     2　　　　1
  ```

  3.If-else语句

  `IF`语句用于定义一个条件，如果条件为真，则执行一组操作；如果条件为假，则可以选择执行另一组操作。

  注意，`...`是一个示例的缩进标记，你可以使用多个空格或制表符来代替它，以保持代码的可读性。

  ```py
${rst} 　　Run Keyword If 　　1 < 2 　　　　  Set Variable　　2
... 　　　 ELSE IF 　　　　　 Set Variable　　1

Run Keyword If    '${age}' > 18 and '${score}' >= 90
...               Set Variable    Excellent
...               ELSE IF    '${age}' > 18 and '${score}' >= 70
...               Set Variable    Good
...               ELSE
...               Set Variable    Needs Improvement
Log    ${result}

  ```

  4.for循环语句

    1.　FOR … IN RANGE

  ```py
FOR    ${i}                  　　IN RANGE    60
       Continue For Loop If      ${i}>60
       Sleep                     1
       Log                      ${i}
END
  ```

    2.　 FOR … list()

  ```py
@{temp}　　Create List　　　　　　　　a　　　　　　　　b　　　　　　c
FOR       ${each}            　　  IN  　　　　    @{temp}
          Continue For Loop If    ${each}=b
          Sleep              　　  1
          Log                 　　 ${each}
END
  ```

  

  5.else-if语句

  你还可以使用`ELSE IF`语句来定义多个条件，根据不同的条件执行不同的操作。

  ```py
${score} =    Get Test Score
IF    '${score}' >= 90
    Log    Excellent
ELSE IF    '${score}' >= 70
    Log    Good
ELSE
    Log    Needs Improvement
END
  ```

  在上述示例中，根据`${score}`的值，会执行不同的`Log`操作。

  6.if嵌套流程语句

  嵌套IF语句在Robot Framework中允许你在一个IF语句内部再嵌套另一个IF语句，从而实现多层条件判断。

  ```py
${age} =    Get Age
${score} =    Get Test Score

IF    '${age}' > 18
    IF    '${score}' >= 90
        Log    Adult with Excellent score
    ELSE
        Log    Adult with Good score
    END
ELSE
    Log    Not an Adult
END
  ```

  在上述示例中，首先检查`${age}`是否大于18，如果是，就进一步检查`${score}`是否大于等于90。如果`${age}`小于等于18，将执行`Not an Adult`的操作。

  ## 二、环境安装

  参照之前写的公司文档，Robotframework环境安装一章：[VDI 自动化测试环境准备 - Wenxuan Qiu - Confluence (jaguarmicro.com)](http://space.jaguarmicro.com/pages/viewpage.action?pageId=84998143)

  ## 三、RIDE的基本介绍及使用

  界面功能介绍：[RIDE使用教程 - 简书 (jianshu.com)](https://www.jianshu.com/p/5a8f296dc678)

  ### 1、**ride的一些模块功能介绍**

  **(1)加载外部文件**

  导入界面  Library：加载测试库，Resource：加载资源，Variables：加载变量文件

  **(2)定义内部变量**

  Add Scalar：定义变量。Add List：定义列表型变量。Add Dict：定义字典

  (**3)元数据定义**

  Add Metadata：定义元数据。（对“元数据”的理解可百度）

  **(4)settings**

  Documentation：文档，（项目，套件，用例都有。）可以给当前的对象加入文档说明。

  Suite Setup：测试套件启动的时候就执行某个关键字。(例：我在Suite Setup设置了Sleep | 5sec，表示等待5秒，要注意关键字的参数要使用 | 分隔)

  Suite Teardown：测试套件结束的时候就执行某个关键字。

  Test Setup：案例启动的时候执行某个关键字。

  Test Teardown：案例结束的时候执行某个关键字。

  Test Template：测试模版，这是可以指定某个关键字为这个测试套件下所有TestCase的模版，这样所有的TestCase就只需要设置这个关键字的传入参数即可。

  Test Timeout：设置每一个测试案例的超时时间，只要超过这个时间就会失败，并停止案例运行。这是防止某些情况导致案例一直卡住不动，也不停止也不失败。

  ![image-20231108143451666](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231108143451666.png)

  ### 2、Robotframework文件结构

  1.项目Project

  ![image-20231108102519627](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231108102519627.png)

  项目的文件类型为directory文件夹，文件夹下只能创建测试套件**Test Suite**

  2.测试套件**Suite**

  测试套件的文件类型为file,因为在file文件内才能创建测试用例**Test case**

  ![image-20231108103129398](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231108103129398.png)

  3.测试用例Test case

  测试用例必须放在测试套件内

  ![image-20231108103334700](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231108103334700.png)

  文件层级关系：

  - 项目-》测试套件-》测试用例
  - 项目-》资源文件-》关键字/变量

  ![image-20231108144857078](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231108144857078.png)

  4.资源文件Resource

  在文件夹下面可以创建资源文件，格式必须选择txt格式。一个资源文件下面可以创建很多用户自定义的关键字，可以在测试套件中导入并调研它里面的自定义关键字。

  ![image-20231108144227952](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231108144227952.png)

  5.关键字Keyword

  在资源文件txt下创建关键字Keyword

  ![image-20231108144423588](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231108144423588.png)

  

  ### 3、RIDE运行简单的测试用例

  首先、先创建一个项目`Robotdemo02（directory）`

  在项目下创建一个测试套件`Suite01(file)`用来存放测试用例，在Suite下创建一个用例`Test_01(test case)`

  我们再在项目Robotdemo02下创建一个资源文件用来存放关键字：`资源文件(txt)`

  在关键字.txt下创建`关键字(Keyword)`创建一个登录关键字、一个删除关键字

  ![image-20231108153146081](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231108153146081.png)

  ![image-20231108153231406](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231108153231406.png)

  接下来在Suite01的import内引入资源文件，切记资源文件与测试用例相对位置最好是比较近

  ![image-20231108153339565](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231108153339565.png)

  此时测试套件下所有的测试用例都可以使用资源文件的关键字

  如：测试用例Test_01

  ![image-20231108153436881](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231108153436881.png)

  运行测试用例：勾选测试用例，点击Run界面的Start运行测试用例并输出报告文件

  ![image-20231108153724487](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231108153724487.png)

  ### 4、使用RIDE导入外部文件

  首先使用Import Library关键字导入外部文件

  我们在D盘创建了一个sum.py文件，用于计算a+b的和

  导入D:/sum.py，使用Evaluate关键字传入两个入参，将得到的结果放在变量sum内

  ![image-20231108164357519](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231108164357519.png)

  打印结果：

![image-20231108164421147](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231108164421147.png)

  

### 3、RobotFramework的命令行执行用例



  ## 四、Robotframework常用关键字 

  ### 1、快捷键

  F5 搜索关键字

  ![image-20231108154507812](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231108154507812.png)

  F8 运行测试用例

  Ctrl + Shift + 空格 补全关键字

  ### 2、常用关键字

  1.`Evaluate` 执行python语法

  ![image-20231108163426630](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231108163426630.png)

  2.`Log 打印`

  3.`Comment 注释`

  Comment 注释（左边栏右键快捷注释）

  ![image-20231108154943922](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231108154943922.png)

  ![image-20231108155010087](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231108155010087.png)

  4.获取时间`Get Time`

  5.连接多个字符串

  连接多个字符串`Catenate 参数1 参数2` 默认输出参数之间用空格连接，我们可以使用 `SEPARATOR=#`设置连接的字符为#

  ![image-20231108155852464](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231108155852464.png)

  

  ## 五、外部资源导入

  ### 1、简单函数导入

  导入的libraries文件都是函数，则可以直接导入，注意函数名最好和文件名.py相同

  导入资源文件Resource则是txt，robot文件类型的文件，直接导入

  ### 2、复杂文件导入

  Robotframework复杂文件导入，如：在Library导入一个类时，如果这个类有初始化方法则需要传参，即在导入的文件名后面接上初始化参数，这样Robotframework导入文件时默认就会给你的类初始化，且类方法的函数名都会被当成关键字可以直接调用

  ```
*** Settings ***
Library           D:/workspaceDemo/wenxuan.qiu/JiraRobotProject/JiraOperation.py    ${url}    ${username}    ${password}  # 传入初始化参数

你可以将函数只从转成的关键字再次封装成可理解的关键字：

关键字1：通过测试计划名称获得测试计划key
    [Arguments]    ${testplanname}
    ${testplankey}    JiraOperation.Get Testplan Key By Testplan Name    ${testplanname}
    [Return]    ${testplankey}

  ```

  ![image-20231110103119809](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231110103119809.png)

  ## 六、输出文件

  ### 1、默认生成文件

  Robotframework执行完测试用例后会默认在用户文件夹内生成三个文件

  - xml类型的输出文件
  - html类型的Log日志文件
  - html类型的report报告文件

  ![image-20231110103448405](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231110103448405.png)

  ### 2、指定路径生成文件

  我们也可以在命令行指定生成的文件路径，方法如下

  可以使用标签分别指定生成的文件的路径

  ```
robot --outputdir path/to/report/directory --log path/to/log/file.log --report path/to/report/file.html your_test_suite.robot
  ```

  ![image-20231110103709685](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231110103709685.png)

  当然，如果你指定其中一个文件的路径，其他两个文件也会默认在该路径下生成；

  ![image-20231110104131313](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231110104131313.png)

  ### 3、将RF报告集成到Allure框架

  如果需要将Robotframework的输出结果以Allure测试报告的形式展示，我们需要先下载插件allure-robotframework和robotframework-allurereport

  ```
pip install allure-robotframework
pip install robotframework-allurereport
  ```

  下载完插件之后，使用标签`--listener allure_robotframework`将Robotframework用例执行结果以allure能识别的本地数据文件格式输出到指定文件夹内

  ```py
robot --listener allure_robotframework+执行的robot文件.robot
robot --listener allure_robotframework "D:\workspacedemo\wenxuan.qiu\JiraRobotProject\功能模块：更新jira测试用例\JiraOP测试套件.robot" # 执行robot文件会在当前文件默认生成output/allure的数据文件
  ```

  这里我们可以看到，在当前文件夹不仅生成了日志和报告，也生成了一个allrue框架格式的本地文件夹，这个文件夹可以被allure识别，使用allure serve命令查看allure测试报告；也可以使用generate 命令生成测试报告html文件

  使用`--listener allure_robotframework`标签会自动在当前.robot文件下生成output/allure的数据文件和日志+报告+xml三件套，如下所示：

  ![image-20231113114906788](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231113114906788.png)

  

  ![image-20231110173023252](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231110173023252.png)

  

  如果是在RIDE内输出allure识别的数据文件，将参数放在Arguments输入框内

  ![image-20231110155737980](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231110155737980.png)

  在指定路径生成log4文件：

  ![image-20231110152639930](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231110152639930.png)

  

  使用allure框架的generate命令解析本地数据文件，并生成本地html测试报告

  ```py
allure generate {本地数据文件} -o {生成的allrue测试报告文件}
allure generate ./log4 -o ./allure-report
allure generate .\output\allure\ -o ./allure-reprots
  ```

  ![image-20231110173222179](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231110173222179.png)

  打开生成的本地html文件，此时Robotframework测试用例报告将以allure的形式展示

  ![image-20231110153057463](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231110153057463.png)

  ### 4、将RF报告与pytest报告整合在一起

  以上我们已经熟悉了将rf报告输出为allure可识别的数据文件的方法，如生成的allure-Robotframework测试框架的数据文件夹默认为output/allrue,pytest框架生成的数据文件report，使用generate命令合成两个数据文件生成一个本地包含html的测试报告

  如图：report为pytest框架下的测试数据文件，output/allure为Robotframework测试数据问价

  ![image-20231113142136902](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231113142136902.png)

  使用generate -o指定多个数据文件合成一个html本地框架文件，命令如下

  ```py
allure generate D:\workspacedemo\output\allure D:\workspacedemo\report -o allrue-pytest-robot
  ```

  ![image-20231113142511830](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231113142511830.png)

  此时本地生成了一个allrue-pytest-robot文件夹

  ![image-20231113142557529](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231113142557529.png)

  ## 七、案例一：使用RF完成对jira用例的更新

  

  首先我们使用RIDE创建一个项目文件夹JiraRobotProject

  在项目文件夹下创建一个功能模块（存放测试套件和功能测试用例）、一个资源模块文件夹（存放公共数据和公共元素资源文件（内含关键字））

  之所以要将Robotframework文件分为资源模块和功能模块，有利于测试用例选择资源文件当中的元素完成各种业务流程，增加了代码的复用性，易于管理和拓展

  ![image-20231109180153863](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231109180153863.png)

  找到外部的资源文件，并导入外部资源Library,找到python文件JiraOperation.py（内含操作jira测试用例的类方法），导入到资源文件：公共元素内：

  导入的文件名包含拓展名，且文件名与类名要相等

  这里要注意，如果你导入的是一个函数或方法，则Args不需要传参

  如果是一个类，导入类文件时robot会默认给这个类初始化对象，此时需要你在导入的时候传入构造参数，才可以正常导入含类py文件

  ![image-20231109180816544](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231109180816544.png)

  导入类之后，我们可以在资源文件内将类方法封装成关键字，如下所示

  robot会默认将py文件的方法转换为robot关键字，这里我们也可以再将python中的关键字封装成中文通俗易懂的关键字，如果方法有入参和出参，需要将其填在上方的列表当中

  ![image-20231109181342636](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231109181342636.png)

  这里我创建了四个关键字，刚好可以按顺序拼接到测试用例中完成一整套更新测试用例的业务流程。如下所示：

  首先在功能模块的文件夹下创建一个测试套件（测试套件内可以导入资源文件可以供套件内的所有用例访问），在测试套件下创建一个测试用例：更新jira

  创建完之后我们需要能够在用例当中使用资源文件：公共元素当中的关键字，因此我们在测试套件当中导入资源文件，如下所示：

  ![image-20231109182256097](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231109182256097.png)

  我还导入了公共数据.txt的资源文件，我们可以在公共数据.txt资源文件当中创建变量名并赋初始值

  ![image-20231109182526530](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231109182526530.png)

  导入完资源文件之后，我们就可以在测试用例当中调用资源文件的关键字完成不同的业务流程

  ![image-20231109182647436](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231109182647436.png)

  运行测试用例

  运行测试用例有两种方法，第一种是直接使用RIDE的RUN界面运行，首先勾选测试用例，点击Start按钮，用例运行成功

  ![image-20231109183000963](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231109183000963.png)

  测试用例运行成功，默认在c盘生成测试报告和日志文件

  还有一种运行方法就是使用命令行的格式运行

  在命令行终端使用robot + .robot文件路径运行，如下：（使用这种方法，如果运行的robot文件导入了其他路径的资源文件，需要确保资源文件在.robot文件内的索引位置要一致，否则.robot文件找不到资源文件的路径而发生报错）

  ![image-20231109183206579](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231109183206579.png)

  查看.robot文件下的资源文件路径：

  ![image-20231109183504274](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231109183504274.png)

  查看生成的测试报告和日志文件

  测试报告：

  ![image-20231109183701976](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231109183701976.png)

  日志文件：

  ![image-20231109183709420](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231109183709420.png)

  测试用例+资源文件汇总robot代码：

  ```py
*** Settings ***  # 导入设置区
Library           D:/workspaceDemo/wenxuan.qiu/JiraRobotProject/JiraOperation.py    ${url}   ${username}    ${password}

*** Variables *** # 变量区
${username}       wenxuan.qiu    # 用户名
${password}       Njpj#04125617    # 密码
${url}            http://jira.jaguarmicro.com    # jira路径
${testplanname}    autoTestPlanDemo    # 测试计划名称

*** Test Cases *** # 测试用例区
测试用例：更新jira2
    ${testplankey}    关键字1：通过测试计划名称获得测试计划key    ${testplanname}
    ${testcyclename}    关键字2：通过测试计划key获取第一个测试周期名称    ${testplankey}
    ${testrunid}    关键字3：通过测试周期名称获取第一个测试运行id    ${testplankey}    ${testcyclename}
    ${res}    关键字4：通过测试运行id更新对应测试用例    ${testrunid}    1
    log    ${res}

*** Keywords ***   # 关键字区
关键字1：通过测试计划名称获得测试计划key
    [Arguments]    ${testplanname}
    ${testplankey}    JiraOperation.Get Testplan Key By Testplan Name    ${testplanname}
    [Return]    ${testplankey}

关键字2：通过测试计划key获取第一个测试周期名称
    [Arguments]    ${testplankey}
    ${testcyclename}    JiraOperation.Get First Test Cycle Name By Testplan Key    ${testplankey}
    [Return]    ${testcyclename}

关键字3：通过测试周期名称获取第一个测试运行id
    [Arguments]    ${testplankey}    ${testcyclename}
    ${testrunid}    JiraOperation.Get First Run Id In Test Run By Cycle Name And Testplan Key    ${testplankey}    ${testcyclename}
    [Return]    ${testrunid}

关键字4：通过测试运行id更新对应测试用例
    [Arguments]    ${testrunid}    ${status}
    ${res}    JiraOperation.Update Testrun By Run Id    ${testrunid}    ${status}
    [Return]    ${res}

  ```

## 八、RobotFramework命令行运行测试用例

![image-20240104194740163](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240104194741.png)

- 执行测试套件内的单个测试用例

```
pybot --test 用例名称 测试套件路径
robot --test 02-清理环境并搭建环境23net+24blk C:\Users\wenxuan.qiu\Pycharmproject\product_test\01-TestCase\06-DDK-Service-Pkg\DDK-管控admin 2.0\01-测试裸金属场景\02-本地裸金属环境搭建.robot

例：
pybot --test 002_电话红娘系统_客户管理_我的珍爱通_今日联系会员查询 D:\code\testcases\api\crm\tel_matchmaker\
客户管理.robot

robot --test "03-清理环境并搭建环境4net+4blk" C:\Users\wenxuan.qiu\Pycharmproject\product_test\01-TestCase\06-DDK-Service-Pkg\DDK-管控admin_2.0\01-测试裸金属场景\02-本地裸金属环境搭建.robot \
--test "01-查看版本信息,02-提取和查看各模块版本信息,03-判断设备状态均ok" \
C:\Users\wenxuan.qiu\Pycharmproject\product_test\01-TestCase\06-DDK-Service-Pkg\DDK-管控admin_2.0\01-测试裸金属场景\03-SOC信息及环境检测.robot

robot --suit 03-SOC信息及环境检测.robot --test "01-查看版本信息 02-提取和查看各模块版本信息 03-判断设备状态均ok" 

```

- RobotFramework指定测试报告的生成目录

```
Installation and Usage
$ pip install allure-robotframework
$ robot --listener allure_robotframework ./my_robot_test
The default output directory is output/allure. Use the listener's argument to change it:

$ robot --listener allure_robotframework:/set/your/path/here ./my_robot_test
The listener supports the robotframework-pabot library:

$ pabot --listener allure_robotframework ./my_robot_test
```

- 执行测试文件


```
pybot 测试文件路径（绝对路径 或 相对路径都可）

pybot 客户管理.robot
```

- 执行测试目录

```
pybot 测试目录路径（绝对路径 或 相对路径都可）

```



  # 六、pytest+Allure标签汇总

| 标签名                | 功能描述                                                     | 用法示例                                                     |
| --------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Allure 标签**       |                                                              |                                                              |
| `@allure.feature()`   | 标记测试用例的功能模块，用于大类别的分类。                   | `@allure.feature('登录')` 表示测试用例属于登录模块。         |
| `@allure.story()`     | 标记具体的用户故事或功能，用于更细致的分类。                 | `@allure.story('密码错误时登录')` 表示一个具体的用户故事或测试场景。 |
| `@allure.step()`      | 标记测试用例的特定步骤，用于提供步骤级的详情。               | `@allure.step('输入用户名和密码')` 表示记录了一个测试步骤。  |
| `@allure.severity()`  | 标记测试用例的严重性级别，用于指示测试的重要性。             | `@allure.severity(allure.severity_level.NORMAL)` 表明测试用例的严重性级别为普通。 |
| `@allure.title()`     | 为测试用例提供一个自定义的标题，用于增强报告的可读性。       | `@allure.title('验证有效登录')` 为测试用例设置了一个明确的标题。 |
| **Pytest 标签**       |                                                              |                                                              |
| `@pytest.mark`        | 用于标记测试，使其可以作为一组运行或特殊处理。               | `@pytest.mark.slow` 表示这是一个运行缓慢的测试。             |
| `@pytest.mark.skip`   | 总是跳过带有此标记的测试。                                   | `@pytest.mark.skip(reason='暂不运行')` 总是跳过这个测试。    |
| `@pytest.mark.skipif` | 在满足某个条件时跳过带有此标记的测试。                       | `@pytest.mark.skipif(sys.version_info < (3, 7), reason='需要Python 3.7+')` 在Python版本小于3.7时跳过测试。 |
| `@pytest.mark.xfail`  | 标记预期会失败的测试。如果测试通过，它会被标记为意外的成功。 | `@pytest.mark.xfail(reason='此功能尚未完成')` 标记一个预期失败的测试。 |
| `@pytest.mark.<name>` | 创建自定义标记，可用于对测试进行自定义的分类或过滤。         | `@pytest.mark.smoke` 创建了一个名为“smoke”的自定义标记。     |

  首先是allure下的标签，该系列下的标签用于显示在allure测试报告的html界面上

  ## 1、allure标记装饰器

  ### 1.feature()、story()和step()

  - @allure.feature()
  - @allure.story ()
  - @allure.step()

  @allure.feature()一般用于标记类，属于类层级的allure装饰器

  @allure.story()相当于feature的子集，用于标记类下的方法

  @allure.story()相当于story的子集，用于标记方法下的关键步骤

  ![image-20231018111148773](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231018111148773-169992864057234.png)

  如下所示：

  ```py
import allure
import pytest


@allure.feature("登录模块")
class TestLogin:
    # @allure.title("登录功能")
    @allure.story("登录成功")
    def test_login_success(self):
        with allure.step("步骤1：打开页面"):
            print("打开页面")
        with allure.step("步骤2：登录页面"):
            print("页面登录")
        with allure.step("步骤3：登录密码"):
            print("输入密码")
        assert 1 == 1
        print("用户登录成功！")

    @allure.story("登录失败")
    def test_login_failure(self):
        with allure.step("步骤1：打开页面"):
            print("打开页面")
        with allure.step("步骤2：登录页面"):
            print("页面登录")
        with allure.step("步骤3：输入密码"):
            print("输入密码")
        assert "1" == 1
        print("页面登录失败！")


@allure.feature("搜索模块")
class TestSearch:

    @allure.story("搜索成功")
    def test_login_success(self):
        with allure.step("步骤1：打开页面"):
            print("打开页面")
        with allure.step("步骤2：搜索信息"):
            print("页面搜索")
        assert 1 == 1
        print("搜索信息成功展示！")

    @allure.story("搜索失败")
    def test_login_failure(self):
        with allure.step("步骤1：打开页面"):
            print("打开页面")
        with allure.step("步骤2：搜索信息"):
            print("页面搜索")
        assert "1" == 1
        print("页面登录失败！")
  ```

  我们先看看没有设置标记装饰器时，allure报告是咋样的

![img](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/format,png.png)

  ![img](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/format,png-16999258795471.png)

   

  加了@allure.feature和@allure.story之后，allure报告又是怎么样的呢

![img](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/format,png-16999258795472.png)

  ![img](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/format,png-16999258795483.png)

   两种装饰器标记类和方法，用于将抽象的类和方法名替换成装饰器内的字体展示在测试报告中中，使得测试报告更加通俗易懂

  ### 2.severity 优先级标签

  **作用：**按严重性（优先级）来标记测试用例，它使用allure.severity_level枚举值作为参数

  **BLOCKER**: 阻塞性缺陷，阻止功能使用或导致数据丢失，需要立即解决。

  **CRITICAL**: 关键性缺陷，影响关键功能的错误，但不会导致系统完全无法使用。

  **NORMAL**: 一般性缺陷，影响非核心功能的错误，或不符合预期的行为。

  **MINOR**: 次要问题，不影响功能和用户操作的小问题或界面缺陷。

  **TRIVIAL**: 微不足道的问题，一些微小的改善点，例如拼写错误或界面微调。

  ```py
import allure

@allure.severity(allure.severity_level.BLOCKER)
def test_function():
    # 测试代码

  ```

  运行结果，查看allure报告

  其实就是测试用例多了个优先级severity属性而已...

  ![img](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/format,png-169992628328812.png)

  我们也可以通过命令行参数运行指定severity的测试用例哦

  当使用`--allure-severites`筛选执行用例时，如果一个类优先级被选中，那么这个类内所有相同优先级的方法+未定义优先级的方法将被执行。

  ```py
pytest tests.py --allure-severities normal,critical
  ```

  

  ### 3. title 添加标题

  title只能标识测试方法和函数，作用是在测试报告Suites上用title信息替换抽象的类和方法名，使得测试报告更好理解。

  ```py
@allure.title("title:登录模块")
@allure.feature("登录模块")
class TestLogin:
    # @allure.title("登录功能")
    @allure.title("title:登录成功")
    @allure.story("登录成功")
    def test_login_success(self):
        with allure.step("步骤1：打开页面"):
            print("打开页面")
        with allure.step("步骤2：登录页面"):
            print("页面登录")
        with allure.step("步骤3：登录密码"):
            print("输入密码")
        assert 1 == 1
        print("用户登录成功！")

    @allure.title("title:登录失败")
    @allure.story("登录失败")
    def test_login_failure(self):
        with allure.step("步骤1：打开页面"):
            print("打开页面")
        with allure.step("步骤2：登录页面"):
            print("页面登录")
        with allure.step("步骤3：输入密码"):
            print("输入密码")
        assert "1" == 1
        print("页面登录失败！")
  ```

  ![image-20231018160720425](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231018160720425-169992864057335.png)

  

  ## 2、pytest标签

  pytest标签不会显示在allure测试报告下，但是可以控制测试用例的执行

  ### 1.Mark标签

  ![image-20231017152305625](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152305625-169992864057337.png)

  **1.Mark.标签名 设置标签组**

  首先我们要再测试用例上使用解释器定义mark标签

  ![image-20231017152317492](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152317492-169992864057336.png)

  然后，再使用-m标签进行标签选择执行，有三种选择方式都可

  ```py
-m=double
-m double
-m "double"
-m 'double'
  ```

  ![image-20231017152337581](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152337581-169992864057339.png)

  另外，以上都是我们自定义的一个标记，pytest识别不出会给我们抛出警告，并不影响运行

  ![image-20231017152344921](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152344921-169992864057341.png)

  我们可以在pytest.ini配置文件中配置标签名，这样pytest就能自动识别出我们的标签不会给警告了

  在pytest.ini配置标签名

  ![image-20231017152357788](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152357788-169992864057338.png)

  再执行用例发现不再报警告

  ![image-20231017152414446](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152414446-169992864057343.png)

  **2.自定义mark**

  mark首先在pytest.ini配置文件里面定义，才可以使用

  定义mark配置信息：将mark自定义分为三类：test1、test2、test3,

  即markers = 类型名 ： 类型注释

  ![image-20231017152433978](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152433978-169992864057340.png)

  你可以用mark标记不同的测试用例，通过 pytest -m 类型名 筛选需要执行哪类测试用例

  - 选择这类测试用例： pytest -m 类型名
  - 不选择这类测试用例： pytest "-m not" 类型名 由于not会当成类型名，因此我们要用” “包裹 

  ![image-20231017152505665](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152505665-169992864057342.png)

  执行标签用例

  ![image-20231017152515087](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152515087.png)

  **3.Mark.skip 跳过测试用例**

  ![image-20231017152626079](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152626079.png)

  使用场景

  ![image-20231017152632970](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152632970.png)

  ```py
方法1：添加装饰器：
    @pytest.mark.skip 为方法级别的跳过，也可以用作类级别的跳过
    @pytest.mark.skipif 为方法级别的跳过，也可以用作类级别的跳过
方法2：代码中添加跳过后面的代码
    pytest.skip(reason)
  ```

  #### 1.skip跳过

  ```py
@pytest.mark.skip(reason="代码没有实现") # 在用例前面添加skip装饰器，reason上输出跳过提示
def test_zero():
    assert 0 == func(0)
  ```

  ![image-20231017152713637](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152713637-169992864057448.png)

  #### **2.skipif代码跳过**

  ```py
# skipif的用法
@pytest.mark.skipif(条件表达式, reason="")
# 当条件表达式返回True时，会跳过用例

@pytest.mark.skipif(1 == 1,reason='满足条件跳过此用例') # 跳过用例并输出reason信息
def test_pytest_1():
    print('case4')

@pytest.mark.skipif(1 == 2,reason='不满足条件不会跳过此用例')
def test_pytest_2():
    print('case5')
  ```

  实例，当平台操作系统为win，且python版本高于3.6时跳过用例

  ```py
print(sys.platform)  # 输出操作系统平台 win为windows,darwin为mac


# 判断如果为win操作系统，跳过该用例并输出信息
@pytest.mark.skipif(sys.platform == 'win32', reason="does not run windows")
def test_case1():
    assert True


# 判断如果为mac操作系统，跳过该用例并输出信息
@pytest.mark.skipif(sys.platform == 'darwin', reason="does not run mac")
def test_case2():
    assert True


# 判断python版本信息小于3.6，跳过用例给出信息
@pytest.mark.skipif(sys.version_info < (3, 6), reason="requires python3.6 or lower")
def test_case3():
    assert True

  ```

  ![image-20231017152748253](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152748253-169992864057450.png)

  #### **3.skip代码内判断跳过**

  创建一个检查登录方法，和一个测试方法

  ```py
def check_login():  # 检查登录方法
    return False


def test_function():  # 测试方法
    print("start...")

    if not check_login():  # 如果登录不成功，跳过后续步骤，并返回reason信息
        pytest.skip("登录未成功...") # 用代码实现步骤跳过

    print("end...")
  ```

  执行测试用例：当if判断错误时调用pytest.skip跳过后续代码，并输出跳过reason

  ![image-20231017152816084](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152816084-169992864057449.png)

  **4.Mark.xfail  标签**

  期望测试用例为失败并标记，失败不会影响其他测试用例执行

  ![image-20231017152832940](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152832940-169992864057347.png)

  xfail相比较于skip来说，虽然使用的格式类似，但是xfail不会跳过用例而是执行测试用例，成功输出xpass，失败输出xfail

  xfail相比于skip，reason任何时候都会输出

  ```py
# 用法1：加上装饰器@pytest.mark.xfail
@pytest.mark.xfail(reason="bug case1") 
def test_case1():
    print("test_xfail1方法执行...")
    assert 1 == 2

# 我们也可以定义xfail变量为pytest.mark.xfail，下次调用直接使用@xfail便捷调用
xfail = pytest.mark.xfail

@xfail(reason="bug case2")
def test_case2():
    print("test_xfail方法执行...")
    assert 1 == 1
  ```

  ![image-20231017152846960](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231017152846960-169992864057451.png)

  xfail内部代码跳过,与skip跳过不同的是，xfail没有判断条件，执行完xfail无论如何都会跳过后面的代码

  ```py
def test_xfail():
    print("*****开始测试*****")
    pytest.xfail(reason="该功能尚未完成") # xfail在代码内会暂停继续执行下面的代码
    print("测试过程")
    assert 1 == 1
  ```

  ![image-20231120233329825](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231120233331.png)

