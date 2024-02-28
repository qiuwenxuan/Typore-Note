# Ubuntu配置Samba

# [1. 安装samba](http://space.jaguarmicro.com/pages/viewpage.action?pageId=96979805#1-安装samba)

- Ubuntu/ Debian：

```
sudo apt-get install samba
```

- CentOS：

```
sudo yum install samba
```

# [2. 配置smb.conf](http://space.jaguarmicro.com/pages/viewpage.action?pageId=96979805#2-配置smbconf)

配置前最好先备份一下原始文件

```
cp /etc/samba/smb.conf /etc/samba/smb.conf.backup
```

在smb.conf文件的末尾添加上如下，保存并退出

```
[connellsun]
   path = /home/connellsun    # 这个是connellsun登录时访问目录
   available = yes
   browseable = yes
   public = yes
   writable = yes
   valid users = connellsun
   map archive = no
```

还有其他的一些字段，可以参考如下：

- path：设置共享文件夹的路径；
- valid users：设置允许登陆的用户名；
- public：设置是否允许匿名访问；
- force user：设置强制设定新建文件所属用户；—— 参考填写：connellsun
- force group：设置强制设定新建文件所属用户组；—— 参考填写：connellsun
- create mask：设置创建文件设定的权限；—— 参考填写：0664
- directory mask：设置创建文件夹设定的权限；—— 参考填写：0775

# [3. 创建samba用户](http://space.jaguarmicro.com/pages/viewpage.action?pageId=96979805#3-创建samba用户)

创建服务器系统的登录用户，如果想使用现有账户的话，此步骤可以跳过

```
useradd connellsun
passwd xxxxxx

输入 linux用户登录密码
```

为samba设置访问用户，用户是我们系统的用户，密码是访问samba的密码

```
smbpasswd -a connellsun
输入 samba访问密码
```

# [4. 创建samba共享文件夹](http://space.jaguarmicro.com/pages/viewpage.action?pageId=96979805#4-创建samba共享文件夹)

```
mkdir \home\connellsun
cd \home
chmod -R 775 connellsun
chown -R connllsun:connellsun connellsun
```

# [5. 重启samba服务](http://space.jaguarmicro.com/pages/viewpage.action?pageId=96979805#5-重启samba服务)

```
systemctl restart smbd.service
service smbd restart
```

# [6. 访问samba共享文件夹](http://space.jaguarmicro.com/pages/viewpage.action?pageId=96979805#6-访问samba共享文件夹)

在windows下访问samba共享文件夹

win+R 输入 \\+ip 登录

查看共享的文件夹，右键 - 映射网络驱动器 - 可以看见本地多了一个共享盘 - 完成！



# 7.实际操作

首先，登录linux服务器

```
10.20.69.18  qiuwenxuan/qwx1100  ssh 和 samba都一样
```

查看linux服务器内的文件夹

![image-20231123185801534](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231123185802.png)

由于本机上已经启动samba,将linux本机上的文件与windows机的Z盘做一个映射

![image-20231123185900764](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231123185901.png)

接下来，我们要在linux机上使用git下载代码，下载成功后主动共享在windows的Z盘内

代码下载不成功，权限问题可以看如下文档配置ssh-key [Git操作问题总结 - DPU_SW_TEAM - Confluence (jaguarmicro.com)](http://space.jaguarmicro.com/pages/viewpage.action?pageId=1480553)

![image-20231123190050838](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231123190051.png)

代码共享到Z盘成功

![image-20231123190109110](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20231123190110.png)

