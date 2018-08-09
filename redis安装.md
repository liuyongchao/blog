# redis安装redis-4.0.11版本
* 1.[下载最新版本](https://redis.io/)
```bash
wget http://download.redis.io/releases/redis-4.0.11.tar.gz
```
* 2.解压redis
```bash
tar -xvf redis-4.0.11.tar.gz
```
* 3.切换到redis目录
```bash
 cd redis-4.0.11/
```
* 4.确定安装成功后进行编译make
```bash
make
注：新安装的linux系统没有安装gcc环境、需要安装gcc，为了方便采取一键安装方式  # yum install gcc
验证gcc是否安装成功：#rpm -qa|grep gcc
执行make 对Redis解压后文件进行编译
```
* 5.编译成功后，进入src文件夹，执行make install进行Redis安装
```bash
cd src && make install
```
* 6.运行./redis-server启动Redis 服务
```bash
./redis-server
```
