# 一、PX2环境介绍



## 1、ETX环境

我们需要的PX2环境都是处在EXT环境内的，需要先进入EXT网站登录个人账号进入EXT

ETX网址:[Exceed TurboX Dashboard (jaguarmicro.hpc)](https://etxs05.jaguarmicro.hpc:8443/etx/?locale=en)

个人EXT账号：

```
用户名：wemxuan.qiu
密码：YbZn@2021
```

进入到个人ETX页面后，我们可以看到两个调试机启动窗口

首先我们先介绍EXT的两个操作系统窗口

![image-20231210113550025](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210113551.png)

## 2、Linux调试机

`linux调试机`是ext环境每个人自带的窗口、可以用来直接登录Linux服务器环境，如N2环境、host os/BMC环境、X2环境等

登录Linux调试机：

![image-20231210114135524](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210114136.png)

## 3、windows调试机

`windows调试机`是以小组的名义向上申请的调试机，之前我们组公用一台公共的windows调试机，目前已经申请了三台调试机。windows调试机里面可以运行处在PX2环境内的自动化测试用例代码、也可以在里面使用`windTerm远程登录Linux调试机`里的Linuxs服务器。

![image-20231210113927067](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210113928.png)

登录windows调试机：

```
10.1.70.162
cdns-edk
qwe123!@#
```

![image-20231204125031290](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231204125034.png)

# 二、拉起PX2环境

PX2环境拉起技术文档：[20231007-px2-8board环境-拉起8net-8blk业务场景步骤 - SW_TESTING_TEAM - Confluence (jaguarmicro.com)](http://space.jaguarmicro.com/pages/viewpage.action?pageId=82188423)

## 0、PX2服务器环境汇总

拉起EXT环境前，我们首先熟悉一下PX2环境所用到的服务器环境和功能：

在PX2环境下，需要用到的服务器有：

- emu3环境（X2启动环境）：执行X2（SCP/IMU/N2）拉起
- 三个串口调试机：SCP/IMU/N2
- N2串口调试机（也称N2端口）：为host端设备发流提供所需的设备和存储资源
- 跳板机1-GEM口（不稳定且该版本不通）：起到一个远程登录到N2的跳板的作用
- 跳板机2-PCCU口（稳定）：一般通过该口拷贝文件到N2、或使用方法一跳转到N2
- host OS端口：host端的Linux发流窗口，可以在host端提供设备并发流，需要N2端先启动vhost进程为host端提供服务
- host端的BMC窗口：只能在Linux调试机上登录，为浏览器上的界面端，作为host OS端口的启动、关闭的管理器

环境登录方式如图所示：

![image-20231210122401861](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210122403.png)

## 1、X2拉起

以8board环境拉起8net-8blk业务场景为例：

首先是X2拉起、即拉起SCP/IMU/N2环境。

首先需要在linux调试机登录到X2启动账号emu3环境，如下：

```
ssh -X emu3@t02n05 # 密码qwe123!@#
```

登录之后进入目录启动X2工程脚本：

```
cd /share/project/emulation/emu_prj/emu3/TAG_TOP_B600_V3_DSDV_20230323/soc/emu/st_fpga/ice_sys/ice_msoc_full_b23456789_2ep_b600v3_0705_newefuse
source source_me
make run_ice rundir=run_cfg_fw_itest
```

![image-20231210123042662](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210123043.png)

后端起单host，等一会儿出现**PTM>**的提示符，在该提示符下输入如下命令：

```
source ./run_cfg_fw_itest/reboot.tcl
且因为有bug，该命令执行第一次后imu会启动失败，需要执行第二次，第二次执行该命令后，imu才能正常启动
```

![image-20231206112801084](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231206112802.png)

接下来登录到串口调试机，imu串口打印如下信息表示命令执行第一遍失败，需要配重新在X2，PTM启动机上执行一遍命令

SCP串口打印机打印：

![image-20231210124556564](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210124557.png)

N2串口打印机打印：

第一遍启动失败

![image-20231206140937710](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231206140938.png)

第二遍：

![image-20231210124809610](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210124810.png)

N2串口打印机：

![image-20231210124852660](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210124853.png)

这里有个疑问，N2串口打印机就是N2本机、而SCP、IMU打印机的登录方式是什么呢？

## 2、配置N2 ip 和端口映射

### 1.配置PCCU可以跳转到N2

在N2串口配置PCCU跳板机的ip地址，执行如下命令相当于给N2自己配置ip地址为**`192.168.8.11`**

```
echo 2 > /sys/kernel/mm/hugepages/hugepages-1048576kB/nr_hugepages

mkdir -p /mnt/huge
mount none /mnt/huge -t hugetlbfs

echo 0000:01:00.0 > /sys/bus/pci/drivers/virtio-pci/unbind
echo 0000:01:00.1 > /sys/bus/pci/drivers/virtio-pci/unbind
echo 0000:01:00.2 > /sys/bus/pci/drivers/virtio-pci/unbind
echo 0000:01:00.3 > /sys/bus/pci/drivers/virtio-pci/unbind
echo 0000:01:00.4 > /sys/bus/pci/drivers/virtio-pci/unbind
echo 0000:01:00.5 > /sys/bus/pci/drivers/virtio-pci/unbind
echo 0000:01:00.6 > /sys/bus/pci/drivers/virtio-pci/unbind
echo 0000:01:00.7 > /sys/bus/pci/drivers/virtio-pci/unbind

cd /modules
insmod virtio_net.ko

insmod igb_uio.ko

echo 0x1af4 0x1041 > /sys/bus/pci/drivers/igb_uio/new_id

ip addr add 192.168.8.11/24 dev eth4
ip link set dev eth4 up
echo "1 4 1 7" > /proc/sys/kernel/printk
```

以上命令只能在N2串口机上手动敲命令、命令配置结束后，就可以通过登录到PCCU串口调试机上，再远程登录到N2端啦

```
ssh cdns-edk@10.1.66.25

ssh root@192.168.8.11
```

### 2.PCCU配置 -p 12022端口与N2互通

在跳板机10.1.66.25上，配置如下命令后，即可在，etx内直接ssh到N2，通过PCCU口。

将10.1.66.25的12022端口访问给转发到与10.1.66.25内部联通的192.168.8.11的22端口。

```
ssh -f -N cdns-edk@10.1.66.25 -L 10.1.66.25:12022:192.168.8.11:22
```

之后，即可在etx任意页面通过ssh [root@10.1.66.25](mailto:root@10.1.66.25) -p 12022 来访问N2，密码为root，示例如下：

```
ssh root@10.1.66.25 -p 12022   #密码root
```

执行以上操作相当于原本N2端口是在PCCU机上跳板访问的，但是将能直接访问的PCCU跳板机的12022端口与N2 ip的22端口实现映射，我们只需要访问PCCU的-p 12022端口就能直接跳转到N2的192.168.8.11:22ip地址



## 3、跳转机拷贝业务版本到N2

先将下载好的业务版本压缩包拷贝到跳板机10.1.66.25上，解压缩完成后，再拷贝到N2上，使用如下命令：（1015版本已下载放到10.1.66.25的/home/cdns-edk/cindy/corsica_dpu_6.0_corsica_1.0.5.1015_bugfix1025_jmnd.tar.gz）

```
cd /home/cdns-edk/cindy 

找到压缩包：/home/cdns-edk/cindy/corsica_dpu_6.0_corsica_1.0.5.1015_bugfix1025_jmnd.tar.gz
解压文件为jmnd在当前文件夹
```

进入到跳转机PCCU目录找到并解压业务版本文件，将版本文件拷贝到N2的run目录下面(这一步远程拷贝需要耗时1小时左右)

```
sshpass -p root scp -rp jmnd root@192.168.8.11:/run/
```

![image-20231206161114266](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231206161115.png)

跳转机上拷贝文件夹到N2侧如果出现问题，这里重新拷贝一遍业务版本文件

![image-20231206170809112](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231206170810.png)

再将版本从N2的/run/目录拷贝到/usr/share/jmnd下，使用如下命令：

```
rm -rf /usr/share/jmnd;mv /run/jmnd /usr/share/
```



打开N2端，进入run文件夹内，确认jmnd文件夹是否拷贝成功

![image-20231206161203304](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231206161204.png)

确认拷贝成功后，再将版本从N2的/run/目录拷贝到/usr/share/jmnd下，使用如下命令：

```
rm -rf /usr/share/jmnd;mv /run/jmnd /usr/share/
```

备注：N2上的/run/所在的存储介质IO读取速度较快，所以这里使用/run/目录做一个跳板。

## 4、N2上运行业务版本

### 0.修改spdk_env.json文件

将/usr/share/jmnd/config/spdk_cfg/spdk_env.json文件中的vhost_blk_num修改为8，vhost_nvme_num修改为0，与将要启动的8net-8blk场景匹配。

![image-20231210132022411](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210132023.png)

### 1.启动vdev进程

```
cd /usr/share/jmnd/single/debug_script/8net_8blk/   
./1_start_vdev.sh 200

重启进行如果超时：
rm -rf /var/run/jmnd_mgmt_server.pid
```

![image-20231210132514195](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210132515.png)

### 2.启动ovs进程

```
cd /usr/share/jmnd/single/debug_script/8net_8blk/   

./2_underlay_ovs.sh

若提示./2_underlay_ovs.sh: line 41: ovs-appctl: command not found ...
则需设置环境变量：
export PATH=$PATH:/usr/share/jmnd/bin/ovs/images/bin 
```

![image-20231206194756489](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210132441.png)

进程启动成功后，可以看到ovsdb-server及ovs-vswitchd两个进程正常运行：

```
ps -ef |grep ovs
```

![image-20231210132546742](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210132547.png)

### 3.启动spdk进程

spdk脚本的作用是启动N2端的vhost进程，生成设备存储资源并提供给host端

启动N2端的vhost进程，vhost端会生成存储资源并提供给host端设备使用。

```
cd /usr/share/jmnd/single/debug_script/8net_8blk/   

./3_start_spdk.sh
```

`当host端发流存在问题或者设备出现缺陷时，多半是N2端的vhost进程未启动，需要在N2端重启vhost进程。`

查看N2端vhost进程是否启动

```
ps -ef |grep vhost
```

查看start_jmnd_vhost.log日志，等待日志显示8个blk设备资源创建完成

```
tail -f /var/log/jmnd/start_jmnd_vhost.log # -f是实时打印日志，也可以使用tail -n 100打印后100行日志信息
```

![image-20231210145735191](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210145736.png)

### 4.启动hyper_commander进程

```
cd /usr/share/jmnd/single/debug_script/8net_8blk/   

./4_start_boot.sh
```

查看日志是否启动进程成功

```
cat /var/log/jmnd/jmnd_admin_default.log
```

![image-20231210150148288](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210150149.png)

当日志显示vm status RUNNING和rebot信息成功，重启host服务机

## 5、host端冷重启

打开linux调试机的火狐浏览器，登录host侧的BMC（host BMC相当于host OS的管理页面，可以控制host服务器的开关机等功能）

在网址上输入10.1.215.67 ip地址，登录用户名密码，都为admin

![image-20231210150518657](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210150519.png)

点击“远程控制”--“控制台重定向”--“启动H5Viewer”，在弹出的KVM窗口中，点击“电源”--“强制关机再开机”，之后等待host启动成功。

![image-20231210150433613](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210150434.png)

![image-20231210150731387](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210150732.png)

启动成功时，会在该窗口进入host OS界面

## 6、host侧加载驱动

host端重启之后，需要登录host服务器页面加载virtio驱动

### 1.关闭RC超时时间

在配置驱动之前，需要先关闭RC超时时间：

lspci -tv查看设备的PCIE号，如下截图所示，PCIE号为3e:02.0

```
lspci -tv
```

![image-20231210151121351](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210151122.png)

使用如下命令关闭RC超时时间：

```
setpci -s 3e:02.0 68.w=0099
```

使用如下命令查询确认RC超时时间已关闭：

```
spci -s 3e:02.0 -vvv |grep TimeoutDis
```

![image-20231210151227475](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210151228.png)

### 2.加载virtio驱动

内核加载virtio_net驱动

```
modprobe virtio_net

systemctl stop NetworkManager   #该命令一定要执行，否则会出现host的大网IP访问特别慢的问题，原因未知，目前发现情况如此
```

内核加载virtio_blk驱动

```
sysctl -w kernel.hung_task_panic=0   #关闭io hung超时panic

modprobe virtio_blk
```



# 三、进程重拉

如果host端设备和发流出现问题。或者N2端设备资源无法加载，我们可以先尝试半重拉环境。

## 1、杀掉jmnd进程、ovs进程



![image-20231211095442743](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231211095446.png)

杀掉jmnd、ovs进程的时候会出现僵尸进程，需要等待半小时左右僵尸进程自动释放为止

## 2、查看ovs僵尸进程

```
ps -ef| grep defunc
```

![image-20231211095517342](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231211095518.png)

## 3、待僵死进程释放，删除共享内存

```
ipcrm --all # 把整个共享内存删了
```

## 4、删除资源文件

```
rm -rf /dev/shm/*
```

## 5、重新在N2上运行业务版本，执行4.1、4.2、4.3、4.4步骤

### 1.启动vdev进程

```
cd /usr/share/jmnd/single/debug_script/8net_8blk/   
./1_start_vdev.sh 200

重启进行如果超时：
rm -rf /var/run/jmnd_mgmt_server.pid
```

![image-20231210132514195](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210132515.png)

### 2.启动ovs进程

```
cd /usr/share/jmnd/single/debug_script/8net_8blk/   

./2_underlay_ovs.sh

若提示./2_underlay_ovs.sh: line 41: ovs-appctl: command not found ...
则需设置环境变量：
export PATH=$PATH:/usr/share/jmnd/bin/ovs/images/bin 
```

![image-20231206194756489](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210132441.png)

进程启动成功后，可以看到ovsdb-server及ovs-vswitchd两个进程正常运行：

```
ps -ef |grep ovs
```

![image-20231210132546742](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210132547.png)

### 3.启动spdk进程

spdk脚本的作用是启动N2端的vhost进程，生成设备存储资源并提供给host端

启动N2端的vhost进程，vhost端会生成存储资源并提供给host端设备使用。

```
cd /usr/share/jmnd/single/debug_script/8net_8blk/   

./3_start_spdk.sh
```

`当host端发流存在问题或者设备出现缺陷时，多半是N2端的vhost进程未启动，需要在N2端重启vhost进程。`

查看N2端vhost进程是否启动

```
ps -ef |grep vhost
```

查看start_jmnd_vhost.log日志，等待日志显示8个blk设备资源创建完成

```
tail -f /var/log/jmnd/start_jmnd_vhost.log # -f是实时打印日志，也可以使用tail -n 100打印后100行日志信息
```

![image-20231210145735191](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210145736.png)

### 4.启动hyper_commander进程

```
cd /usr/share/jmnd/single/debug_script/8net_8blk/   

./4_start_boot.sh
```

查看日志是否启动进程成功

```
cat /var/log/jmnd/jmnd_admin_default.log
```

![image-20231210150148288](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210150149.png)

当日志显示vm status RUNNING和rebot信息成功，重启host服务机

# 四、PX2业务测试



## 1、host reset，加载virtio-blk驱动，FIO流通

业务测试流程：

![image-20231210153221169](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210153222.png)

自动化测试用例执行命令：

```
pytest test_blkdev.py::TestLegacyBlkDev::test_blkpf_modern_multidev_io --env-type=EMU --env-sub-type=all -vs

pytest test_blkdev.py::TestLegacyBlkDev::test_netpf_legacy_multidev_host_host_lsp --env-type=EMU --env-sub-type=all -vs
pytest test_netdev.py::TestModernNetDev::test_netpf_modern_driver_ok --env-type=EMU --env-sub-type=all -vs      
test_blkpf_modern_multidev_io

test_blkdev.py下的test_blkpf_legacy_driver_ok、test_blkpf_legacy_multidev_io、test_blkpf_legacy_multidev_reload_driver、test_blkpf_legacy_multidev_hotplug
test_netdev.py下的test_netpf_legacy_driver_ok、test_netpf_legacy_multidev_host_host_lsp

pytest 
test_blkdev.py::TestLegacyBlkDev::test_blkpf_legacy_driver_ok test_blkdev.py::TestLegacyBlkDev::test_blkpf_legacy_multidev_io 
test_blkdev.py::TestLegacyBlkDev::test_blkpf_legacy_multidev_reload_driver 
test_blkdev.py::TestLegacyBlkDev::test_blkpf_legacy_multidev_hotplug 

test_blkdev.py::TestModernBlkDev::test_blkpf_modern_driver_ok 
test_blkdev.py::TestModernBlkDev::test_blkpf_modern_multidev_io 
test_blkdev.py::TestModernBlkDev::test_blkpf_modern_multidev_reload_driver 
test_blkdev.py::TestModernBlkDev::test_blkpf_modern_multidev_hotplug 

test_blkdev.py::TestPackedBlkDev::test_blkpf_packed_driver_ok 
test_blkdev.py::TestPackedBlkDev::test_blkpf_packed_multidev_io 
test_blkdev.py::TestPackedBlkDev::test_blkpf_packed_multidev_reload_driver 
test_blkdev.py::TestPackedBlkDev::test_blkpf_packed_multidev_hotplug 


pytest test_netdev.py::TestLegacyNetDev::test_netpf_legacy_multidev_host_host_lsp --env-type=EMU --env-sub-type=all -vs

test_netdev.py::TestModernNetDev::test_netpf_modern_driver_ok 
test_netdev.py::TestModernNetDev::test_netpf_modern_multidev_host_host_lsp 

test_netdev.py::TestPackedNetDev::test_netpf_packed_driver_ok 
test_netdev.py::TestPackedNetDev::test_netpf_packed_multidev_host_host_lsp 
--env-type=EMU --env-sub-type=all -vs
```



### 1.host BMC冷重启reset

在etx的Linux服务器上登录host bmc

![image-20231204164625437](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231204164626.png)

![image-20231204165314607](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231204165315.png)

![image-20231210150731387](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210150732.png)

![image-20231204171154866](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231204171156.png)

等待界面端窗口登录成功

也可以直接在windows调试机上以命令行方式执行host冷重启(只有windows上有ipmitool工具)

```
ipmitool -I lanplus -H 10.1.251.67 -U admin -P admin power status # 查看Power状态

ipmitool -I lanplus -H 10.1.251.67 -U admin -P admin power reset # 重启reset power
```

![image-20231204164949073](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231204164950.png)

当ping通host os的ip地址,说明host os重启成功

```
ping 10.1.66.67
```

![image-20231204165731610](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231204165732.png)

### 2.关闭RC超时时间

在配置驱动之前，需要先关闭RC超时时间：

lspci -tv查看设备的PCIE号，如下截图所示，PCIE号为3e:02.0

```
lspci -tv
```

![image-20231210151121351](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210151122.png)

使用如下命令关闭RC超时时间：

```
setpci -s 3e:02.0 68.w=0099
```

使用如下命令查询确认RC超时时间已关闭：

```
spci -s 3e:02.0 -vvv |grep TimeoutDis
```

![image-20231210151227475](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210151228.png)

### 3.加载virtio驱动

内核加载virtio_net驱动

```
modprobe virtio_net

systemctl stop NetworkManager   #该命令一定要执行，否则会出现host的大网IP访问特别慢的问题，原因未知，目前发现情况如此
```

内核加载virtio_blk驱动

```
sysctl -w kernel.hung_task_panic=0   #关闭io hung超时panic

modprobe virtio_blk
```

### 4.fio发流

host端修改fio_test.cfg发流文件

```
vim fio_test.cfg
```

将host端需要发流的设备写入fio_test.cfg模板文件

![image-20231204172034544](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231204172035.png)

执行命令给脚本传参，fio开始发流

```
IOENGINE=libaio IODEPTH=64 NUMJOBS=1 BSRANGE=512B-1024k RW=randrw     RWMIXREAD=50 RUNTIME=3600 VERIFY=crc32 fio /root/fio_test.cfg --eta-newline=1     --output=/root/fio_test_output.txt
```

host端发流成功：

![image-20231204172325910](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231204172327.png)

![image-20231204181450217](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231204181451.png)



## 2、virtio_blk设备热插拔，FIO流通

业务测试流程：

![image-20231210155246129](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210155247.png)

自动化测试用例执行命令：

```
pytest test_blkdev.py::TestLegacyBlkDev::test_blkpf_legacy_multidev_hotplug --env-type=EMU --env-sub-type=all -vs
```

### 1.host端查看vdx设备正常

可以在host端输入lsblk查看设备的拓扑图

```
lsblk
```

![image-20231204172814014](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231204172815.png)

也可以使用命令查看设备文件视图，推荐使用方法2，可以查看设备的bdf号

```
ls -l /sys/class/block/vd* # 使用命令查看设备和bdf号
```

![image-20231210155707389](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210155708.png)

### 2.查看N2设备，挑选满足后端virtio设备类型的设备

登录到N2侧，进入6000端口调试模式，查看设备信息

```
ssh root@127.0.0.1 -p 6000 # 进入调试机模式，密码jmnd
```

输入命令,查看设备列表

```
m
device list
```

![image-20231204215134837](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231204215136.png)

找到对应的vdx设备的guest_features(16进制)的id,将其转化为2进制模式，判断设备是否满足对应virtio设备类型

判断条件如下：

```
legacy virtio设备类型：
vm_name=easy_bm，pci_dev_type=PF,dev_type=VIRTIO_BLK,guest_features的第34位bit的值为0,第32位的值为0

modern virtio设备类型：
vm_name=easy_bm,pci_dev_type=PF,dev_type=VIRTIO_BLK,grest_features的第34位bit的值为0,第32位的值为1

packed virtio设备类型：
vm_name=easy_bm，pci_dev_type=PF,dev_type=VIRTIO_BLK,grest_features的第34位bit的值为1
```

挑选出所有满足legacy virtio发流设备，随机选择一个设备如vdh（32位为0,34位也为0）

```
vdh			0000000010001646		0000 0001 0000 0000 0000 0001 0110 0100 0110
```

### 3.生成执行热插拔xml脚本

找到easy_bm.xml脚本，找到对应vdx设备的xml`<disk>`内设备信息

```
cd /usr/share/imnd/single/debug_script/8net_8blk
cat easy_bm.xml
```

![image-20231204175059607](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231204175100.png)

![image-20231204175159302](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231204175200.png)

我们如上找到的满足virtio设备名为vdh,将easy_bm.xml内的vdh设备的<disk>...</disk>内容添加在新的xml文档内，如blk_vdh.xml(建议放在N2的/root/目录下)

```
<disk type='vhostuser' device='disk' model='virtio'>
      <driver name='qemu' type='raw' queues='4'/>
      <source type='unix' path='/var/tmp/vhost.8'>
        <reconnect enabled='yes' timeout='10'/>
      </source>
      <target dev='vdh' bus='virtio'/>
    </disk>
```

![image-20231204175519190](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231204175520.png)

### 4.执行热拔插命令

先热拔再热插

```
qmp detach-device easy_bm blk7.xml # 热拔
qmp attach-device easy_bm blk7.xml # 热插
```

![image-20231210161933584](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210161934.png)

### 5.对热拔插的单个设备使用fio发流

host端修改fio_test.cfg发流文件

```
vim fio_test.cfg
```

将host端需要发流的vdh设备写入fio_test.cfg模板文件(图片为多个发流图片，仅供参考)

![image-20231204172034544](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231204172035.png)

执行命令给脚本传参，fio开始发流

```
IOENGINE=libaio IODEPTH=64 NUMJOBS=1 BSRANGE=512B-1024k RW=randrw     RWMIXREAD=50 RUNTIME=3600 VERIFY=crc32 fio /root/fio_test.cfg --eta-newline=1     --output=/root/fio_test_output.txt
```

host端发流成功：

![image-20231204172325910](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231204172327.png)



## 3、FIO带流kill -9杀掉vhost进程再拉起，FIO恢复

```
pytest test_blkdev.py::TestLegacyBlkDev::test_blkpf_legacy_multidev_io_forcekill_vhost test_blkdev.py::TestModernBlkDev::test_blkpf_modern_multidev_io_forcekill_vhost test_blkdev.py::TestPackedBlkDev::test_blkpf_packed_multidev_io_forcekill_vhost --env-type=CRB --env-sub-type=all -vs


1.fio持续发流
2.N2上kill -9 vhost进程
	ps aux |grep 'jmnd_vhost'|grep -v grep| awk '{print $2}' |xargs kill -9
3.查看vhost进程是否杀死
	ps aux |grep 'jmnd_vhost'|grep -v grep| awk '{print $2}' |wc -l
4.启动vhost进程
	cd /usr/share/jmnd/single/debug_script/8net_8blk;./3_start_spdk.sh
5.查看vhost进程是否存在
	ps aux |grep 'jmnd_vhost'|grep -v grep| awk '{print $2}' |wc -l
6.查看fio流是否已恢复
	tmux capture-pane -S -20 -t tmux_fio_test && tmux show-buffer
	
	
	
查看fio进程命令：
	ps aux |grep 'fio /root/fio_test.cfg --eta-newline=1 --output=/root/fio_test_output.txt'|grep -v grep | awk '{print $2}'
host杀掉fio的进程命令：
	ps aux |grep 'fio /root/fio_test.cfg --eta-newline=1 --output=/root/fio_test_output.txt'|grep -v grep | awk '{print $2}' |xargs kill -15
检查fio发流命令：
	ps aux |grep 'fio /root/fio_test.cfg --eta-newline=1 --output=/root/fio_test_output.txt'|grep -v grep | awk '{print $2}'
```

业务测试流程：

![image-20231210162355872](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210162357.png)

自动化测试用例命令：

```
pytest test_blkdev.py::TestLegacyBlkDev::test_blkpf_legacy_multidev_io_forcekill_vhost --env-type=EMU --env-sub-type=all -vs
```

### 1.先fio全部设备持续发流

host端修改fio_test.cfg发流文件

```
vim fio_test.cfg
```

将host端需要发流的设备写入fio_test.cfg模板文件

![image-20231204172034544](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231204172035.png)

执行命令给脚本传参，fio开始发流

```
IOENGINE=libaio IODEPTH=64 NUMJOBS=1 BSRANGE=512B-1024k RW=randrw     RWMIXREAD=50 RUNTIME=3600 VERIFY=crc32 fio /root/fio_test.cfg --eta-newline=1     --output=/root/fio_test_output.txt
```

host端发流成功：

![image-20231204172325910](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231204172327.png)

![image-20231204181450217](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231204181451.png)

### 2.N2端杀死vhost进程

```
查看进程启动：
ps -ef |grep vhost

杀掉vhost进程：
ps aux |grep 'jmnd_vhost'|grep -v grep| awk '{print $2}' |xargs kill -15
```

此时host端fio发流失败

![image-20231204181450217](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231204181451.png)

### 3.N2端重拉vhost进程

```
cd /usr/share/jmnd/single/debug_script/8net_8blk;./3_start_spdk.sh

查看资源生成日志
tail -f /var/log/jmnd/start_jmnd_vhost.log
```

查看N2端vhost进程是否启动

```
ps -ef |grep vhost
```

查看start_jmnd_vhost.log日志，等待日志显示8个blk设备资源创建完成

```
tail -f /var/log/jmnd/start_jmnd_vhost.log # -f是实时打印日志，也可以使用tail -n 100打印后100行日志信息
```

![image-20231210145735191](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210145736.png)

### 4.检查N2 dev list设备状态是否driver ok

登录到N2侧，进入6000端口调试模式，查看设备信息

```
ssh root@127.0.0.1 -p 6000 # 进入调试机模式，密码jmnd
```

输入命令,查看设备列表

```
m
device list
```

![image-20231204215134837](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231204215136.png)

### 5.查看host端是否生成8个设备

可以在host端输入lsblk查看设备的拓扑图

```
lsblk
```

![image-20231204172814014](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231204172815.png)

也可以使用命令查看设备文件视图，推荐使用方法2，可以查看设备的bdf号

```
ls -l /sys/class/block/vd* # 使用命令查看设备和bdf号
```

![image-20231210155707389](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231210155708.png)

### 6.查看fio发流是否已恢复

手动测试需可以在host端fio发流界面查看发流是否已恢复

![image-20231204181450217](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231204181451.png)

自动化测试会调用fio发流函数自动创建tmux窗口进行fio发流，需查看tmux窗口是否生成，fio界面是否发流

![image-20231204181310154](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231204181311.png)

## 4、Net设备业务：多设备多队列慢速路径流通(host-host)

[Trex 发流测试记录 - Vinny Yang - Confluence (jaguarmicro.com)](http://space.jaguarmicro.com/pages/viewpage.action?pageId=84992135)

```
host侧
    ls -l /sys/class/net # 查看host端的所有网络设备
    ip addr show # 展示所有的网口

N2侧
    ssh root@127.1 -p 6000
    ovs-vsctl show # 查看dpdk内的网桥
    如果执行以上命令出现 command not found（环境变量未配置），则需要将ovs所在目录移动到配置好的环境变量目录下，命令如下：
	mv /usr/share/jmnd/bin/ovs/images/bin/ /usr/games/
    
    清流表
    
    ovs-ofctl del-flows br-jmnd                      #清慢表
    ovs-appctl revalidator/purge                    #清快表
ovs-ofctl del-flows br-jmnd;ovs-appctl revalidator/purge
    
    ovs-ofctl add-flow br-jmnd nw_src=185.233.190.2,dl_type=0x0800,in_port=bm_itest_hostnet3,action=output:bm_itest_hostnet2 # 添加流表
    ovs-ofctl dump-flows br-jmnd                    #查看慢表配置
    ovs-appctl dpctl/dump-flows -m                  #查看快表配置
ovs-ofctl dump-flows br-jmnd;ovs-appctl dpctl/dump-flows -m |grep offloaded:yes |grep -v drop # 查看快慢表是否含有生效开启offloaded的流表（是否含有offloaded:yes开启，且过滤掉被丢弃的流表）
	
	
	
    
    配置trex发流文件，选中host上的两个端口发流
    cp /home/v2.93/cfg/simple_cfg.yaml /etc/trex_cfg.yaml;vim /etc/trex_cfg.yaml
   
   	启动trex_server
   	tmux kill-session -t tmux_trex_server 2>/dev/null;tmux new -s tmux_trex_server -d && tmux send -t tmux_trex_server "cd /home/v2.93/ && ./t-rex-64 -i --no-scapy-server" Enter
   	启动trex_console
   	tmux kill-session -t tmux_trex_console 2>/dev/null;tmux new -s tmux_trex_console -d && tmux send -t tmux_trex_console "cd /home/v2.93/ && ./trex-console -r" Enter
   	启动tcpdump抓包
   	tmux kill-session -t tmux_tcpdump;tmux new -s tmux_tcpdump -d && tmux send -t tmux_tcpdump "tcpdump -i bm_itest_hostnet2 -c 2 -xxx src 185.233.190.2" Enter
   	在trex_console里发包
   	tmux send -t tmux_trex_console "pkt -p ens -s Ether(dst='04:02:03:04:05:06',src='00:01:00:00:00:02')/IP(src='185.233.190.2',dst='172.0.1.1')" Enter"


```

1.首先使用命令查看host的ensx接口

```
ls -l /sys/class/net
```

选取两个host的ens接口用于发流

![image-20231220173708044](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231220173709.png)

1.找到host侧ensX与ovs端口的映射关系：

使用命令找到host端所有网络设备 ensX:pcie_num号 ，并使用命令ip addr show ensX展现所有网卡的mac_addr地址,将信息以字典的形式传入列表

```
ls -l /sys/class/net
ip addr show ensX

字典如下：
[{'dev_name': 'ens10', 'pcie_num': '0000:3e:02.0', 'mac_addr': '52:54:00:00:96:87'}, 
```

![image-20231212234318288](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231212234321.png)

![image-20231213110814360](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231213110815.png)

2.登录N2的6000端口获取N2的设备列表device list，通过pcie号将dev_name匹配到ens网口，将dev_name和guest_features获取到

![image-20231212234607729](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231212234608.png)

通过dev_name与网桥上的端口的iface匹配，得到网桥端口与ensX的映射

```
ovs-vsctl show
```

![image-20231212235115417](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231212235116.png)



​	5.查找到ovs_br上的端口的iface值和N2上的dev_name对应的source_path相等，即可找到网桥上的端口和host端的ensX网口的映射关系，当host端选择对应的ensX设备时，对应着N2上的ovs_br dbdk的net端口，我们需要将这两个端口配置发流

随机选择两个ensX设备，修改ymal发流文件，将ensX对应的ovs端口配置好流表，发流端口需要准确配置。

![image-20231213210930991](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231213210932.png)

4.启动trex对ensX发流

​	1.启动trex-sever

​	2.启动trex-console

​	3.trex-console发流

5.查看快表和慢表，慢表抓到一个包，快表生成了一个抓包记录

## 5、多设备多队列ctrlqueue反复修改active qp 

```

```

测试步骤：

![image-20231215170146385](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231215170147.png)

1.查看host端网络接口

```
ls -l /sys/class/net
```

![image-20231220173708044](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231220173709.png)

2.随机选择一个发流网络接口将他的状态变为up，并配置ip网关

```
ip link set dev ens16 up 

ip addr del 7.7.7.1/24 dev ens9 # 添加网管前需先删除原来的7.7.7.1网关的接口的配置
ip addr add 7.7.7.1/24 dev ens16 # 给端口添加ip网关7.7.7.1
```

3.查看网络接口是否配置成功

```
ip a # 查看所有的网络接口ip
```

![image-20231220173944752](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231220173946.png)

查看单个网络接口ip

```
ip addr show ensX # 查看单个网络接口的配置的ip地址
```

![image-20231220174333215](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231220174334.png)

4.查看host网络接口对应着的N2端的网桥的端口

```
进入N2查看host网络接口对应的网络设备
ovs-vsctl show
```

![image-20231220174515664](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231220174516.png)

![image-20231220175108036](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231220175109.png)

5.找到ensx网口对应的网桥端口后，人为在网桥上配置一个dpdk0端口

N2侧添加端口0000:01:00.1到br-ext，0000:01:00.1为pccu口，该口绑定macport0口

```
ovs-vsctl add-port br-ext dpdk0 -- set Interface dpdk0 type=dpdk options:dpdk-devargs=0000:01:00.1 options:n_txq=8 options:n_rxq=8 options:n_rxq_desc=2048 options:n_txq_desc=2048 ofport_request=1
使用命令查看端口是否配置成功：
ovs-vsctl show
```

![image-20231220175337489](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231220175338.png)

6.N2侧添加流表，macport0到ensX之前流量互通

```
首先清慢快表：
ovs-ofctl del-flows br-ext                          #清慢表
ovs-appctl revalidator/purge                        #清快表

配置流表：
ovs-ofctl add-flow br-ext in_port=dpdk0,action=output:netX
ovs-ofctl add-flow br-ext in_port=netX,action=output:dpdk0


```

7.测试仪接口配置7.7.7.2（该步骤目前在测试仪上手动添加，日后需要去调用接口实现自动化配置）

8.host侧ensX设备通过macport0口往测试仪打流（使用ping的方式，两个ip地址产生包交互）

```
ping 7.7.7.2 -c 3
```

9.host侧查询ensX设备的qp最大值及当前值，与N2侧设置的vnet的qp值对比

```
ethtool -l ensX
```

10.host侧修改ensX设备的active qp num为[1,2,3,4,5,6,7,8]中的任意一个值，假如是4

```
ethtool -L ensX combined 4
```

11.循环执行第8/9/10步100次

# 五、业务测试知识点

## 1、如何关闭host fio发流

当host fio发流阻塞时，如何关闭host fio发流

当发现blk设备fio发流失败且无法关闭时，首先查看设备的inflight值（及blk设备的待读写的请求数）

```
cat /sys/block/vd*/inflight
```

正常情况下，blk设备发流停止时，inflight值都是0，当blk设备损坏堵塞了请求，就会产生infligt值

如下所示为vda和vdb设备阻塞

![image-20231227175212683](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231227175214.png)

![image-20231227215900417](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231227215901.png)

一旦产生这种情况就需要让N2重新上电，重新拉N2上的业务

问题解释：



首先，查看虚拟盘内存

```
df -h
```

![image-20231227203750711](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231227203751.png)

可以看到盘内存满了，查看host_disk_images文件夹内存是否满

```
ll /data/host_disk_images
```

![image-20231227214552658](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231227214553.png)

这边可以看到，每次执行fio发流的时候，内存都会加一点，如果发流次数过多，很有可能会导致空间不够,从而fio发流失败

我们可以修改bm_config.json配置文件，将设备占用的内存修改小，可以避免内存空间不够用的情况`（fio发流失败，很大可能是因为这个原因）`

```
vim /usr/share/jmnd/single/auto/config/bm_config.json
```

![image-20231227215108053](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231227215109.png)



## 2、如何判断对应virtio设备属于什么类型

判断条件如下：

```
legacy virtio设备类型：
vm_name=easy_bm，pci_dev_type=PF,dev_type=VIRTIO_BLK,guest_features的第34位bit的值为0,第32位的值为0，status=DRIVER_OK

modern virtio设备类型：
vm_name=easy_bm,pci_dev_type=PF,dev_type=VIRTIO_BLK,grest_features的第34位bit的值为0,第32位的值为1，status=DRIVER_OK

packed virtio设备类型：
vm_name=easy_bm，pci_dev_type=PF,dev_type=VIRTIO_BLK,grest_features的第34位bit的值为1,status=DRIVER_OK
```



## 3、N2 bdf号和guest_features

![image-20231207102721162](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231207102722.png)

```
dev_name	guest_features			Hex
vda			
vdb			0000000510001646		0101 0001 0000 0000 0000 0001 0110 0100 0110
vdc			0000000510001646	      ^  ^
vdd			0000000510001646
vde			0000000510001646
vdf			0000000110001646
vdg			0000000110001646
vdh			0000000010001646		0000 0001 0000 0000 0000 0001 0110 0100 0110
vdj			0000000010001646		  ^  ^
```



## 4、host端bdf号和设备名

目前只有6个设备名和bdf号

![image-20231204174458629](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231204214642.png)





1.host-host 调通

|      |      |      |
| ---- | ---- | ---- |
|      |      |      |
|      |      |      |
|      |      |      |

