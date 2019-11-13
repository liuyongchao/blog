### centos7源码安装mysql5.7教程

1. 检查有没有安装：

   ```bash
   rpm  -qa  |  grep  mariadb
   ```

2. 检查有没有安装：

   ```
   rpm  -qa  |  grep  mysql
   rpm  -e  --nodeps  mariadb-libs-5.5.44-2.el7.centos.x86_64
   # yum  -y  remove  卸载查到的内容
   ```

3. 查看是否有相关的组和用户

   ```bash
   cat  /etc/group  |  grep  mysql
   cat  /etc/passwd  |grep  mysql
   ```

4. 没有的话就创建，有的话跳过

   ```bash
   groupadd  mysql
   useradd  -r  -g  mysql  mysql
   ```

5. 下载mysql5.7.24的包【自己的tar包库里面也有】,建议放到/home目录下方便管理。

   ```bash
   wget  https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.24-linux-glibc2.12-x86_64.tar.gz
   ```

6. 解压：

   ```bash
   tar  -xzvf  mysql-5.7.24-linux-glibc2.12-x86_64.tar.gz
   ```

7. 将mysql目录下的文件挪到系统目录下

   ```bash
   cd  mysql-5.7.24-linux-glibc2.12-x86_64
   mv  ./*  /usr/local/mysql
   ```

8. 创建数据库存放目录：

   ```bash
   mkdir  -p  /usr/local/mysql/data
   ```

9. 更改目录属组：

   ```bash
   chown  -R  mysql:mysql  /usr/local/mysql/
   ```

10. 给可执行权限：

    ```bash
    chmod  -R  755  /usr/local/mysql/
    ```

11. 创建配置文件并赋权限：

    ```bash
    touch  /etc/my.cnf
    chown  -R  mysql:mysql  /etc/my.cnf
    ```

12. 编译并安装，末尾是密码：

    ```bash
    /usr/local/mysql/bin/mysqld  --defaults-file=/etc/my.cnf  --initialize  --user=mysql  --datadir=/usr/local/mysql/data  --basedir=/usr/local/mysql
    ```

13. 启动mysql服务：

    ```bash
    /usr/local/mysql/support-files/mysql.server  start
    ```

14. 做软连接添加到系统里面：

    ```bash
    ln  -s  /usr/local/mysql/support-files/mysql.server  /etc/init.d/mysql  
    ```

15. 用系统命令重启下：

    ```bash
    service  mysql  restart
    ```

16. 做个软连接，将mysql放入/usr/bin/目录下可用系统命令进行登录：

    ```bash
    ln  -s  /usr/local/mysql/bin/mysql  /usr/bin
    ```

17. 用系统命令登录：

    ```bash
    mysql  -u  root  -p
    ```

18. 修改初始化密码：

    ```mysql
    alter  user  'root'@'localhost'  identified  by  'win_2008';                #这一步必须要做
    ```

19. 进入mysql的用户库，然后给root用户可以远程登录的权限并刷新：

    ```mysql
    mysql>use  mysql;
    mysql>update  user  set  user.Host='%'  where  user.User='root';
    mysql>flush  privileges;
    ```

20. 编辑配置文件【需要什么可以自己加】：

    ```bash
    vim  /etc/my.cnf
    [mysqld]
    port  =  3306
    sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
    ```

21. 设置成开机自启动：

    ```bash
    cp  /usr/local/mysql/support-files/mysql.server  /etc/init.d/mysqld
    chmod  +x  /etc/init.d/mysqld
    chkconfig  --add  mysqld
    ```

22. 重启mysql，实现全部功能

    ```bash
    systemctl  restart  mysqld
    ```

