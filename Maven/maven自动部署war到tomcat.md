### maven自动部署war到tomcat.md
#### 1.配置maven的setting.xml
```xml
<!--标签<servers>添加子标签-->
<server>
      <id>tomcat8.5</id>
      <username>admin</username>
      <password>123456</password>
    </server>
```
### 2.配置pom.xml
```xml
<!--标签<plugins>添加子标签-->
<plugin>
	<groupId>org.apache.tomcat.maven</groupId>
	<artifactId>tomcat7-maven-plugin</artifactId>
	<version>2.2</version>
	<configuration>
		<!-- 远程tomcat下manager路径 -->
		<url>http://127.0.0.1:8080/manager/text</url>
		<server>tomcat8.5</server>
		<username>admin</username>
		<password>123456</password>
		<update>true</update>
		<path>/${project.artifactId}</path>
	</configuration>
</plugin>
```
### 3.执行mvn命令
```
(1) mvn clean install

(2) mvn tomcat7:deploy (第一次部署执行)

(3) mvn tomcat7:redeploy（第二次部署是执行）
```

### 4.配置阿里镜像

```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                          https://maven.apache.org/xsd/settings-1.0.0.xsd">
      <mirrors>
        <mirror>
            <id>alimaven</id>
            <name>aliyun maven</name>      
            <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
            <mirrorOf>central</mirrorOf>
        </mirror>
      </mirrors>
</settings>
```

