### centOS6.5安装jdk
#### 1.查看centOS自带JDK是否已经安装
```
yum list installed | grep java
```
#### 2.查看安装版本
```
java -version
```
#### 3.卸载centOS自带JDK
```
##卸载
sudo yum -y remove java-1.7.0-openjdk*
sudo yum -y remove tzdata-java.noarch
#查看是否卸载干净
java -version
-bash: /usr/bin/java: No such file or directory
```
#### 4.安装openJDK（方法一）
```
yum -y list java*
sudo yum -y install java-1.8.0-openjdk*
#查看是否安装成功
java -version
openjdk version "1.8.0_171"
OpenJDK Runtime Environment (build 1.8.0_171-b10)
OpenJDK 64-Bit Server VM (build 25.171-b10, mixed mode)
```

#### 4.下载JDK（方法二）
```
wget https://download.oracle.com/otn-pub/java/jdk/8u191-b12/2787e4a523244c269598db4e85c51e0c/jdk-8u191-linux-x64.tar.gz
```
