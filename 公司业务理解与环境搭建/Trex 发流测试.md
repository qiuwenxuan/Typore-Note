# Trex发流业务理解

参考链接：[Trex 发流测试记录 - Vinny Yang - Confluence (jaguarmicro.com)](http://space.jaguarmicro.com/pages/viewpage.action?pageId=84992135#Trex发流测试记录-4.uplink-host场景时trex测试)

![image-20231123114914958](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231123114917.png)

## 1、**host-host场景时trex测试**

首先我们来看一下最简单的Trex发流测试`host-host场景时trex测试`

![image-20231115143532048](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231115143532048.png)

存在两个虚拟机，应该是host端的VM,还有一个是N2端，现在我们对这几个端口都讲解

`host-VM (客户端虚拟机）`

在host-VM机里面user-space可以看成是用户自己的空间，存在两个网络端口ens4、ens5，Kernel是一个网桥，上面有两个端口virbr0、virbr1



`ARM N2 Clusters（ARM N2核心集群）`

在ARM N2 Clusters中，OVS&DPDK交换机硬件上存在一个网桥br-ext上面有四个端口：net_jmnd1、net_jmnd2、vnet0、vnet1,核心集群之外存在两个虚拟出来的真实网卡macif0、macif1

各个端口的映射关系

net_jmnd1通过virbr0绑定ens4，net_jmnd2通过virbr1绑定ens5

macport0代码中已默认绑定vnet0，macport1代码中应该也是已默认绑定vnet1





4.在N2上配置/etc/trex_cfg.yaml文件中interfaces的值为virtiouser0及virtiouser1，并UP两个端口
ifconfig virtiouser0 up
ifconfig virtiouser1 up
这两个口是我图中所指的这两个端口吗，这两个端口需要使用命令打开吗

![image-20231115153055439](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231115153055439.png)

解答：virtiouser0 和virtiouser1对应着新版本的macif0和macif1

![image-20231115155159615](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231115155159615.png)

1.host端-vm0获取virtio_net 的设备信息，并随机选出一个net 设备，包括dev_name,pcie_num,mac_addr。当中vm0是什么端口吗

![image-20231115153347701](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231115153347701.png)

解答：vm0并不是一个端口，而是host-VM虚机，在老版本可能一个用例涉及多个虚拟机，但现在只有一个虚拟机统称为VM
