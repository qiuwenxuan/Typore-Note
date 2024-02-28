# Linux上安装Jenkins并展示allure报告



# 1. 确认安装正确的java版本

到官网https://get.jenkins.io/war-stable/查看Jenkins版本匹配的java版本，我这里选择安装java11

使用java --version命令是否已安装java版本

```
java --version
```

![image-20240225111654424](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240225111655.png)

如上图所示，暂未安装java版本，我这里选择安装java11（jenkins是java项目，因此需要安装java环境）

```
apt install openjdk-11-jre-headless
```

安装成功后，再使用java --version命令确认版本



# 2. 下载Jenkins的安装war包

通过公司镜像源获取Jenkins的安装war包 

wget [https://mirrors.jaguarmicro.com/jenkins/war/2.428](https://mirrors.jaguarmicro.com/jenkins/war/2.428/)[/jenkins.war](https://mirrors.jaguarmicro.com/jenkins/war-stable/2.414.3/jenkins.war)

# 3. 启动Jenkins服务

在jenkins.war下载目录，后台直接启动jenkins服务，Linux服务器重启后，则需要重新执行该命令启动jenkins服务

```
nohup java -jar jenkins.war --httpPort=8080 &
```



# 4. 登录Jenkins的web界面进行初始化配置

浏览器中登录如下地址，即可访问Jenkins进行配置。http://linux机器IP地址:8080，本次安装的Linux的IP地址为10.1.70.149，所以登录如下地址即可。

[http://10.1.70.149:8080](http://10.1.70.149:8080/job/test_pipeline/build?delay=0sec)

第一次登录界面，根据提示到Linux服务器上获取密码。

![image-20240225112011614](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240225112012.png)

```
cat /root/.jenkins/secrets/initialAdminPassword
```

![image-20240225112030724](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240225112032.png)

因为本Linux服务器不通外网，只能联通公司镜像源，所以如下截图中选择跳过插件安装。

![image-20240225112051664](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240225112052.png)

创建第一个管理员用户，这里创建的用户名/密码为itest/qwe123!@#

![image-20240225112111003](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240225112112.png)

![image-20240225112127455](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240225112128.png)



# 5. 配置插件源为公司内部源

## 5.1 web界面先配置公司内部源

Dashboard→Manage Jenkins→Manage Plugins，找到Advanced settings，如下几个截图所示。

![image-20240225112155050](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240225112156.png)

![image-20240225112210165](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240225112211.png)

![image-20240225112227669](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240225112229.png)

下滑到更新源区域输入公司内的源的url： http://mirrors.jaguarmicro.com/jenkins/updates/update-center.json 并且点击提交

![image-20240225112248483](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240225112249.png)

## 5.2 Linux服务器上修改配置文件中的源地址

上述步骤操作一两分钟后，登录到Jenkins服务器，查看/root/.jenkins/目录下生成updates文件夹

![image-20240225112327580](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240225112328.png)

修改/root/.jenkins/updates下的 default.json文件，将https://[updates.jenkins.io](http://updates.jenkins.io)/download全部替换为http://[mirrors.jaguarmicro.com](http://mirrors.jaguarmicro.com)/jenkins

```
cd /root/.jenkins/updates sed -i 's/https:\/\/[updates.jenkins.io](http://updates.jenkins.io)\/download/http:\/\/[mirrors.jaguarmicro.com](http://mirrors.jaguarmicro.com)\/jenkins/g' default.json
```

然后重启Jenkins，重启后，后续就可以安装各种插件了。

![image-20240225112359834](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240225112401.png)

# 6. 安装需要的插件

## 6.1 安装中文显示插件

Dashboard→Manage Jenkins→Manage Plugins，找到Available plugins，在搜索框中搜索chinese，选中后点击 Install即可。

安装好后，重启Jenkins，页面即可显示中文。



## 6.2 安装pipeline相关插件

Dashboard-->系统管理-->插件管理→Available plugins，在搜索框中输入pipeline，在列出的相关插件中选择如下后，点击安装。

![image-20240225112455623](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240225112456.png)

安装好之后的效果如下，新建任务时，有如下流水线的选项。

![image-20240225112510578](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240225112511.png)

## 6.3 安装blue ocean插件

Dashboard-->系统管理-->插件管理→Available plugins，在搜索框中输入blue ocean，在列出的相关插件中选择如下后，点击安装。

![image-20240225112705336](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240225112706.png)

安装好之后的效果如下，Dashboard页面出现“打开 Blue Ocean”选项。

![image-20240225112724897](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240225112726.png)

## 6.4 安装pipeline stage view 插件

Dashboard-->系统管理-->插件管理→Available plugins，在搜索框中输入pipeline stage view，在列出的相关插件中选择如下后，点击安装。

![image-20240225112926772](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240225112928.png)

安装好之后的效果如下，在任务显示界面出现阶段视图的展示。

![image-20240225112935670](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240225112937.png)

## 6.5 安装maven和junit插件

Dashboard-->系统管理-->插件管理→Available plugins，在搜索框中输入junit，在列出的相关插件中选择如下后，点击安装。

![image-20240225112952098](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240225112953.png)

maven安装后的效果如下，新建任务时，有如下构建一个maven项目的选项。

![image-20240225113006245](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240225113007.png)

junit插件的效果暂时没有看到。

# 7. 配置 pub key

即生成测试服务器的ssh key，将其添加到git代码托管平台。这样jenkins流水线可以直接拉取代码了。



# 8. allure报告集成

## 8.1 allure插件安装

安装Allure Jenkins Plugin插件

![image-20240225204244541](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240225204245.png)

## 8.2 配置allure commandline

Dashboard-->系统管理→全局工具配置，选择Allure Commandline安装，输入节点的allure安装目录

![image-20240225204302847](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240225204304.png)

## 8.3 配置流水线显示allure报告

```
pipeline {
    parameters {
        string defaultValue: 'pytest', name: 'test_type'
    }
    
    //agent any
    agent { label 'windows调试机'}
    
    stages {
        stage('Build') {
            steps {
                echo "test_type: ${params.test_type}"
                echo "Build"
            }
        }
        stage('Deliver') {
            steps {
                echo "test_type: ${params.test_type}"
                echo "Deliver"
            }
        }
        stage('Pull Test Code') {
            steps {
                deleteDir()
                
                checkout scmGit(branches: [[name: '*/PX2-test']], extensions: [], userRemoteConfigs: [[url: 'ssh://git@bb.jaguarmicro.com:7999/product/sw_itest.git']])
            }
        }  
        stage('Test') {
            steps {
                echo "test_type: ${params.test_type}"
                echo "test"
                bat 'dir'
            }
        }
        stage('Test-Modern-Net-SIM') {
            steps {
                bat 'pytest -s tests/pytestCase/test_netdev.py::TestModernNetDev --env-type=SIM --env-sub-type=all -vs --alluredir=./report/result-modern-net --clean-alluredir'
            }
        }
        stage('Test-Modern-Blk-SIM') {
            steps {
                bat 'pytest -s tests/pytestCase/test_blkdev.py::TestModernBlkDev --env-type=SIM --env-sub-type=all -vs --alluredir=./report/result-modern-blk --clean-alluredir'
            }
        }
        stage('Test-NVMe-SIM') {
            steps {
                bat 'pytest -s tests/pytestCase/test_nvmedev.py::TestNVMeDev --env-type=SIM --env-sub-type=all -vs --alluredir=./report/result-nvme --clean-alluredir'
            }
        }
    }
    post('Results') { // 执行之后的操作
        always{
            script{// 集成allure，目录需要和保存的results保持一致，注意此处目录为job工作目录之后的目录，Jenkins会自动将根目录与path进行拼接
                allure includeProperties: false, jdk: '', report: 'report/allure-report', results: [[path: 'report/result-modern-net'],[path: 'report/result-modern-blk'],[path: 'report/result-nvme']]
            }
        }
    }
}
```

