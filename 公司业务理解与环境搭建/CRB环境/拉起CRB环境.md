```
硬件要求：电源线，串口线，网线
1、串口线连接N2 BMC,给BMC配置ip和Mac地址
2、解压N2 BMC升级包，到BMC界面端传入文件升级
3、N2网络配置，配置N2 mac 并添加服务到开机自启动
4、CPLD查看版本，切换升级通道，并下载对应的升级包拷贝到BMC升级
5、L2Switch升级 （N2内） L2Switch版本只需要升级一次便可一劳永逸
6、SCP IMU固件升级
7、内核升级
```



## 搭建CRB环境

具体参考：[03-01-01-01 CRB 单板版本部署 - SW_TESTING_TEAM - Confluence (jaguarmicro.com)](http://space.jaguarmicro.com/pages/viewpage.action?pageId=93697020)

CRB启动文件 /usr/share/jmnd/single/auto/config/bm_config.json

```
vim /usr/share/jmnd/single/auto/config/bm_config.json

<disk type='nvme' device='disk' model='virtio'>
      <driver name='qemu' type='raw' queues='1'/>
      <source type='unix' path='/var/tmp/vnvme0'>
        <reconnect enabled='yes' timeout='10'/>
      </source>
      <target dev='vdg' bus='virtio'/>
    </disk>

<disk type='nvme' device='disk' model='virtio'>
<driver name='qemu' type='raw' queues='2'/>
<source type='unix' path='/var/tmp/vnvme1'>
<reconnect enabled='yes' timeout='10'/>
</source>
<target dev='vdg' bus='virtio'/>
</disk>
```

![image-20240104111808034](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240104111809.png)

## 1、CRB环境信息

![image-20231225204847269](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231225204848.png)

```
dpu：
BMC:10.21.168.68  root/0penBmc
N2:10.21.168.168  root/root

host:原148 
BMC:10.21.168.92 admin/admin
host:10.21.168.91 root/jaguar1
```

## 2、N2部署业务版本

```
cd /usr/share/jmnd/single/auto/script ;./kill_all.sh  # 杀掉原本的业务

echo 8192 > /sys/kernel/mm/hugepages/hugepages-2048kB/nr_hugepages
echo 8 > /sys/kernel/mm/hugepages/hugepages-1048576kB/nr_hugepages
echo 1 > /sys/module/vfio/parameters/enable_unsafe_noiommu_mode
modprobe vfio-pci enable_unsafe_noiommu_mode=1
export PATH=$PATH:/usr/share/jmnd/bin/ovs/images/bin

N2起不来：BMC上 mdio-tool
或者在bmc上对N2上下电，执行BMC上的switch.sh脚本，
或者在BMC上执行：
	ipmitool power off
	ipmitool power on
```

```
cd /usr/share/jmnd/single/auto/script;./start_all.sh # 启动N2上所有的业务进程

```

```
cd /usr/share/jmnd/single/auto/script;./start_all.sh
查看当前文件夹内的业务是否启动成功
/usr/share/jmnd/single/auto/script# ll 
```

![image-20231227190109851](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231227190111.png)

查看N2状态

```
qmp list
```

![image-20231227190202438](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231227190203.png)

查看N2的设备状态

```
ssh root@127.1 -p 6000
```

![image-20231227190232520](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231227190239.png)

## 3、host部署业务版本

登录BMC,重启host

```
host bmc操作"Powe reset"和"Power Cycle"
	ipmitool -I lanplus -H 10.21.187.92 -U admin -P admin power status # 查看host bmc状态
	ipmitool -I lanplus -H 10.21.187.92 -U admin -P admin power reset # 重启host bmc
	
重启后查看host pcie设备：
	lspci | grep -i virtio
```

![image-20231227190310500](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231227190311.png)

host加载blk,net驱动

```
modprobe virtio_blk
lsmod | grep virtio_blk # 查看驱动是否加载
# 卸载驱动 rmmod virtio_blk

modprobe virtio_net
lsmod | grep virtio_net # 查看驱动是否加载
# 卸载rmmod virtio_net

modprobe nvme
lsmod | grep nvme |grep -v nvme_core # 查看驱动是否加载
# 卸载rmmod nvme

lsmod | grep -q nvme |grep -v nvme_core; [ $? -eq 0 ] && { lsmod | grep -q nvme || echo 'unloaded successfully';} || echo 'Failed to unload module'

modprobe -r virtio_blk; [ $? -eq 0 ] && { lsmod | grep -q virtio_blk || echo 'unloaded successfully'; } || echo 'Failed to unload module'
 
modprobe -r virtio_blk; [ "$(lsmod | grep virtio_blk )" ] && echo 'Failed to unload module' || echo 'unloaded successfully'
modprobe -r virtio_net; [ "$(lsmod | grep virtio_net )" ] && echo 'Failed to unload module' || echo 'unloaded successfully'
modprobe -r nvme ; [ "$(lsmod | grep nvme | grep -v nvme_core)" ] && echo 'Failed to unload module' || echo 'unloaded successfully'

ls -lh /sys/block/nvme* |grep -E 'b7:00.0' |awk '{print $NF}'|awk -F '/' '{print $NF}'
```





## 问题1：重启业务失败

报错信息，巨页内存不够

![image-20231227203634384](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231227203636.png)



```
ll /data/host_disk_images
```

![image-20231227214552658](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231227214553.png)

这边可以看到，每次执行fio发流的时候，内存都会加一点，如果发流次数过多，很有可能会导致空间不够,从而fio发流失败

删除所有的日志

```
rm -r /var/tmp/*
```

![image-20240103153152848](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240103153156.png)

查看巨页内存是否满了

```
cat /proc/meminfo
```

![image-20231228155442432](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231228155445.png)



```
df -h # 查看内存
```

![image-20240102143017415](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240102143020.png)



目前只需要在N2的bmc上给N2下电和上电重启

```
DPU下电： i2cset -f -y 4 0x74 0x20 0xbf
DPU上电： i2cset -f -y 4 0x74 0x20 0xdf
连接L2 Switch:
	mdio-tool
```

重启之后巨页挂载都没有，可以重新启动start_all脚本重新拉起业务，巨页在脚本中制动挂载即可

此时巨页内存应该够用了，重新执行start_all脚本即可



## 手动执行DPU绑定pccu口

![image-20240110165534873](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240110165536.png)

1、登录dpu

2、dpu执行命令：

```
绑定前使用命令dpdk-devbind.py -s 选择对应的pcie设备号绑定驱动

dpdk-devbind.py -b vfio-pci 01:00.6 # 将pcie总线01:00.0连接到vfio-pci驱动上

ovs-vsctl del-port br-jmnd dpdk0 # 添加dpdk0网桥接口之前先删除原有的dpdk0网桥
ovs-vsctl add-port br-jmnd dpdk0 -- set Interface dpdk0 type=dpdk options:dpdk-devargs=01:00.6 options:n_rxq_desc=2048 options:n_txq_desc=2048 # 在ovs-vsctl虚拟交换机上的网桥br-jmnd上添加dpdk0端口，该端口通过01:00.0pcie总线连接到vfio-pci驱动
```

3、登录到imu串口`(B810版本后，pf的switchdev属性不需要测试人员手工切换了，dpdk驱动和内核驱动已经支持设置)`

首先在dpu的bmc上输入以下命令，跳转到不同的串口，这里我们需要再在imu串口上输入命令：

i2cset -f -y 4 0x74 0xbe 0x77

```
BMC SOL串口 —— 远程访问
i2cset -f -y 4 0x74 0xbe 0x73 # - SCP
i2cset -f -y 4 0x74 0xbe 0x77 # - IMU
i2cset -f -y 4 0x74 0xbe 0x7b # - N2
```

在另外的服务器上执行跳转串口的命令

```
ipmitool -I lanplus -H 10.21.186.68 -U root -P 0penBmc sol deactivate
ipmitool -I lanplus -H 10.21.186.68 -U root -P 0penBmc sol activate
```

输入以下命令将流量卸载到imu,

```
dpe_vnet_set pf 9 switch_mode 1  # 开启offload,卸载流量 
9 是pcie设备号在imu串口的映射
1 是开启流量卸载，imu串口只能有一个pcie号的流量卸载，当绑定到其他的串口时，需要先使用0关闭原本的流量卸载再绑定新的流量卸载。
```

![image-20240110171036957](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240110171038.png)

## pcie号与流量卸载的映射关系

将pcie号绑定的设备在imu串口卸载流量，其中pcie号与imu的映射关系如下：

1.先到N2上 lspci 查看12个pf的pci号

![img](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/wps1.jpg)

从左到右，依次是 bus 号 device号和 function号。黄色框内是bus号，目前固定是01；蓝色框内是device号；红色框内是function 号。

SFI 的计算是 device id * 8 + function id + 9，因此如图中 01:00:0 代表sfi 9 的pf。

如果希望知道具体某个口的pci号，可以ethtool -i xxx , 查看bus info 字段

![img](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/wps2.jpg)

## CRB环境更新固件

![image-20240112150423635](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240112150427.png)

1、首先获取到最新的固件版本（SCP和IMU），将其下载到VDI内，使用SCP拷贝到CRB BMC（10.21.186.68）的/tmp/文件夹内

2、使用md5sum命令解码，对比两个终端的文件是否正确

```
md5sum imu_flash_crb_5.img
md5sum scp_flash_crb_5_n2_2.0g_cmn_1.65g.img
```

![image-20240112151346992](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240112151348.png)

![image-20240112150750771](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240112150752.png)

3、在CRB bmc上使用命令升级固件

ssh登录BMC 在/tmp目录下执行如下命令（`注意，需要先升级SCP再升级IMU`）

```
firmware_tool.sh SCP /tmp/scp_flash_crb_5_n2_2.0g_cmn_1.65g.img  # SCP目录
firmware_tool.sh AP /tmp/crb_imu_flash_202312201634.img      # IMU目录
```

![image-20240112151659292](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240112151700.png)

之后对N2上下电

```
ipmitool power off
sleep 3
ipmitool power on
sleep 3
mdio-tool

ipmitool power off
ipmitool power on
```

## pccu口解绑pcie设备

pccu口绑定pcie设备的顺序是：首先将普通的Network device分配一个DPDK-compatible内核驱动，然后将分别配好的内核驱动用dpdk接管，如图所示：

```
dpdk-devbind.py -s
```

![image-20240223103918747](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240223103922.png)

但因为bdpk只能接管一个口，如

![image-20240223104034768](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240223104036.png)

则剩下的01:00.7口，是一个DPDK-compatible driver内核的口，可以直接使用命令解绑

```
dpdk-devbind.py -u 01:01.7
```

而01:00.7口，需要先与dpdk解绑变成DPDK-compatible driver内核的口，然后再使用`dpdk-devbind.py -u`删除成为普通的network device即可。
