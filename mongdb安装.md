# ubuntu下安装MongDB
* 1.用apt-get进行安装(以root用户安装)
```bash
sudo apt-get install mongodb
注意：或者 su -root 权限为root，软件安装环境变量为root用户；
      su root 权限为root，软件安装环境变量为当前用户
```
* 2.装好后会自动运行mongod程序，通过"pgrep mongo -l "查看进程是否已经启动
```bash
pgrep mongo -l 
```
* 3.在终端输入"mongo"，然后回车进入数据库
```bash
mongo
```
