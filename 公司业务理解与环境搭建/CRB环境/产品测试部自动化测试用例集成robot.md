# 1、搭建产品测试部测试环境

## 1.安装python 3.75 和第三方包

首先下载到产品测试部已经调试好的代码，根据他们环境搭建文档执行环境部署

环境安装：[自动化测试Windows python基础包使用说明 - DPU_QA_TEAM - Confluence (jaguarmicro.com)](http://space.jaguarmicro.com/pages/viewpage.action?pageId=75601125#id-自动化测试Windowspython基础包使用说明-目的：统一robotframwork、RIDE及脚本执行依赖python包)

适用于：本地windows PC、windows VDI

目的：统一RobotFramework、ride及脚本执行依赖基础包

安装方法：

1. 下载安装包

2. 安装Miniconda3
   1. 本地PC安装，按默认配置安装即可（正常默认安装在C:\ProgramData\Miniconda3，若未安装在路径，指定路径C:\ProgramData\Miniconda3重新安装）
   2. VDI安装，安装到C:\ProgramData\Miniconda3，默认不在该路径

3. 将py375.zip解压到Miniconda的envs目录
   1. py375.zip下载路径 http://jfrog.jaguarmicro.com:8082/artifactory/auto_test/py375.zip
   2. 注意：若安装在C:\ProgramData，该目录在Win10是隐藏的
   3. ![image-20231221185552701](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231221185553.png)

4. 安装成功后，在开始菜单选择下图标记图标打开Miniconda命令行，执行以下命令打开py375环境

5. 切换到py375\Scripts目录，打开RIDE，打开成功即可运行脚本

   ```
   conda activate py375
   python C:\ProgramData\Miniconda3\envs\py375\Scripts\ride.py
   ```

   

   1. ![image-20231221185846582](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231221185848.png)
   2. ![image-20231221185902466](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231221185903.png)

RF包中包含的库

最新包，包含库具体如下：

```
(py375) C:\Users\loneba.zhang>pip list
Package            Version
----------------------------- -------------
alabaster           0.7.13
allure-pytest         2.13.2
allure-python-commons     2.13.2
allure-robotframework     2.13.2
arrow             1.2.3
artifactory          0.1.17
attrs             23.1.0
Automat            22.10.0
Babel             2.12.1
bcrypt             4.0.1
behave             1.2.6
certifi            2022.12.7
cffi              1.15.1
charset-normalizer       3.1.0
click             8.1.7
colorama            0.4.6
constantly           15.1.0
cryptography          39.0.2
decorator           5.1.1
dict2xml            1.7.3
dnspython           2.3.0
docker             6.0.1
docutils            0.19
email-validator        1.3.1
et-xmlfile           1.1.0
exceptiongroup         1.1.1
Flask             2.2.5
func-timeout          4.3.5
future             0.18.3
hyperlink           21.0.0
idna              3.4
ifaddr             0.2.0
imagesize           1.4.1
importlib-metadata       6.6.0
importlib-resources      5.12.0
incremental          22.10.0
iniconfig           2.0.0
install-requires        0.3.0
invoke             2.1.2
itsdangerous          2.1.2
Jinja2             3.1.2
jmespath            1.0.1
jsonpath-ng          1.5.3
jsonschema           4.17.3
libtmux            0.21.1
markdown-it-py         2.2.0
MarkupSafe           2.1.3
mdurl             0.1.2
netmiko            4.2.0
ntc-templates         3.3.0
numpy             1.21.6
openpyxl            3.1.2
packaging           23.0
paramiko            3.1.0
parse             1.19.0
parse-type           0.6.0
pathlib            1.0.1
pathspec            0.11.2
pexpect            4.8.0
Pillow             9.4.0
pip              22.3.1
pkgutil_resolve_name      1.3.10
platformdirs          3.8.1
pluggy             1.0.0
ply              3.11
psutil             5.9.0
ptyprocess           0.7.0
py               1.11.0
pyartifactory         1.13.0
pycparser           2.21
pycryptodomex         3.18.0
pydantic            1.10.13
Pygments            2.14.0
pyipmi             0.11.0
PyNaCl             1.5.0
pyperclip           1.8.2
Pypubsub            4.0.3
pyrsistent           0.19.3
pyserial            3.5
pytest             7.3.1
pytest-html          2.1.1
pytest-metadata        3.0.0
python-dateutil        2.8.2
python-ipmi          0.5.4
pytz              2023.3
pywin32            306
PyYAML             6.0
pyzmq             25.1.1
requests            2.28.2
retrying            1.3.4
rich              13.5.2
rich-click           1.6.1
robotframework         6.0.2
robotframework-ipmilibrary   0.3.5
robotframework-jsonlibrary   0.5
robotframework-requests    0.9.4
robotframework-ride      2.0
robotframework-robocop     4.1.0
robotframework-seriallibrary  0.4.3
robotframework-sshlibrary   3.8.1rc2.dev1
robotframework-tidy      4.5.0
scapy             2.4.3
scapy-helper          0.14.8
scp              0.14.5
setuptools           65.6.3
six              1.16.0
snowballstemmer        2.2.0
Sphinx             5.3.0
sphinxcontrib-applehelp    1.0.2
sphinxcontrib-devhelp     1.0.2
sphinxcontrib-htmlhelp     2.0.0
sphinxcontrib-jsmath      1.0.1
sphinxcontrib-qthelp      1.0.3
sphinxcontrib-serializinghtml 1.1.5
tabulate            0.8.10
textfsm            1.1.3
tmuxp             1.27.1
tomli             2.0.1
Twisted            22.10.0
twisted-iocpsupport      1.0.3
typing_extensions       4.7.1
urllib3            1.26.15
vncdotool           1.1.0
websocket-client        1.5.1
Werkzeug            2.2.3
wheel             0.38.4
wincertstore          0.2
wxPython            4.2.0
xlwt              1.3.0
XMind             1.2.0
xmind2testcase         1.5.0
xmindparser          1.0.9
xmltodict           0.13.0
zipp              3.15.0
zmq              0.0.0
zope.interface         6.0
```

## 2.更新RF包

RF包会根据脚本开发需要定期增加相关python库。

若已有RF包替换新RF包，建议删除目录C:\ProgramData\Miniconda3\envs中py375，从10.20.25.180中下载最新的（具体位置见第1步，下划线后日期越近越新）；

注：删除RF时，有可能报”文件被占用“错误，此时可重启PC或VDI，然后再删除，拷贝最新的。

## 3.修改环境配置文件

修改底层py配置文件，填入自己环境的服务器ip

找到py配置文件路径：

![image-20231221173733011](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231221173734.png)

修改env_test.py文件:

![image-20231221185028882](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231221185030.png)

![image-20231221173326567](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231221173327.png)

![image-20231221173329072](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231221173330.png)



# 2、执行用例

打开自动化用例文件夹：

![image-20231221180823551](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231221180824.png)

![image-20231221180607682](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231221180609.png)

综上所述，执行产品测试部的自动化测试用例需要搭建好产品测试部的对应的环境平台，在环境里搭建不同场景的业务环境，再执行相应的业务测试。



![image-20240104161801152](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240104161804.png)

需要在dpu bmc的root目录下创建一个switch_init.sh文件给N2上下电，如下：

```
i2cset -f -y 4 0x74 0x21 0xdf
sleep 2
i2cset -f -y 4 0x74 0x21 0xff
sleep 3
mdio-tool
```

![image-20240104161807605](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240104161811.png)

# 3、云盘搭建环境

云盘搭建环境有很多的坑，自动化用例会登录到DPU上，然后将dpu的/home目录下的镜像文件拷贝到`/data/host_disk_images`下，去拉起云盘的centos的host系统，具体拉起哪个需要看bm_config.json里的配置文件

首先，需要在/etc/sysconfig/network-scripts/目录下面为eno3口配置静态ip和网关等

然后配置网关

云盘搭建4net-4blk环境，先登录到dpu,删除一些配置和日志文件，将/home目录下的镜像文件拷贝到/data/host_disk_images/目录下，这样做的目的是，将一个纯净的镜像拷贝一份做一些配置，使之运用到云盘搭建host里

![image-20240116220830312](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240116220833.png)

/data/host_disk_images/内可能会有多个镜像文件，具体使用的是哪个镜像文件拉起的云盘host,需要进入到对应的bm_config.json文件内查看

![image-20240116220849637](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240116220850.png)

这里我们搭建云盘的配置文件是centos8.4-8g-ext4-host-dmar.img_bm

这里有个问题是，镜像centos8.4-8g-ext4-host-dmar.img_bm里的网口eno3与我们的环境的网口eno4不一样，因此我们在host服务器上将eno3连接网线到dpu上，可以实现host-dpu服务器的ip互通，在此之前我们需要将他们的镜像centos8.4-8g-ext4-host-dmar.img_bm在host上配置好ens3的ip为10.21.187.91/23，且将host的密码配置为本地host一样，既可以不修改配置文件host密码复用。