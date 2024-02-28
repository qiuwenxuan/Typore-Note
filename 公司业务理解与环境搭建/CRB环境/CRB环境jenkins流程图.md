## CRB环境门槛用例组网图

![image-20240122110030028](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240122110031.png)



## CRB环境jenkins流水线

![image-20240122105314652](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240122105315.png)

![image-20240122105145790](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240122105147.png)



```
jenkins流程业务理解：
1、我们在93服务器上拉起了6个虚拟机，每个虚拟机都是一个相对独立的环境，每个环境之间相对独立，互相隔离。
	1. 102 jenkins服务器
	2. 103 基础支撑用例运行服务器
	3. 105 P4用例运行服务器
	4. 106 RDMA用例运行服务器
	5. 107 基础支撑用例运行服务器
2、在102 jenkins服务器作为master主节点，用来跑Crosica_Daily_Build_New作为父Job,该父job的作用是：
	1. 清理工作目录并拉取自动化部署代码 (clean workspace & Pull Deploy Code)
		1.1 deletDir()
		1.2 git pull 自动化部署业务代码
	2. 部署基础支撑用例运行环境 (1.1 Basic Support Testing Deploy)
		2.1 SCP deploy
		2.2 IMU deploy
		2.3 reboot N2 (待定)	
	3. 构建基础支撑 job (1.2 Basic Support Testing) 
		3.1 基础支撑Job运行 (Base_Support_Testing)
			3.1.1 清理用例代码目录,并下载最新的基础支撑代码
			3.1.2 运行基础支撑用例
			3.1.3 集成allure报告
	4. 部署基础用例运行环境 (2.1 Clould BaseTestcase Tesing Deploy)
         4.1 SCP deploy
         4.2 IMU deploy
         4.3 部署业务版本
     5. 构建基础用例 Job (2.2 Clould BaseTestcase Tesing)
     	5.1 基础测试Job运行
     		5.1.1 清理用例代码目录，并下载最新的基础用例代码
     		5.1.2 跑基础测试用例
     		5.1.3 集成allure报告
     6. 构建产品测试 Job (3 Product_Testcase_Testing)
     	6.1 产品测试Job运行
            6.1.1 清理用例代码目录，并下载最新的产品测试用例代码
            6.1.2 跑产品测试用例代码
            6.1.3 集成allure报告
     7. 合成allure测试报告并统计测试用例结果
     	7.1 创建allure_result文件夹
     	7.2 拷贝基础支撑，基础用例，产品测试allure数据文件到allure_result对应的文件夹下
     	7.3 合并生成统一的测试报告并展现
     	7.4 统计基础支撑、基础用例、产品测试用例执行结果
```

```
workspace=/root/jaguar/jenkins/Crosica_Daily_Build_new

Crosica_Daily_Build_new
	my_workspace=/root/jaguar/jenkins/Crosica_Daily_Build_new
	
	
```



```
基础支撑：
workspace= /home/jaguar/jenkins/Corsica_Daily_Build_New/Base_Support_Testing
node= 基础支撑用例调试机107
allure_file= /home/jaguar/jenkins/Corsica_Daily_Build_New/Base_Support_Testing/base_support_testcase/output/allure
allure_report= /home/jaguar/jenkins/Corsica_Daily_Build_New/Base_Support_Testing/report/allure-report
基础用例：
workspace= /home/jaguar/jenkins/Corsica_Daily_Build_New/Clould_BaseTestcase_Tesing
node= 云支撑用例调试机103
allure_file= /home/jaguar/jenkins/Corsica_Daily_Build_New/Clould_BaseTestcase_Tesing/tests/pytestCase/report/result
allure_report= /home/jaguar/jenkins/Corsica_Daily_Build_New/Clould_BaseTestcase_Tesing/report/allure-report
junit_xml= tests/pytestCase/report/*.xml
产品测试：
workspace= /home/jaguar/jenkins/Corsica_Daily_Build_New/Product_Testcase_Testing
node= 云支撑用例调试机103
allure_file= /home/jaguar/jenkins/Product_Testcase_Testing/product_testing/output/allure
allure_report= /home/jaguar/jenkins/Product_Testcase_Testing/report/allure-report
```

