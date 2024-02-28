准备好要导入的用例表格，如果是.xlsx格式的文件，需要另存为CSV格式并保存在本地目录

![image-20231024202057022](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231024202057022.png)

打开jira网址http://jira.jaguarmicro.com，选择我们需要导入的测试用例集，点击编辑

![image-20231024204143896](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231024204143896.png)

点击导入

![image-20231024204234014](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231024204234014.png)

对CVS文件选择对应的映射，点击导入

![image-20231024204443558](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231024204443558.png)

导入成功

![image-20231024204507616](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231024204507616.png)



测试用例导入jira常见问题：

1.打开cvs文件显示乱码

![image-20231024204938797](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231024204938797.png)

点击cvs文件，右键选择用notepad++打开

![image-20231024204801673](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231024204801673.png)

选择编码，转为UTF-8编码，点击保存。再次打开即可解除乱码

![image-20231024204841124](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231024204841124.png)

2.用例导入显示无效的键值、未收到必需输入的'Test Case'

![image-20231024205141773](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231024205141773.png)

首先检查自己的键值映射是否正确

![image-20231024205210777](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231024205210777.png)

其次，第一次导入的用例不需要引入”**问题键值**“字段，否则会引发上述问题

![image-20231024205353297](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231024205353297.png)

这里解释一下原理：

第一次导入用例时，”**问题键值**“字段不需要额外导入，jira系统会自动给用例添加的一个唯一标识的**问题键值**，当我们需要对用例进行变更覆盖时，才需要导入对应的**问题键值**覆盖相同问题键值的用例。适用于更新操作。

3.步骤和步骤ID的对应

执行步骤的每一步需要写在一个单独的单元格，并且步骤ID与步骤一一对应

![image-20231025093526054](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231025093526054.png)

导入的用例在Jira步骤上查看

![image-20231120232347093](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231120232348.png)