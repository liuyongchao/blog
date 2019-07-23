# 一、centOS6.7安装mysql5.6

## 1.下载mysql的repo源

```bash
#官网https://dev.mysql.com/downloads/mysql/5.6.html#downloads
#下载MySQL-server-5.6.42-1.el6.x86_64.rpm
wget https://dev.mysql.com/get/Downloads/MySQL-5.6/MySQL-server-5.6.42-1.el6.x86_64.rpm
wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
#下载MySQL-client-5.6.42-1.el6.x86_64.rpm
wget https://dev.mysql.com/get/Downloads/MySQL-5.6/MySQL-client-5.6.42-1.el6.x86_64.rpm
```

## 2.安装包

```bash
sudo rpm -ivh MySQL-server-5.6.42-1.el6.x86_64.rpm
sudo rpm -ivh mysql-community-release-el7-5.noarch.rpm
```

## 3.启动mysql

```bash
#启动服务
service mysql start
#查看密码
sudo cat /root/.mysql_secret
```

## 4.登录mysql

```mysql
#登录mysql
mysql -uroot -pnaxxm
#重置密码
SET PASSWORD = PASSWORD('naxm');
#生效
flush privileges;
```

# 二、centOS7安装mysql5.7

### 1.下载mysql的repo源

```bash
wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
```

### 2.安装包

```bash
#安装这个包后，会获得两个mysql的yum repo源：/etc/yum.repos.d/mysql-community.repo，/etc/yum.repos.d/mysql-community-source.repo
sudo rpm -ivh mysql-community-release-el7-5.noarch.rpm
```

### 3.安装mysql

```bash
#根据提示安装就可以了,不过安装完成后没有密码,需要重置密码
sudo yum install mysql-server
```

### 4.免密登录mysql

```bash
mysql -u root
#登录时有可能报这样的错：ERROR 2002 (HY000): Can‘t connect to local MySQL server through socket ‘/var/lib/mysql/mysql.sock‘ (2)，原因是/var/lib/mysql的访问权限问题。下面的命令把/var/lib/mysql的拥有者改为当前用户sudo chown -R root:root /var/lib/mysql
```

### 5.设置免密登录配置

```bash
vi /etc/my.cnf
#[mysqld]添加 skip-grant-tables
#重启mysql服务
service mysqld restart
```

### 5.修改密码

```bash
#更新root密码
mysql > update mysql.user set authentication_string=password('123456') where user='root' and Host = 'localhost';
```

### 6.切换数据库再次修改密码

```bash
SET PASSWORD = PASSWORD('123456');
```

