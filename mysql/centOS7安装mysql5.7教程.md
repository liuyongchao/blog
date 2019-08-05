# centOS7安装mysql5.7教程(官网文档版)

## 系统环境

```bash
[root@centos7 ~]# uname -a
Linux centos7 3.10.0-862.el7.x86_64 #1 SMP Fri Apr 20 16:44:24 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux
```



## 一、卸载mysql

```bash
#查看yum已安装
yum list installed mysql*
#yum卸载已安装
yum remove mysql-community-client mysql-community-common mysql-community-libs mysql-community-libs-compat mysql-community-server mysql57-community-release
rm -rf /var/lib/mysql
rm /etc/my.cnf
#查看rpm安装
rpm -qa | grep -i mysql
#rpm卸载列表
rpm -e mysql57-community-release-el7-9.noarch
cd /var/lib/  
rm -rf mysql/
#清除余项
whereis mysql
#删除上面的文件夹
rm -rf /usr/bin/mysql
#删除配置
rm –rf /usr/my.cnf
rm -rf /root/.mysql_sercret
#剩余配置检查
chkconfig --list | grep -i mysql
chkconfig --del mysqld
```



## 二、安装mysql

### 1.进入mysql官网获取RPM包下载地址

```bash
#进入mysql官网获取RPM包下载地址https://dev.mysql.com/downloads/repo/yum/
wget https://repo.mysql.com//mysql80-community-release-el7-3.noarch.rpm
sudo rpm -Uvh mysql80-community-release-el7-3.noarch.rpm
```

### 2.查看哪些子存储库已启用或禁用

```bash
yum repolist all | grep mysql
```

### 3.(方法一)禁用最新GA系列的子存储库并启用特定系列的子存储库

```bash
#禁用8.0
sudo yum-config-manager --disable mysql80-community
#启用5.7
sudo yum-config-manager --enable mysql57-community
```

### 4.(方法二)手动编辑/etc/yum.repos.d/mysql-community.repo

```bash
#找到要配置的子存储库的条目，然后编辑该enabled选项。指定enabled=0禁用子存储库，或enabled=1启用子存储库。要安装MySQL 5.7，请确保MySQL 8.0具有enabled=0，并且MySQL 5.7具有 enabled=1
#注意：当启用多个版本系列的子存储库时，Yum将使用最新的系列
# Enable to use MySQL 5.7
[mysql57-community]
name=MySQL 5.7 Community Server
baseurl=http://repo.mysql.com/yum/mysql-5.7-community/el/7/$basearch/
enabled=0
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

[mysql80-community]
name=MySQL 8.0 Community Server
baseurl=http://repo.mysql.com/yum/mysql-8.0-community/el/7/$basearch/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
```

### 5.验证是否特定子库启用成功

```bash
yum repolist enabled | grep mysql
```

### 6.安装MySQL服务器的软件包以及其他所需的软件包

```bash
sudo yum install mysql-community-server
```

### 7.启动MySQL服务器并查看状态

```bash
sudo service mysqld start
sudo service mysqld status
#基于EL7的平台
sudo systemctl start mysqld.service
sudo systemctl status mysqld.service
```

### 8.查看root临时密码

```bash
sudo grep 'temporary password' /var/log/mysqld.log
```

### 9.登录并修改密码

```sql
mysql -uroot -p
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
--MySQL的 validate_password 插件默认安装。这将要求密码包含至少一个大写字母，一个小写字母，一个数字和一个特殊字符，并且密码总长度至少为8个字符。
```

### 10.修改密码策略及长度

```sql
set global validate_password_policy=0;
set global validate_password_length=1;
```

### 11.修改密码

```mysql
#(方法一)
ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';
#(方法二)
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('123456');
#(方法三)当前登陆用户
SET PASSWORD = PASSWORD('123456');
#5.7之前
update user set password=password('123456') where user='root';
```

## 三、用户创建、权限及远程授权

### 1.创建本地用户

```mysql
#foo表示你要建立的用户名，后面的123456表示密码,localhost限制在固定地址localhost有权访问登陆
CREATE USER foo@localhost IDENTIFIED BY '123456';
#或者
CREATE USER 'foo'@'localhost' IDENTIFIED BY '123456'; 
```

### 2.本地用户授权

```mysql
#所有权限
grant all privileges on *.* to 'foo'@'localhost' IDENTIFIED BY '123456';
#授予foo本地localhost用户对mysql库user表的增删改查权限
GRANT INSERT,DELETE,UPDATE,SELECT ON mysql.user TO 'foo'@'localhost';
#从mysql数据库的grant表中重新加载权限数据
flush privileges;
```

### 3.远程访问授权

```mysql
#%代表有权访问的所有主机，生产环境根据情况设置主机ip地址
GRANT ALL PRIVILEGES ON *.* TO 'foo'@'%' IDENTIFIED BY '123456';
#从mysql数据库的grant表中重新加载权限数据
flush privileges;
#查询是否修改成功
select user,authentication_string,host from mysql.user;
```

### 4.取消授权

```mysql
revoke all on *.* from 'foo'@'%';
```

### 5.附加

```mysql
#查看授权列表
select * from mysql.user where user='foo'\G;
#查看授权数据库
select * from mysql.db where user='foo'\G;
#查看授权数据表
select * from mysql.tables_priv\G;
#查看授权内容
show grants for foo;
```

### 6.删除用户

```mysql
drop user foo@'%';
```

