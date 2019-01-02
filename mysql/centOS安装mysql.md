# centOS6.7安装mysql

## 1.下载mysql的repo源

```bash
#官网https://dev.mysql.com/downloads/mysql/5.6.html#downloads
#下载MySQL-server-5.6.42-1.el6.x86_64.rpm
wget https://dev.mysql.com/get/Downloads/MySQL-5.6/MySQL-server-5.6.42-1.el6.x86_64.rpm
#下载MySQL-client-5.6.42-1.el6.x86_64.rpm
wget https://dev.mysql.com/get/Downloads/MySQL-5.6/MySQL-client-5.6.42-1.el6.x86_64.rpm
```

## 2.安装包

```bash
sudo rpm -ivh MySQL-server-5.6.42-1.el6.x86_64.rpm
sudo rpm -ivh MySQL-client-5.6.42-1.el6.x86_64.rpm
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

