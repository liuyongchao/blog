### 安装nginx
```
进入：/usr/java/nginx位置
下载nginx:    wget http://nginx.org/download/nginx-1.8.0.tar.gz
下载openssl : wget http://www.openssl.org/source/openssl-fips-2.0.9.tar.gz
下载zlib    : wget http://zlib.net/zlib-1.2.8.tar.gz
下载pcre    : wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.38.tar.gz
如果没有安装c++编译环境，还得安装，通过yum install gcc-c++完成安装
```
下一步，编译安装
openssl ：
```
[root@localhost] tar zxvf openssl-fips-2.0.9.tar.gz

[root@localhost] cd openssl-fips-2.0.9

[root@localhost] ./config && make && make install
pcre:

 

[root@localhost] tar zxvf pcre-8.36.tar.gz

[root@localhost] cd pcre-8.36

[root@localhost]  ./configure && make && make install

 
```
zlib:
```
[root@localhost]tar zxvf zlib-1.2.8.tar.gz

[root@localhost] cd zlib-1.2.8

[root@localhost]  ./configure && make && make install
```
 

最后安装nginx
```
[root@localhost]tar zxvf nginx-1.8.0.tar.gz

[root@localhost] cd nginx-1.8.0

[root@localhost]  ./configure && make && make install
```
   启动nginx
```
/usr/local/nginx/sbin/nginx
```
出现错误提示
```
[root@localhost lib]# error while loading shared libraries: libpcre.so.1: cannot open shared object file: No such file or directory

   原因   在RedHat 64位机器上nginx读取的pcre文件为/lib64/libpcre.so.1文件，默认安装pcre时libpcre.so文件安装在/usr/local/lib/目录下，所以输入/opt/nginx/sbin/nginx -V 找不到文件路径！！

        1.首先确定安装了pcre.

        2.切换路径： cd /usr/local/lib  执行   ln -s /usr/local/lib/libpcre.so.1 /lib64/

        3.root权限下添加软链接 /usr/local/lib/libpcre.so.1 到 /lib64/ ：  ln -s /usr/local/lib/libpcre.so.1 /lib64/
```
