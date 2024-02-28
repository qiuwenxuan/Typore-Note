## **tmux命令及快捷方式**

```
# 列出所有窗口
指  令：tmux ls
快捷键：Ctrl+b s

# 退出tmux窗口 
指  令：tmux detach
快捷键：Ctrl+b d

exit(关闭并杀死窗口)

# 新建tmux窗口
指  令：tmux new -s <name>

# 重命名会话
指  令：tmux rename-session -t <old-name> <new-name>
快捷键：Ctrl+b $

# 进入指定窗口
指  令：tmux attach -t <name>

# 切换会话
指  令：tmux switch -t <name>

# 后台创建会话
tmux new -s <name> -d -d表示在后台创建会话

tmux send:向指定-t tmux窗口发送命令
例：tmux send -t tmux_trex_server "cd /home/v2.93/ && ./t-rex-64 -i --no-scapy-server" Enter  # Enter模拟了键盘上的Enter键,确保命令被执行tmux send -t <name> '命令' Enter

tmux kill-session -t <session-name> 杀死窗口
ssh root@10.1.66.67

tmux窗口内移动
ctrl+b ]  进入复制模式,可以实现上下翻页

```



## **Linux快捷键**

| Ctrl + c | 终止当前执行的进程        |
| -------- | ------------------------- |
| Ctrl + d | 相当于exit，退出当前shell |

## 杀死tmux窗口内的进程

以silent_script.sh脚本为例，该脚本仅占用一个进程号且不输出任何内容

```shell
#!/bin/bash

# 无限循环，不执行任何操作，也不输出任何内容
while :
do
    sleep 60
done

```

创建完该脚本，新建一个tmux窗口运行此脚本

首先查看所有的tmux窗口，找到我们需要杀死的进程

```
ps aux | grep tmux
```

![image-20231115214655126](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/image-20231115214655126.png)

正常杀死在tmux内的tmux_trex_console窗口，命令：

```
tmux kill-session -t tmux_trex_console
```

查看是否成功杀死该窗口：（一般来说，杀死该窗口，里面的进程也会一并杀死）

```
 ps aux | grep tmux
```

如果该窗口没杀死，我们使用以下命令获得它的进程号

```
ps aux | grep silent | grep -v grep | awk '{print $2}'
```

使用kill -9强制杀死

```
kill -9 $(ps aux | grep <脚本关键字> | grep -v grep | awk '{print $2}')
```

使用命令查看里面的运行进程id,判断进程是否被杀死

```
 ps aux | grep silent | grep -v grep
 for i in {1..3}; do kill -9 $(ps aux | grep trex-console | grep -v grep | awk '{print $2}') && break || sleep 1; done
```

如果没杀死，重复以上两个步骤最多三次，三次之后报错

总结综上所述，查找指定脚本进程号，并判断是否杀死，如果杀死输出“进程已杀死”，未杀死输出“无法杀死该进程”

```
for i in {1..3}; do kill -9 $(ps aux | grep <silent_script.sh> | grep -v grep | awk '{print $2}') && break || sleep 1; done && echo "进程已被杀死" || echo "无法杀死进程"
```



## 逻辑判断语法

如以下命令：

```shell
for i in {1..3}; do kill -9 $(ps aux | grep <silent_script.sh> | grep -v grep | awk '{print $2}') && break || sleep 1; done && echo "进程已被杀死" || echo "无法杀死进程"
```

1. **`&&` 和 `||` 运算符**：
   - `&&`（逻辑与）：如果左侧的命令执行成功（返回状态为0），则执行右侧的命令。
   - `||`（逻辑或）：如果左侧的命令执行失败（返回状态非0），则执行右侧的命令。
   - `&&` 和 `||` 拥有相同的优先级，并且是从左到右进行评估。
2. **命令分组和子shell**：
   - 圆括号 `()` 用于创建一个子shell，里面的命令会在新的子进程中执行。
   - 花括号 `{}` 用于将命令组合在一起，但不会创建新的子进程。
3. **循环结构（如 `for` 循环）**：
   - 循环本身的逻辑是独立于 `&&` 和 `||` 运算符的。循环内的命令会按照循环的逻辑执行。

所以综上所述：

- `for i in {1..3}; do ...; done` 是一个循环结构。循环会执行 3 次，除非遇到 `break` 语句提前跳出。
- `kill -9 ... && break || sleep 1` 是在循环内部执行的命令。这里如果 `kill -9` 成功执行（即成功杀死进程），则执行 `break` 退出循环。如果 `kill -9` 失败，则执行 `sleep 1`。
- `&& echo "进程已被杀死"` 和 `|| echo "无法杀死进程"` 是在循环结束后执行的。如果整个循环正常完成（`kill -9` 成功），则执行 `echo "进程已被杀死"`。如果循环因为 `kill` 命令始终无法成功执行而结束，则执行 `echo "无法杀死进程"`。



## linux挂载

“挂载”（Mounting）是指将一个文件系统接入到另一个文件系统的过程，使其成为后者的一部分，通常在一个特定的挂载点（即目录）上。

挂载命令：mount -t device dir

卸载： umount

### 挂载的基本概念：

1. **文件系统**：
   - 文件系统是存储和组织数据的方法，它决定了数据在存储设备上如何存储和检索。
   - 例如，Linux 中常见的文件系统类型有 ext4、NTFS、FAT32 等。
2. **挂载点**：
   - 挂载点是文件系统树结构中的一个目录，用作访问挂载的文件系统的入口点。
   - 例如，在 Linux 系统中，你可以把一个 USB 驱动器挂载到 `/mnt/usb` 目录，之后就可以通过访问这个目录来访问 USB 驱动器中的文件。
3. **挂载的过程**：
   - 当你挂载一个设备时，操作系统会将该设备的文件系统加入到整个系统的文件系统树中，通常在一个空的目录（挂载点）上。
   - 挂载之后，位于该设备上的文件和目录就可以通过访问挂载点的路径来访问。
4. **卸载（Unmounting）**：
   - 卸载是挂载的反向过程，它从文件系统树中移除一个已经挂载的文件系统。
   - 在卸载设备前进行这一操作是重要的，因为它确保所有的数据都已经正确地写入存储设备并且文件系统不再被使用。

### 挂载的实际应用：

在日常使用中，挂载最常见的例子是插入 USB 驱动器。当你插入驱动器时，操作系统会自动将其挂载到文件系统的某个点，使你可以访问里面的文件。



## Linux远程拷贝 scp

scp与cp的命令类似，但是scp允许不同服务器或操作系统端口之间的拷贝

### **命令格式：**

`scp [参数] [原路径] [目标路径]`

常用参数：

- -B 使用批处理模式（传输过程中不询问传输口令或短语）
- -P port 注意是大写的P, port是指定数据传输用到的端口号

```shell
scp -P 22 qiuwenxuan@10.20.69.18:/home/qiuwenxuan simple_cfg.yaml
```



## 删除文件夹

```
rm 
```



## mkdir 创建文件

```
mkdir -p /path/to/folder # -p 递归创建文件，即如果 /path/to 不存在，也会一并创建

```



## ps查找进程

```
ps -ef # 显示所有进程
ps -aux 和 -ef类似 无区别

ps -ef |grep xxx|grep -v grep # grep -v 接过滤不显示的命令
```



## ssh与sshpass

`ssh` 是 SSH 协议的标准客户端，用于与远程主机建立安全的连接。

`sshpass` 是一个辅助工具，用于在脚本中自动提供密码给 `ssh` 命令，以避免需要交互式输入密码。

如：远程拷贝将密码作为参数传递

```
sshpass -p root scp -rp jmnd root@192.168.8.11:/run/
```



## kill -9 & kill -15

1. **`kill -15`：**

   - 也称为 `SIGTERM` 或终止信号。
   - 这是一种优雅的终止方式，它告诉进程立即终止，但允许它完成一些清理工作。进程有机会在收到这个信号后做一些清理操作，保存状态，关闭文件等。
   - 如果进程响应正常，它会尽力进行清理操作，然后自行退出。

   ```
   bashCopy code
   kill -15 PID
   ```

2. **`kill -9`：**

   - 也称为 `SIGKILL` 或强制终止信号。
   - 这是一种强制性的终止方式，它会立即终止进程，不给予进程进行清理的机会。使用这个信号会强制终止进程，但也可能导致数据损坏或未完成的操作。

   ```
   bashCopy code
   kill -9 PID
   ```

一般来说，首先尝试使用 `kill -15`，因为它会更友好地请求进程终止。只有在进程无法正常终止或者出现其他问题时，才考虑使用 `kill -9`。



## find 查找指定文件名

指定路径查找文件名

```
find /path/to/search -name "filename"
find / -name "tmux"
```

**忽略大小写查找：**

```
find /path/to/search -iname "filename"
```

**在当前目录及子目录下查找：**

```
find . -name "filename"
```



## df -h  查看内存



## 标准输出和标准错误

在Shell命令中，文件描述符被用来表示不同的流：

- `1` 代表标准输出（stdout），即正常的命令输出。
- `2` 代表标准错误（stderr），即错误信息的输出。

因此，`2>/dev/null` 将标准错误流（错误消息）重定向到 `/dev/null`，从而丢弃任何错误消息。如果你想将标准输出重定向到 `/dev/null`，可以使用 `1>/dev/null`。

## 配置网络接口ip地址和网关

需要修改相应的网络配置文件。在大多数 Linux 发行版中，这些文件位于 `/etc/sysconfig/network-scripts/` 目录下，文件名通常以 `ifcfg-` 开头，

### 给网口配ip地址

```
ip addr add <IP_ADDRESS/MASK> dev <INTERFACE_NAME>
ip addr add 192.168.1.2/24 dev eth0
```

### 删除设备对应的ip地址

```
sudo ip addr del <IP_ADDRESS/MASK> dev <INTERFACE_NAME>
```

### 添加网关

```
route add default gw 10.21.187.254
```

### 查看所有网关

```
ip route show
```

### 重启网络服务

```
systemctl restart NetworkManager
```

## ubuntu安装java环境

```
sudo apt-get remove --purge openjdk-\*  # 删除原有的java环境
sudo apt-get update # 更新apt下载源
apt install openjdk-8-jre-headless # 下载jdk8
java -version # 查看安装是否成功
```

## 查看LInux系统信息

```
uname -a # 查看版本当前操作系统内核信息

cat /proc/version # 查看当前操作系统版本信息

cat /proc/cpuinfo # 查看Linux cpu相关信息

hostname # 查看服务器名称

ifconfig # 查看网络信息

lsblk # 查看磁盘，可用块设备信息，并显示依赖关系

df -h # 查看可用磁盘空间
```

![image-20240123155530237](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240123155531.png)

## 重定向内容到剪切板

`clip` 命令将输入的内容剪切到剪贴板中。在Linux或Unix系统中，通常使用 `xclip` 或 `xsel` 命令来实现这个功能。

```
clip < ~/.ssh/id_rsa.pub
将~/.ssh/id_rsa.pub秘钥内容重定向到剪切板clip
```

## nohup后台运行工具

`nohup` 是一个在Unix和Unix-like系统（比如Linux）中用来运行命令的工具。它的作用是让命令在后台运行，并且不受用户退出或断开终端的影响，即使用户退出登录或者关闭了终端，被 `nohup` 启动的程序也能继续执行。通常情况下，`nohup` 会将程序的标准输出和标准错误输出重定向到一个名为 `nohup.out` 的文件中。

```
nohup ./t-rex-64 -i --no-scapy-server --emu >./trex.log & # 后台运行t-rex-64脚本，并将输出重定向到trex.log
```



## 压缩和解压缩

解压缩是一个常用的操作，在 [Linux](https://so.csdn.net/so/search?q=Linux&spm=1001.2101.3001.7020) 中通常比较常用的是 tar 命令，zip 和 rar 命令则是 Windows 中比较常用。

### tar命令

```
# 压缩文件或目录到test.tar.gz
tar -zcvf test.tar.gz dir/file

# 解压文件的内容 把c换成x就行了
tar -zxvf test.tar.gz -C 目标文件夹路径
```

```
-z : 使用gzip来解压文件
-c : --create 创建一个新的归档压缩包
-x : 从解压包中解压文件
-v : --verbose 详细列出处理的文件
-f : --file 必选，使用档案文件或设备
```



### rar命令

```
rar a -r test.rar file # 压缩文件

unrar x test.rar # 解压文件
```



### zip命令

```
zip -r test.zip file # 压缩文件

unzip test.zip # 解压文件
```

