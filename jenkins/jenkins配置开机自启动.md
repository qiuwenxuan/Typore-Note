# Jenkins配置开机自启动

# 1.首先在/home/jaguar下创建jenkins_start.sh脚本

## 1.1 创建脚本

```
cd /home/jaguar vi jenkins_start.sh
```

jenkins_start.sh中填入下面的内容：

```
curl -so /home/jaguar/agent.jar http://10.21.187.102:8080/jnlpJars/agent.jar		// -s是静默模式，-o是指定输出的文件路径 java -jar /home/jaguar/agent.jar -jnlpUrl http://10.21.187.102:8080/computer/10%2E21%2E187%2E103/jenkins-agent.jnlp -secret 72c25b2ac1f475f4cb943ed3c344d9878abefaa4cae8bfc15ad98cbf613840b7 -workDir "/home/jaguar/jenkins"
```

注意上面的内容不要照抄，因为每台机器的私钥和工作目录都不一样，上面的内容最好从Jenkins的控制台上获取

![image-20240226105139467](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240226105140.png)

如果是**master**节点的配置开机自启动，则jenkins_start.sh中填入下面的内容：

```
java -jar jenkins.war --httpPort=8080			// 启动jenkins服务器，注意这个只是在master节点上才配置，从节点不要有这行命令
```

后面的操作和从节点基本一致，只是**验证**的时候，从节点验证是否配置成功，是在Jenkins的控制台上看是否显示connected；而master是否配置成功，检查的是Jenkins能否在网页端访问。

## 1.2 执行脚本

```
cd /home/jaugar && chmod +x jenkins_start.sh	// 进入脚本所在目录并为脚本赋予可执行的权限 ./jenkins_start.sh								// 执行脚本
```

## 1.3 验证脚本正确性

首先查看从节点运行脚本后的打印消息：

![image-20240226105147802](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240226105149.png)

接着查看Jenkins控制台的节点连接情况：

![image-20240226105155567](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240226105156.png)

## 1.4 关闭脚本

**键入ctrl + c关闭脚本。脚本必须关闭，否则会影响下面的Linux开机自启动的服务配置**

# 2. 配置Linux开机自启动

## 2.1 创建service文件

```
cd /etc/systemd/system vi jenkins.service
```

## 2.2 编写service文件内容

```
[Unit] Description=Jenkins Server [Service] ExecStart=/bin/bash /home/jaguar/jenkins_start.sh WorkingDirectory=/home/jaguar User=root Restart=always [Install] WantedBy=multi-user.target
```

只要你是跟着教程一步一步做的，那么上面的文件照抄即可，否则根据需要修改ExecStart指定的脚本路径

## 2.3 使能和启动服务

上面的service文件保存后，在命令行中键入下面的命令

```
systemctl enable jenkins.service systemctl start jenkins.service
```

## 2.4 验证服务是否配置成功

![image-20240226105204678](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240226105206.png)

## 2.5 重启本机做最后验证

键入下面的命令，让主机重启。重启后到Jenkins控制台查看节点有没有自动连接，如果没有则说明配置失败，如果已连接则说明配置成功。

```
reboot			// ！！！重启本机，注意要保存自己的工作以及确认一下重启操作会不会影响到他人
```

重启中到Jenkins控制台查看节点的连接状态：

![image-20240226105224431](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240226105225.png)

重启完成后再次查看节点连接状态：

![image-20240226105231104](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240226105232.png)

大功告成！