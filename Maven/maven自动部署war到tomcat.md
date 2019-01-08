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

