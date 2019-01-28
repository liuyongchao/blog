# Openvpn-admin搭建

## 一、安装nginx

### 1.nginx安装

```bash
#1.安装nginx
yum install nginx -y
#2.运行nginx
systemctl start nginx
#3.查看状态
systemctl enable nginx
```

### 2.修改配置文件

```
server {
        listen       80;
        server_name  192.168.199.132;
        location / {
        root         /home/vhost/openvpn-admin;
        index index.php index.html index.htm;
        }

        location ~ \.php$ {

                fastcgi_pass 127.0.0.1:9000;

                fastcgi_index index.php;
                fastcgi_param SCRIPT_FILENAME  /home/vhost/openvpn-admin$fastcgi_script_name;
                include fastcgi_params;
        }
}
```

### 3.测试配置是否正确并重启

```bash
nginx -t
nginx -s reload
```

## 二、安装mysql

### 1.安装用的Yum Repository

```bash
wget -i -c http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm
yum -y install mysql57-community-release-el7-10.noarch.rpm
yum -y install mysql-community-server

```

