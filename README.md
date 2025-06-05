# Ubuntu16安装Python3.7

### 前言

> 本文将按照Python的各个版本进行介绍的是Ubuntu16下安装3.7版本Python的过程
> 本文对于Python安装的定位是：独立安装，不修改或覆盖系统原有python的文件
>
> Ubuntu16.04自带了Python2.7和Python3.5，现在准备安装的版本为Python3.7.1，Python可以多个版本共存，不影响。
>
> 
>
> **镜像常备无后忧，误删误改不必愁；宕机之后徒唏嘘，早知备份何以忧**

## 一、前期准备

安装所须依赖：

```shell
sudo apt-get install libffi-dev uuid-dev lzma-dev liblzma-dev libncurses5-dev libgdbm-dev sqlite3 libsqlite3-dev openssl tcl8.6-dev tk8.6-dev libreadline-dev zlib1g-dev build-essential bzip2 libbz2-1.0 libbz2-dev libc6-dev libdb-dev libexpat1 libexpat1-dev libgdbm3 libncursesw5-dev libpcap-dev libreadline5 libreadline6 libreadline6-dev libsqlite0 libsqlite0-dev libsqlite3-0 libssl-dev libssl1.0.0 libxml2-dev libxslt1-dev sqlite tcl tk tk-dev xz-utils zlib1g zlib1g-dev make  

# 这是一句命令(德语超大声)
```
这里出错误的话移步第二步的 6. 的前，错误修复[更换镜像源修复](https://github.com/AGMXiaoTian/Ubantu-Python37?tab=readme-ov-file#%E6%9B%BF%E6%8D%A2%E6%BA%90%E6%93%8D%E4%BD%9C%E5%A6%82%E4%B8%8B)
#### 1. 1、python去官网下载：

#### [Python官网下载](https://www.python.org/downloads/release/python-371/)

如图第一个就可以

![image-20250529132219074](https://gitee.com/NeighborAngel/ubantu-python/raw/master/img/image-20250529132219074.png)

下载好文件为：

![image-20250529132339314](https://gitee.com/NeighborAngel/ubantu-python/raw/master/img/image-20250529132339314.png)

#### 2、~~setuptools官网下载源码~~：(暂时不需要)

[setuptools官网下载](https://pypi.org/project/setuptools/#files)

![image-20250529132830161](https://gitee.com/NeighborAngel/ubantu-python/raw/master/img/image-20250529132830161.png)

#### 3、 ~~pip官网下载源码~~：(暂时不需要)

#### [pip官网下载](https://pypi.org/project/pip/#files)

![image-20250529132929632](https://gitee.com/NeighborAngel/ubantu-python/raw/master/img/image-20250529132929632.png)

**~~通过Xftp将下载好的三个压缩文件传到/opt/dataset目录下~~**     **实际上暂时只需要传Python-3.7.1.tgz**

![image-20250529161125724](https://gitee.com/NeighborAngel/ubantu-python/raw/master/img/image-20250529161125724.png)

## 二、安装python3.7

>
> 源码的安装一般由3个步骤组成：配置（configure）、编译（make）、安装（make install）。
>  configure是一个可执行脚本，它有很多选项，使用命令./configure --help输出详细的选项列表。最常用的参数是 --prefix=目录，这个目录就是软件最后的安装目录。

#### 1、进入/opt/dataset目录解压文件

```shell
cd /opt/dataset
sudo tar -zxvf Python-3.7.1.tgz

# 默认的文件夹为Python-3.7.1，当然你也可以把它解压到一个你想放到的文件夹内，在最后面添加参数 -C <目录>,
```

#### 2、安装依赖

```shell
sudo apt-get install libpcre3 libpcre3-dev
sudo apt-get install -y libffi-dev

# 这里报错也移步至第二的 6. 更换源
```

#### 3、配置

```shell
cd Python-3.7.1/
sudo ./configure --prefix=/opt/python3.7
```

#### 4、编译

```shell
sudo make
```

#### 5、安装

```shell
sudo make install
```

正常安装是这样子的   如果失败尝试如下->[下面的安装失败修复流程]()

![image-20250529141052181](https://gitee.com/NeighborAngel/ubantu-python/raw/master/img/image-20250529141052181.png)

> ```ruby
> 如果出现config error：no acceptable C compiler found in $PATH
> 解决方法是安装GCC
> 一些apt-get会失败  选择下面的方式换源即可
> ```


## sudo make install 安装失败修复流程：
```shell
sudo make clean  # 清除上次编译结果
sudo ./configure --prefix=/usr/local/python3.7 --enable-optimizations
sudo make -j$(nproc)  # 使用所有 CPU 核心加速编译
sudo make install
```

## 替换源操作如下：
```shell
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak  # 先备份

# 替换源
sudo nano /etc/apt/sources.list

# 删除里面原来的   将里面的源替换成下面四条
deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse

# 保存后执行（ctrl+O ->回车 ->ctrl+X）
sudo apt update

# 然后重新运行->
sudo apt-get install libpcre3 libpcre3-dev

```

#### 6、创建软连接

```shell
sudo ln -s /opt/python3.7/bin/python3.7 /usr/bin/python3.7
```

#### 7、测试Python版本

```shell
python3.7 -V    # 如图 回显Python版本号为3.7.1为正确
```

![image-20250529141852885](https://gitee.com/NeighborAngel/ubantu-python/raw/master/img/image-20250529141852885.png)

#### 8、结束

到目前为止，Python3.7的版本已经安装完成了  

果你想让`python3`默认运行Python 3.7.1，你可以做一个软链接

```shell
sudo ln -sf /usr/local/python3.7/bin/python3.7 /usr/bin/python3
```



###### *鸣谢：*

简书：[fifi说](https://www.jianshu.com/p/6943bed3fd92)

简书：[python追求者](https://www.jianshu.com/p/2a22c0843af3)

CSDN：[一点年羊](https://blog.csdn.net/qq_35743870/article/details/125903040)
