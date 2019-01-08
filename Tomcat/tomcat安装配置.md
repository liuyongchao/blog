### tomcat安装
#### 1.tomcat下载解压
```
wget http://mirrors.tuna.tsinghua.edu.cn/apache/tomcat/tomcat-8/v8.5.37/bin/apache-tomcat-8.5.37.tar.gz
tar -zxvf apache-tomcat-8.5.37.tar.gz
```
#### 2.tomcat启动、停止
```
./apache-tomcat-8.5.37/bin/startup.sh 
./apache-tomcat-8.5.37/bin/shutdown.sh
```
### tomcat配置
#### 1.tomcat配置端口号相关
```
vim ./apache-tomcat-8.5.37/conf/server.xml
 <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```
#### 2.tomcat配置tomcat-users.xml
* manager-gui - 访问HTML界面。
* manager-status - 仅访问“服务器状态”页面。
* manager-script - 访问本文档中描述的工具友好的纯文本界面，以及“服务器状态”页面。
* manager-jmx - 访问JMX代理接口和“服务器状态”页面。
```
<role rolename="manager-gui"/>
<user username="admin" password="elasticsearch@dev" roles="manager-gui"/>
```
#### 3.tomcat配置manager.xml文件$CATALINA_BASE/conf/[enginename]/[hostname]夹中配置或新建文件
* 本机访问配置
```
<Context privileged="true" antiResourceLocking="false"
         docBase="${catalina.home}/webapps/manager">
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.0\.0\.1" />
</Context>
```
* 远程访问配置(无ip限制)
```
<Context docBase="${catalina.home}/webapps/manager"
         privileged="true" antiResourceLocking="false" >
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="^.*$" />
</Context>
```
#### 4.tomcat配置host-manager.xml文件$CATALINA_BASE/conf/[enginename]/[hostname]夹中配置或新建文件
```
<Context docBase="${catalina.home}/webapps/manager"
         privileged="true" antiResourceLocking="false" >
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="^.*$" />
</Context>
```
