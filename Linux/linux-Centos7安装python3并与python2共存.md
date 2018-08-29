# 准备工作
* 1.查看是否已经安装Python
```
python -v
```
* 2. 查看一下Python可执行文件的位置
```
which python
```
* 3.切换到/usr/bin/目录下执行 ll python* 命令查看python 指向的是python2.7
```
ll python*
```
* 4.下载相关包
```
yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gcc make
```
* 5.备份python
```
mv python python.bak
```
# 开始编译安装python3
* 1.下载最新版python3.7.0
```
wget https://www.python.org/ftp/python/3.7.0/Python-3.7.0.tar.xz
```
* 2.解压
```
tar -xvf Python-3.7.0.tar.xz
```
* 3.切换进去编译安装
```
cd Python-3.7.0/
make && make install
```
* 4.make安装,make是gcc的编译器
```
yum -y install gcc automake autoconf libtool make
```
* 5.安装gy++
```
yum install gcc gcc-c++
yum install make
```


