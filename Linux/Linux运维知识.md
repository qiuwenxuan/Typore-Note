# 一、云计算介绍

云厂商

![image-20231202122716557](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231202122719.png)



云计算总的来说就是提供虚拟化技术为中小厂提供网络服务器等资源

三种云模式：公有云、私有云、混合云

- IaaS:提供底层的基础设施，云服务器
- PaaS：平台服务器，面向开发人员，将软件编译成二进制交给云厂商
- Saas：软件及服务，如网站、邮件、博客，有专门的人员负责后期的维护

虚拟化：

![image-20231202144651618](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231202144652.png)

服务器类型

![image-20231202144529474](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231202144530.png)

- 塔式服务器：性能一般，体积庞大
- 刀片服务器：性能强，体积小，价格昂贵
- 机架服务器：体积中等，性能中等

# 二、Linux历史

Linux和unix历史

- unix:最早是开源且免费的，但是后面被收购开始收费，unix开始走下坡路
- Linux:开源且免费，可以二次开发，遵循GPL协议（GPL协议：通用的开源协议）

Linux内核介绍：

- 内核的作用：进程管理、内存、文件系统、设备控制、网络管理

![image-20231202150051447](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231202150052.png)

常见的Linux发行版本

![image-20231202150400172](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231202150401.png)

redhat红帽系列：

![image-20231202150510440](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231202150511.png)

# 三、Linux学习

## 1、命令提示符

![image-20231202215647828](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231202215649.png)

![image-20231202215613015](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231202215615.png)

-  ~为家目录，为当前用户的私有场所，其他用户没有权限直接访问 使用命令 `cd / cd ~`表示直接跳转到家目录
-  root的家目录为 /root
-  普通用户的家目录：/home/lisi 

![image-20231202222216118](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231202222217.png)

普通用户的提示词为$、管理员用户的提示符为#

![image-20231202223123075](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231202223124.png)