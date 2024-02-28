# VDI 自动化测试环境准备

## 1、安装组件

1. python3.8
2. pytest
3. paramiko
4. JDK
5. allure-pytest
6. pip源

## 2、安装过程

### 1.Python安装

首先，我们需要先安装python 3.8.10环境，如果安装最新版可能会引起不兼容的报错。

在本机上找到我们的python-3.8.10-amd.exe，复制粘贴到VDI桌面

打开并安装python3.8.10,步骤如下：

![image-20231019204339504](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231019204339504.png)

![image-20231019204350844](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231019204350844.png)



![img](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/wps2.jpg) 

![img](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/wps3.jpg) 

然后我们打开环境变量，查看环境变量是否自动配好：

可见，python解释器路径，Scripts路径都已配好

![image-20231020094222407](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231020094222407.png)

我们在打开cmd，输入`python -V`查看python版本，出现python版本信息代表python环境安装成功：

![image-20231020094548619](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231020094548619.png)

### 2.Java环境安装

#### 2.1 java安装

首先，将下载好的JDK安装包复制粘贴到VDI环境桌面

建议直接默认C盘安装，如果C盘空间不足可以点击更改安装路径，这一步是jdk安装,安装路径需要记住，后面步骤会用到

![image-20231020095314823](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231020095314823.png)

如果以上jdk的安装选择了自定义安装路径，那么下一步jre的路径也建议更改在jdk同一目录下

果上一步没有更改，直接点击下一步。

![image-20231020095742706](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231020095742706.png)

安装成功

![image-20231020095847789](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231020095847789.png)



#### 2.2 配置java环境变量

打开系统设置，点击高级程序设置

![image-20231020100104110](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231020100104110.png)

点击环境变量

![image-20231020100133969](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231020100133969.png)

打开环境变量之后，点击下方的新建

![image-20231020100240151](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231020100240151.png)

1.新建JAVA_HOME变量

```py
JAVA_HOME
```

点击浏览目录，找到刚才记住的java安装路径（未修改默认为 C:\Program Files\Java\jdk1.8.0_181）版本可能不一样

![image-20231020100503467](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231020100503467.png)

2.编辑PATH变量

找到系统变量里面的PATH变量，并新建一个路径（必须是全英文路径，java环境安装路径不能有中文或中文字符）

```py
%JAVA_HOME%\jre\bin
```

![image-20231020100752677](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231020100752677.png)

#### 2.3 CLassPath变量

方法和JAVA_HOME类似，在初始界面新建

```py
ClassPath
```

变量值直接复制即可

```py
.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar; 
```

![image-20231020101050199](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231020101050199.png)

配完环境变量一定要点击确定！！

#### 2.4 验证java环境

进入cmd命令框，输入以下命令：`java`

![image-20231020101709289](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231020101709289.png)

输入`javac`

![image-20231020101728654](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231020101728654.png)

出现以上信息，说明java环境安装成功！

### 3.配置公司镜像源

由于公司的VDI环境属于公司内网，因此在配置pycharm第三方库之前，我们需要对python安装工具pip配置公司镜像源

打开cmd控制台，输入以下命令：

```py
pip config set global.index-url https://mirrors.jaguarmicro.com/pypi/simple
```

配置好公司源后，我们就可以在cmd控制台使用pip安装第三方包了。但我们以往能够直接在在pycharm上安装第三方包，因此也需要对pycharm配置公司的源路径

打开pycharm,点击左下角的python package按钮，点击设置图标

![image-20231020102849434](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231020102849434.png)

添加公司源：

```py
pip config set global.index-url https://mirrors.jaguarmicro.com/pypi/simple
```

![image-20231020103011118](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231020103011118.png)

### 3.Pycharm第三方库安装

#### 3.1 pytest安装

打开pycharm,点击左下角的python package按钮，搜索pytest第三方包，点击下载

![image-20231020103401201](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231020103401201.png)

下载完成自动安装成功。

查看pytest是否安装成功：

打开设置setting，打开python编辑器，查看pytest包已经安装到项目文件内

![image-20231020105124073](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231020105124073.png)



#### 3.2 allure-pytest安装

和pytest安装方法相同，都是打开pycharm,点击左下角的python package按钮，搜索allure-pytest第三方包，点击下载安装即可。

#### 3.3 paramkio安装

和pytest安装方法相同，都是打开pycharm,点击左下角的python package按钮，搜索paramkio第三方包，点击下载安装即可。

查看安装包是否全部安装成功：

![image-20231020105524568](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231020105524568.png)

### 4.robotframework框架安装

打开cmd管理员模式，执行以下命令：

```
#先确认Python版本是3.10.0
python3.exe -V
 
 
#安装RF
pip.exe install robotframework --trusted-host mirrors.jaguarmicro.com
pip.exe install robotframework-ride --trusted-host mirrors.jaguarmicro.com
pip.exe install serial --trusted-host mirrors.jaguarmicro.com
pip.exe install robotframework-sshlibrary --trusted-host mirrors.jaguarmicro.com
```

