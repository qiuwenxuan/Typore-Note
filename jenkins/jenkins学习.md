jenkins官方文档：[Jenkins 用户手册](https://www.jenkins.io/zh/doc/)

## 一、CI/CD介绍

持续集成`CI`

![image-20240116173640416](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240116173644.png)

- 持续开发新功能
- 将新功能持续集成到产品主干里面去

持续交付`CD`

![image-20240116173742962](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240116173744.png)

- 持续集成后将软件交付给用户
- 支撑概念(自动化)
  - 自动化构建、自动化测试、自动发布
  - 块速，高效，容易回溯
  - 支撑平台jenkins

## 二、jenkins的安装和配置

参考链接：[20231114-Linux上安装Jenkins服务并展示allure报告 - SW_TESTING_TEAM - Confluence (jaguarmicro.com)](http://space.jaguarmicro.com/pages/viewpage.action?pageId=88477784&focusedCommentId=96979122#comment-96979122)

## 1、jenkins安装

jenkins是基于java环境运行的服务，至少需要java8环境，因此安装jenkins之前需要先下载java8环境

下载java环境

```
# 删除原有的java环境
sudo apt-get remove --purge openjdk-\*  
# 更新apt下载源
sudo apt-get update 

# 下载jdk8
apt install openjdk-8-jre-headless
apt install openjdk-8-jdk

# 查看安装是否成功
java -version
```



首先使用命令下载jenkins的war包，因为jenkins是基于java写的，因此需要先存在java环境

```
wget https://mirrors.jaguarmicro.com/jenkins/war/2.428/jenkins.war
```

jenkins下载好是一个war包，直接在war包所在的目录下使用java执行启动jenkins命令并指定端口8080

```
nohup java -jar jenkins.war --httpPort=8080 &
```

## 2、修改jenkins插件安装源

首先打开jenkins页面端

![image-20240121213125486](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240121213128.png)

![image-20240121213145129](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240121213146.png)

修改为公司的插件源

```
http://mirrors.jaguarmicro.com/jenkins/updates/update-center.json
```

然后打开.jenkins目录

修改 jenkins.model.JenkinsLocationConfiguration.xml 文件内的url为插件源

![image-20240121213333476](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240121213334.png)

![image-20240121213340126](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240121213341.png)

修改update目录下的default.json文件，将将https://[updates.jenkins.io](http://updates.jenkins.io/)/download全部替换为http://[mirrors.jaguarmicro.com](http://mirrors.jaguarmicro.com/)/jenkins

```
cd /root/.jenkins/updates
sed -i 's/https:\/\/updates.jenkins.io\/download/http:\/\/mirrors.jaguarmicro.com\/jenkins/g' default.json
```

重启jenkins页面端

然后重启Jenkins，重启后，后续就可以安装各种插件了。

![img](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240121213642.png)

## 3、jenkins job配置

## CICD持续集成，持续部署

![image-20240114204326710](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240114204330.png)

1. **持续集成（Continuous Integration，CI）：** 指的是多名开发者在开发不同功能代码的过程中，可以频繁的将代码合并到一起并互相不影响工作
2. **持续部署（Continuous Deployment，CD）：** 基于某种工具和平台实现代码的自动化的构建、测试和部署到线上环境以实现高质量交付



![image-20240110195016446](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240110195017.png)

## Pre steps前置脚本构建

## 钩子自动构建项目

## jenkins常见的构建触发器 

![image-20240114113259229](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240114113303.png)

![image-20240114113751835](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240114113753.png)

- 项目依赖自动构建（其他工程构建后触发）
- ![image-20240114114517976](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240114114519.png)
- 触发远程构建（远程api调用构建）
- ![image-20240114114536498](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240114114537.png)
- 父子job关联构建（当子job被构建时，父job应该先被构建）
- ![image-20240114114625987](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240114114627.png)
- 定时检查构建
- ![image-20240114115220219](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240114115221.png)

## cronitor表达式

```
cronitor表达式有关于五个*号,分别表示：
	分 小时 （每月当中的第几号） 月 （每周的星期几）
其中还包含一些特殊的符号：
	*（每个值）
	,（分隔符）
		如：30 6 15,30 * * ：每个月的第15号，第30号的6:30自动执行
	-（范围符）30 3 * * 0-6 ：表示每个星期的周一到周日的3点半都自动执行，也可以表示为1-7（0和7都表示周末）
	/（每间隔多少时间）/30 * * * * 表示每过30分钟制动构建一次
	
```

![image-20240114154607018](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240114154608.png)

与标准的cronitor表达式不同，jenkins在标准的表达式上加上了一个符号H，表示`分散负载`

H的底层通过将jobname计算它的Hash值得到一个绝对的时间值，当两个不同的jobname时间都为如 `30 3 * * *`，就很有可能导致两个job同时构建从而引发冲突，如果添加了H,就会避免在jenkins的峰值去构建项目，起到一个分散负载的作用

![image-20240114155948841](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240114155950.png)

你可以把H理解成在时间段的任意时间，这里注意，如：`H/10 * * * *` 表示每隔10分钟执行一次，但第一次时间是随机的，当第一次的随机时间确定之后，后面就会变成固定的每隔10分钟执行一次

![image-20240114161250236](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240114161251.png)

## 邮件配置发送



## jenkins集群和并发构建

集群化构建可以有效提升构建效率，尤其是团队项目或子项目比较多的时候，可以在多台机器上并发执行构建。

## 构建节点

![image-20240120003758546](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240120003802.png)

![image-20240120003804017](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240120003805.png)

![image-20240120003813733](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240120003815.png)

![image-20240120003820882](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240120003822.png)

![image-20240120003828275](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240120003829.png)

![image-20240120003836655](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240120003838.png)

## jenkins插件

### sshPublisher远程发送文件插件

[jenkins 插件【Publish Over SSH】的使用说明-CSDN博客](https://blog.csdn.net/qq_41788609/article/details/121830792#:~:text=【Publish Over SSH】插件是通过SSH连接其他Linux机器，远程传输文件及执行Shell命令有两种验证方式，密码方式和秘钥方式一、密码方式Manage Jenkins -> Configure System ->,authentication%2C or use a different key】使用密码登录，填写密码、端口、连接超时点击【T_publish over ssh)



## pipeline流水线

流水线语法：[流水线语法 (jenkins.io)](https://www.jenkins.io/zh/doc/book/pipeline/syntax/)

基本组成部分

```
pipeline:整条流水线
agent:节点
parameters:参数
stages:所有阶段
	stage:某一阶段
		steps：阶段内的每一步，可执行命令
		build(job: job_name, parameters: [string(name: 'my_workspace', value: params.my_workspace)])
post:构建后的操作
	always:无论如何都会运行
	success:成功时运行
	failure:失败时运行

```

jenkins pipeline语法执行shell命令需要在`sh"..."`内执行命令，但是每个sh之间并不关联，如果需要连续执行命令，可以使用`sh"""..."""`在内连续执行多条语句

```groovy
stage("执行构建"){
	steps{ // 使用""只能单个执行一行命令，且文件前后没有关联
		sh "cd demo-1 " // 在工作目录下进入到demo-1文件夹
		sh "mvn clean package" // 打包maven项目，由于每个sh""都是独立的命令，因此执行这个命令会在工作目录下，并没有在/demo-1目录下，需要使用""""""关联执行两条命令
	}
}

stage("执行构建"){
	steps{ // 使用"""连续执行多条命令"""
		sh """
		cd demo-1 
		mvn clean package
		"""
	}
}
```

### pipeline定义方式

1、Pipeline Script：直接把脚本内容写到脚本对话框中

2、Pipeline script from SCM ：Source Control Management–源代码控制管理

![image-20240123100337143](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240123100340.png)

### Pipeline Script 运行任务

脚本如下：

```groovy
pipeline{
    agent any
    stages{
        stage("first"){
            steps {
                echo 'hello world'
            }
        }
        stage("run test"){
            steps {
                echo 'run test'
            }
        }
    }
    post{
        always{
            echo 'always say goodbay'
        }
    }
}

```

脚本中定义了2个阶段（stage）：first和run test；post是jenkins完成构建动作之后需要做的事情。
**运行任务**，可以看到分为3个部分，如下图所示：

![image-20240123100536790](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240123100538.png)

### Pipeline script from SCM 通过代码库运行任务

将pipeline代码（Jenkinsfile）保存到代码库中，然后通过指定代码位置(脚本位置)的方式来运行pipeline任务。

1.创建Jenkinsfile，由Groovy语言实现。一般是存放在项目根目录，随项目一起受源代码管理软件控制。

![image-20240123100655750](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240123100657.png)

2.Jenkinsfile ：创建在根目录
脚本的第二stage 是执行pytestzwf文件下的test_json.py脚本
将项目提交到代码库。

3.在 job(任务)中配置Pipeline script from SCM

![image-20240123100750645](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240123100751.png)

![image-20240123100804808](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240123100806.png)

运行结果

![image-20240123100824707](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240123100825.png)

## jenkinsfile流水线语法

jenkinsfile流水线语法支持两种形式：

Declarative pipeline – 在pipeline v2.5 之后引入，结构化方式，比较简单，容易上手。这种类似于我们在做自动化测试时所接触的关键字驱动模式，只要理解其定义好的关键词，按要求填充数据即可。入门容易，但是灵活性欠缺。

Scripted pipeline – 基于groovy的语法，相较于Declarative，扩展性比较高，好封装，但是有些难度，需要一定的编程工具。

### 声明式流水线

声明式流水线是最近添加到 Jenkins 流水线的 [[1](https://www.jenkins.io/zh/doc/book/pipeline/syntax/#_footnotedef_1)]，它在流水线子系统之上提供了一种更简单，更有主见的语法。

所有有效的声明式流水线必须包含在一个 `pipeline` 块中, 比如:

```groovy
pipeline {
    /* insert Declarative Pipeline here */
}
```

一下所有的语法都是在声明式语法pipeline内

### ==agent(节点/代理)==

`agent` 部分指定了整个流水线或特定的部分, 将会在Jenkins环境中执行的位置，这取决于 `agent` 区域的位置。该部分必须在 `pipeline` 块的顶层被定义, 但是 stage 级别的使用是可选的。

**any**

在任何可用的代理上执行流水线或阶段。例如: `agent any`

**none**

当在 `pipeline` 块的顶部没有全局代理， 该参数将会被分配到整个流水线的运行中并且每个 `stage` 部分都需要包含他自己的 `agent` 部分。比如: `agent none`

**label**

在提供了标签的 Jenkins 环境中可用的代理上执行流水线或阶段。 例如: `agent { label 'my-defined-label' }`

**node**

`agent { node { label 'labelName' } }` 和 `agent { label 'labelName' }` 一样, 但是 `node` 允许额外的选项 (比如 `customWorkspace` )。

**docker**

使用给定的容器执行流水线或阶段。该容器将在预置的 [node](https://www.jenkins.io/zh/doc/book/pipeline/syntax/#../glossary#node)上，或在匹配可选定义的`label` 参数上，动态的供应来接受基于Docker的流水线。 `docker` 也可以选择的接受 `args` 参数，该参数可能包含直接传递到 `docker run` 调用的参数

```groovy
agent {
    docker {
        image 'maven:3-alpine'
        label 'my-defined-label'
        args  '-v /tmp:/tmp'
    }
}
```

**dockerfile**

执行流水线或阶段, 使用从源代码库包含的 `Dockerfile` 构建的容器

```groovy
agent {
    // Equivalent to "docker build -f Dockerfile.build --build-arg version=1.0.2 ./build/
    dockerfile {
        filename 'Dockerfile.build'
        dir 'build'
        label 'my-defined-label'
        additionalBuildArgs  '--build-arg version=1.0.2'
    }
}
```

**label**

一个字符串。该标签用于运行流水线或个别的 `stage`。

该选项对 `node`, `docker` 和 `dockerfile` 可用, `node`要求必须选择该选项。

**customWorkspace**

一个字符串。在自定义工作区运行应用了 `agent` 的流水线或个别的 `stage`, 而不是默认值。 它既可以是一个相对路径, 在这种情况下，自定义工作区会存在于节点工作区根目录下, 或者一个绝对路径。比如:

```groovy
agent {
    node {
        label 'my-defined-label'
        customWorkspace '/some/other/path'
    }
}
```

**reuseNode**

一个布尔值, 默认为false。 如果是true, 则在流水线的顶层指定的节点上运行该容器, 在同样的工作区, 而不是在一个全新的节点上。

这个选项对 `docker` 和 `dockerfile` 有用, 并且只有当 使用在个别的 `stage` 的 `agent` 上才会有效

```groovy
Jenkinsfile (Declarative Pipeline)
pipeline {
    agent none // 	在流水线顶层定义 agent none 确保 stege节点没有被分配
    stages {
        stage('Example Build') { // 	使用镜像在一个新建的容器中执行该阶段的该步骤
            agent { docker 'maven:3-alpine' } 
            steps {
                echo 'Hello, Maven'
                sh 'mvn --version'
            }
        }
        stage('Example Test') { // 使用一个与之前阶段不同的镜像在一个新建的容器中执行该阶段的该步骤
            agent { docker 'openjdk:8-jre' } 
            steps {
                echo 'Hello, JDK'
                sh 'java -version'
            }
        }
    }
}
```

### parameters（参数）

`parameters` 指令提供了一个用户在触发流水线时应该提供的参数列表。这些用户指定参数的值可通过 `params` 对象提供给流水线步骤

**string**

字符串类型的参数, 例如: `parameters { string(name: 'DEPLOY_ENV', defaultValue: 'staging', description: '') }`

**booleanParam**

布尔参数, 例如: `parameters { booleanParam(name: 'DEBUG_BUILD', defaultValue: true, description: '') }`

```groovy
pipeline {
    agent any
    parameters {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
    }
    stages {
        stage('Example') {
            steps {
                echo "Hello ${params.PERSON}"
            }
        }
    }
}
```

### stages(流水线)

包含一系列一个或多个 [stage](https://www.jenkins.io/zh/doc/book/pipeline/syntax/#stage) 指令, `stages` 部分是流水线描述的大部分"work" 的位置。 建议 `stages` 至少包含一个 [stage](https://www.jenkins.io/zh/doc/book/pipeline/syntax/#stage) 指令用于连续交付过程的每个离散部分,比如构建, 测试, 和部署。

```groovy
pipeline {
    agent any
    stages { 
        stage('Example') {
            steps {
                echo 'Hello World'
            }
        }
    }
}
```

#### stage

`stage` 指令在 `stages` 部分进行，应该包含一个 实际上, 流水巷所做的所有实际工作都将封装进一个或多个 `stage` 指令中。

```groovy
pipeline {
    agent any
    stages {
        stage('Example') {
            steps {
                echo 'Hello World'
            }
        }
    }
}
```

#### steps

`steps` 部分在给定的 `stage` 指令中执行的定义了一系列的一个或多个[steps](https://www.jenkins.io/zh/doc/book/pipeline/syntax/#declarative-steps)。

```groovy
pipeline {
    agent any
    stages {
        stage('Example') {
            steps { 
                echo 'Hello World'
            }
        }
    }
}
```

### options

`stage` 的 `options` 指令类似于流水线根目录上的 `options` 指令。然而， `stage` -级别 `options` 只能包括 `retry`, `timeout`, 或 `timestamps` 等步骤, 或与 `stage` 相关的声明式选项，如 `skipDefaultCheckout`。

在`stage`, `options` 指令中的步骤在进入 `agent` 之前被调用或在 `when` 条件出现时进行检查。

可用选项

- buildDiscarder

  为最近的流水线运行的特定数量保存组件和控制台输出。例如: `options { buildDiscarder(logRotator(numToKeepStr: '1')) }`

- disableConcurrentBuilds

  不允许同时执行流水线。 可被用来防止同时访问共享资源等。 例如: `options { disableConcurrentBuilds() }`

- overrideIndexTriggers

  允许覆盖分支索引触发器的默认处理。 如果分支索引触发器在多分支或组织标签中禁用, `options { overrideIndexTriggers(true) }` 将只允许它们用于促工作。否则, `options { overrideIndexTriggers(false) }` 只会禁用改作业的分支索引触发器。

- skipDefaultCheckout

  在`agent` 指令中，跳过从源代码控制中检出代码的默认情况。例如: `options { skipDefaultCheckout() }`

- skipStagesAfterUnstable

  一旦构建状态变得UNSTABLE，跳过该阶段。例如: `options { skipStagesAfterUnstable() }`

- checkoutToSubdirectory

  在工作空间的子目录中自动地执行源代码控制检出。例如: `options { checkoutToSubdirectory('foo') }`

- timeout

  设置流水线运行的超时时间, 在此之后，Jenkins将中止流水线。例如: `options { timeout(time: 1, unit: 'HOURS') }`

- retry

  在失败时, 重新尝试整个流水线的指定次数。 For example: `options { retry(3) }`

- timestamps

  预谋所有由流水线生成的控制台输出，与该流水线发出的时间一致。 例如: `options { timestamps() }`

重点理解一下**timeout**

```groovy
pipeline {
    agent any
    options {
        timeout(time: 1, unit: 'HOURS') // 超时时间，指定一个小时的全局执行超时, 在此之后，Jenkins 将中止流水线运行。

    }
    stages {
        stage('Example') {
            steps {
                echo 'Hello World'
            }
        }
    }
}
```



### input

`stage` 的 `input` 指令允许你使用 [`input` step](https://jenkins.io/doc/pipeline/steps/pipeline-input-step/#input-wait-for-interactive-input)提示输入。 在应用了 `options` 后，进入 `stage` 的 `agent` 或评估 `when` 条件前， `stage` 将暂停。 如果 `input` 被批准, `stage` 将会继续。 作为 `input` 提交的一部分的任何参数都将在环境中用于其他 `stage`。

```groovy
pipeline {
    agent any
    stages {
        stage('Example') {
            input { // 在这个阶段设置了一个输入步骤，这将导致构建过程暂停，等待用户输入确认。用户可以回答 "Yes, we should." 或者取消构建。
                message "Should we continue?" // 显示给用户的消息
                ok "Yes, we should." // 用户可以选择的确认消息
                submitter "alice,bob" // 允许哪些用户提交输入。在这个例子中，允许 "alice" 和 "bob" 提交
                parameters { //  定义输入参数
                    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                }
            }
            steps {
                echo "Hello, ${PERSON}, nice to meet you."
            }
        }
    }
}
```

当运行到 "Example" 阶段时，会等待用户输入确认消息。用户可以选择继续构建并提供一个名字参数，然后输出相应的问候消息。这是一个用于交互的流水线示例。

### ==when==

`when` 指令允许流水线根据给定的条件决定是否应该执行阶段。 `when` 指令必须包含至少一个条件。 如果 `when` 指令包含多个条件, 所有的子条件必须返回True，阶段才能执行。 这与子条件在 `allOf` 条件下嵌套的情况相同 (参见下面的[示例](https://www.jenkins.io/zh/doc/book/pipeline/syntax/#when-example))。

**branch**

当正在构建的分支与模式给定的分支匹配时，执行这个阶段, 例如: `when { branch 'master' }`。注意，这只适用于多分支流水线。

**environment**

当指定的环境变量是给定的值时，执行这个步骤, 例如: `when { environment name: 'DEPLOY_TO', value: 'production' }`

**expression**

当指定的Groovy表达式评估为true时，执行这个阶段, 例如: `when { expression { return params.DEBUG_BUILD } }`

**not**

当嵌套条件是错误时，执行这个阶段,必须包含一个条件，例如: `when { not { branch 'master' } }`

**allOf**

当所有的嵌套条件都正确时，执行这个阶段,必须包含至少一个条件，例如: `when { allOf { branch 'master'; environment name: 'DEPLOY_TO', value: 'production' } }`

**anyOf**

当至少有一个嵌套条件为真时，执行这个阶段,必须包含至少一个条件，例如: `when { anyOf { branch 'master'; branch 'staging' } }`

在进入 `stage` 的 `agent` 前评估 `when `

(这就是when在阶段stage的作用，会先评估判断，确保满足条件再去运行stage下的步骤)

默认情况下, 如果定义了某个阶段的代理，在进入该`stage` 的 `agent` 后该 `stage` 的 `when` 条件将会被评估。但是, 可以通过在 `when` 块中指定 `beforeAgent` 选项来更改此选项。 如果 `beforeAgent` 被设置为 `true`, 那么就会首先对 `when` 条件进行评估 , 并且只有在 `when` 条件验证为真时才会进入 `agent` 

```groovy
pipeline {
    agent any
    stages {
        stage('Example Build') {
            steps {
                echo 'Hello World'
            }
        }
        stage('Example Deploy') {
            when {
                branch 'production'
            }
            steps {
                echo 'Deploying'
            }
        }
    }
}
```

```groovy
pipeline {
    agent any
    stages {
        stage('Example Build') {
            steps {
                echo 'Hello World'
            }
        }
        stage('Example Deploy') {
            when {
                branch 'production'
                environment name: 'DEPLOY_TO', value: 'production'
            }
            steps {
                echo 'Deploying'
            }
        }
    }
}
```

```groovy
pipeline {
    agent any
    stages {
        stage('Example Build') {
            steps {
                echo 'Hello World'
            }
        }
        stage('Example Deploy') {
            when {
                allOf {
                    branch 'production'
                    environment name: 'DEPLOY_TO', value: 'production'
                }
            }
            steps {
                echo 'Deploying'
            }
        }
    }
}
```

```groovy
pipeline {
    agent any
    stages {
        stage('Example Build') {
            steps {
                echo 'Hello World'
            }
        }
        stage('Example Deploy') {
            when {
                branch 'production'
                anyOf {
                    environment name: 'DEPLOY_TO', value: 'production'
                    environment name: 'DEPLOY_TO', value: 'staging'
                }
            }
            steps {
                echo 'Deploying'
            }
        }
    }
}
```

```groovy
pipeline {
    agent any
    stages {
        stage('Example Build') {
            steps {
                echo 'Hello World'
            }
        }
        stage('Example Deploy') {
            when {
                expression { BRANCH_NAME ==~ /(production|staging)/ }
                anyOf {
                    environment name: 'DEPLOY_TO', value: 'production'
                    environment name: 'DEPLOY_TO', value: 'staging'
                }
            }
            steps {
                echo 'Deploying'
            }
        }
    }
}
```

```groovy
pipeline {
    agent none
    stages {
        stage('Example Build') {
            steps {
                echo 'Hello World'
            }
        }
        stage('Example Deploy') {
            agent {
                label "some-label"
            }
            when {
                beforeAgent true
                branch 'production'
            }
            steps {
                echo 'Deploying'
            }
        }
    }
}
```

### parallel(并行)

在Jenkins Pipeline DSL中，你可以使用`parallel`关键字来实现并行执行的语法

一个阶段必须只有一个 `steps` 或 `parallel` 的阶段

任何包含 `parallel` 的阶段不能包含 `agent` 或 `tools` 阶段, 因为他们没有相关 `steps`

另外, 通过添加 `failFast true` 到包含 `parallel`的 `stage` 中， 当其中一个进程失败时，你可以强制所有的 `parallel` 阶段都被终止。

```groovy
pipeline {
    agent any
    stages {
        stage('Parallel Stage') {
        	failFast true // 当一个stage失败，终止所有parallel并行的stage
            parallel { // 并行stage
                stage('Branch A') {
                    steps {
                        echo "Executing Branch A"
                        // Add your steps for Branch A here
                    }
                }
                stage('Branch B') {
                    steps {
                        echo "Executing Branch B"
                        // Add your steps for Branch B here
                    }
                }
                // You can add more branches as needed
            }
        }
        stage('Another Stage') {
            steps {
                echo "This stage runs after parallel execution."
                // Add steps for another stage here
            }
        }
    }
}
```



### ==triggers(触发器)==

`triggers` 指令定义了流水线被重新触发的自动化方法。对于集成了源（ 比如 GitHub 或 BitBucket）的流水线,可能不需要 `triggers`

 当前可用的触发器是 `cron`, `pollSCM` 和 `upstream`。

**cron**

接收 cron 样式的字符串来定义要重新触发流水线的常规间隔 ,比如: `triggers { cron('H */4 * * 1-5') }`

**pollSCM**

接收 cron 样式的字符串来定义一个固定的间隔，在这个间隔中，Jenkins 会检查新的源代码更新。如果存在更改, 流水线就会被重新触发。例如: `triggers { pollSCM('H */4 * * 1-5') }`

![image-20240123102708845](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240123102710.png)

**upstream**

接受逗号分隔的工作字符串和阈值。 当字符串中的任何作业以最小阈值结束时，流水线被重新触发。例如: `triggers { upstream(upstreamProjects: 'job1,job2', threshold: hudson.model.Result.SUCCESS) }`

```groovy
pipeline {
    agent any
    triggers {
        cron('H */4 * * 1-5') // 定时触发器
    }
    stages {
        stage('Example') {
            steps {
                echo 'Hello World'
            }
        }
    }
}
```

upstream样例

```groovy
triggers { // 当 'job1' 或 'job2' 的构建结果为成功时，重新触发当前流水线的执行。这样可以构建一种流水线之间的依赖关系，确保在上游作业构建成功时触发下游作业的执行
    upstream(upstreamProjects: 'job1,job2', threshold: hudson.model.Result.SUCCESS) 
}
```



### environment

[Jenkins Pipeline 10 环境变量使用指南_jenkins pipeline 环境变量-CSDN博客](https://blog.csdn.net/qq_34556414/article/details/117419824)

`environment` 指令制定一个 键-值对序列，该序列将被定义为所有步骤的环境变量，或者是特定于阶段的步骤， 这取决于 `environment` 指令在流水线内的位置。

- 顶层流水线块中使用的 `environment` 指令将适用于流水线中的所有步骤。
- 在一个 `stage` 中定义的 `environment` 指令只会将给定的环境变量应用于 `stage` 中的步骤。

```groovy
pipeline {
    agent any
    environment { // 顶层流水线块中使用的 environment 指令将适用于流水线中的所有步骤。
        CC = 'clang'
    }
    stages {
        stage('Example') {
            environment { // 在一个 stage 中定义的 environment 指令只会将给定的环境变量应用于 stage 中的步骤
                AN_ACCESS_KEY = credentials('my-prefined-secret-text') 
            }
            steps {
                sh 'printenv'
            }
        }
    }
}
```

在stages内需要使用`env.环境变量` 的形式去引用，但是在stages外，jenkins会自动解析这些变量，直接使用`${WORKSPACE}`等去调用即可

```groovy
pipeline {
    agent {
        node {
            //label 'master'
            label '云支撑用例调试机103'
            customWorkspace "${WORKSPACE}/${env.JOB_NAME}"
        }
    }
    stages {
        stage('Print Workspace') {
            steps {
                // 使用 Groovy 字符串插值输出 customWorkspace 的值
                echo "Custom Workspace is: ${env.WORKSPACE}/${env.JOB_NAME}"
                
                // 或者使用脚本步骤输出
                script {
                    def customWS = "${env.WORKSPACE}/${env.JOB_NAME}"
                    echo "Custom Workspace is: ${customWS}"
                }
            }
        }
    }
}

```

在上面的脚本中，`echo` 步骤用于打印 `customWorkspace` 的值。请注意，在 `customWorkspace` 属性中，`WORKSPACE` 和 `JOB_NAME` 前面不需要 `env` 前缀，因为这是在 agent 声明的上下文中，Jenkins 会自动解析这些变量。但是在 `steps` 部分中，你需要使用 `env.WORKSPACE` 和 `env.JOB_NAME` 来引用环境变量。

### tools(工具）

定义自动安装和放置 `PATH` 的工具的一部分。如果 `agent none` 指定，则忽略该操作。

```groovy
pipeline {
    agent any
    tools {
        maven 'apache-maven-3.0.1' 
    }
    stages {
        stage('Example') {
            steps {
                sh 'mvn --version'
            }
        }
    }
}
```

### post(结尾)

`post` 部分定义一个或多个[steps](https://www.jenkins.io/zh/doc/book/pipeline/syntax/#declarative-steps) ，这些阶段根据流水线或阶段的完成情况而 运行(取决于流水线中 `post` 部分的位置). `post` 支持以下 [post-condition](https://www.jenkins.io/zh/doc/book/pipeline/syntax/#post-conditions) 块中的其中之一: `always`, `changed`, `failure`, `success`, `unstable`, 和 `aborted`。这些条件块允许在 `post` 部分的步骤的执行取决于流水线或阶段的完成状态。

##### Conditions

- `always`

  无论流水线或阶段的完成状态如何，都允许在 `post` 部分运行该步骤。

- `changed`

  只有当前流水线或阶段的完成状态与它之前的运行不同时，才允许在 `post` 部分运行该步骤。

- `failure`

  只有当前流水线或阶段的完成状态为"failure"，才允许在 `post` 部分运行该步骤, 通常web UI是红色。

- `success`

  只有当前流水线或阶段的完成状态为"success"，才允许在 `post` 部分运行该步骤, 通常web UI是蓝色或绿色。

- `unstable`

  只有当前流水线或阶段的完成状态为"unstable"，才允许在 `post` 部分运行该步骤, 通常由于测试失败,代码违规等造成。通常web UI是黄色。

- `aborted`

  只有当前流水线或阶段的完成状态为"aborted"，才允许在 `post` 部分运行该步骤, 通常由于流水线被手动的aborted。通常web UI是灰色。

```groovy
pipeline {
    agent any
    stages {
        stage('Example') {
            steps {
                echo 'Hello World'
            }
        }
    }
    post { // 按照惯例, post 部分应该放在流水线的底部。
        always { 
            echo 'I will always say Hello again!'
        }
    }
}
```

### Script(脚本)

`script` 步骤需要 [[scripted-pipeline\]](https://www.jenkins.io/zh/doc/book/pipeline/syntax/#scripted-pipeline)块并在声明式流水线中执行。

```groovy
pipeline {
    agent any
    stages {
        stage('Example') {
            steps {
                echo 'Hello World'

                script {
                    def browsers = ['chrome', 'firefox']
                    for (int i = 0; i < browsers.size(); ++i) {
                        echo "Testing the ${browsers[i]} browser"
                    }
                }
            }
        }
    }
}
```

### 流控制

#### if-else表达式

```groovy
node {
    stage('Example') {
        if (env.BRANCH_NAME == 'master') {
            echo 'I only execute on the master branch'
        } else {
            echo 'I execute elsewhere'
        }
    }
}
```

#### try/catch/finally

```groovy
node {
    stage('Example') {
        try {
            sh 'exit 1'
        }
        catch (exc) {
            echo 'Something failed, I should sound the klaxons!'
            throw
        }
    }
}
```



## jenkins默认环境变量

Jenkins 中有多个默认的环境变量，它们可以在 Jenkins Pipeline 中使用。这些环境变量提供了有关 Jenkins 环境、构建过程和项目配置的信息。以下是一些常见的 Jenkins 环境变量：

- `BRANCH_NAME` - 对于多分支管道，这是当前分支的名称。
- `BUILD_DISPLAY_NAME` - 构建的显示名称，通常是 `#${BUILD_NUMBER}`。
- `BUILD_ID` - 构建的 ID。
- `BUILD_NUMBER` - 当前构建的序号。
- `BUILD_TAG` - 由 Jenkins 自动创建的构建标签。
- `BUILD_URL` - 构建的 URL。
- `CHANGE_ID` - 对于拉取请求，这是变更或拉取请求的 ID。
- `CHANGE_TARGET` - 对于拉取请求，这是目标分支的名称。
- `CHANGE_URL` - 对于拉取请求，这是变更的 URL。
- `CHANGE_TITLE` - 对于拉取请求，这是变更的标题。
- `CHANGE_AUTHOR` - 对于拉取请求，这是作者的名称。
- `CHANGE_AUTHOR_DISPLAY_NAME` - 对于拉取请求，这是作者的显示名称。
- `CHANGE_AUTHOR_EMAIL` - 对于拉取请求，这是作者的电子邮件。
- `EXECUTOR_NUMBER` - 执行器的编号。
- `GIT_COMMIT` - Git 插件提供的当前构建的提交哈希。
- `GIT_URL` - Git 仓库的 URL。
- `GIT_BRANCH` - 当前所在的分支。
- `JOB_BASE_NAME` - 作业的基本名称。
- `JOB_NAME` - 当前作业的名称。
- `JOB_URL` - 作业的 URL。
- `JENKINS_HOME` - Jenkins 主目录的路径。
- `JENKINS_URL` - Jenkins 服务器的 URL。
- `NODE_LABELS` - 执行构建的节点的标签。
- `NODE_NAME` - 执行构建的节点的名称。
- `WORKSPACE` - 当前作业工作目录的路径。
