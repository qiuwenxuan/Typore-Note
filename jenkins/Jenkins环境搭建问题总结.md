# Jenkins环境搭建碰到的问题

## 1.pip install robotframework-sshlibrary后pip坏掉的问题

这个是因为包依赖冲突了， pip 依赖于一个特定版本的OpenSSL库，安装robotframework-sshlibrary拉低OpenSSL的版本，导致pip坏掉

修复手段：重新安装pip

### 1.1 外网环境下载get-pip.py

```
curl -o get-pip.py https://bootstrap.pypa.io/get-pip.py			// 如果下载太慢的话可以直接在浏览器中打开链接然后使用ctrl+s保存文件
```

### 1.2 然后将文件拖到vdi里

### 1.3 在vdi中通过mobaxitem 上传到服务器

### 1.4 在服务器的相应目录下执行get-pip.py即可

```
python get-pip.py
```

### 1.5 刷新shell的命令路径

```
hash -r
```

### 1.6 验证

```
pip list	// 如果出现回显则说明pip已经修复成功
```

# 2.import requests时报错 AttributeError: module 'lib' has no attribute 'X509_V_FLAG_CB_ISSUER_CHECK'

报错信息如下

![image-20240225111213277](http://wenxuanqiu.oss-cn-nanjing.aliyuncs.com/img/20240225111214.png)

报错原因：

```
pyOpenSSL版本与python版本不匹配
```

解决方法：

安装匹配的pyOpenSSL

```
pip3 install pyOpenSSL --upgrade
```

- pip3 install pyOpenSSL ``-``-``upgrade