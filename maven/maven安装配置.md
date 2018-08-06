# 需要准备的东西
* 1.JDK
* 2.Eclipse
* 3. Maven程序包
# 下载安装
* 1.前往https://maven.apache.org/download.cgi下载最新版的Maven程序
* 2.将文件解压到D:\DevInstall\maven目录下
* 3.新建环境变量MAVEN_HOME，赋值D:\DevInstall\maven
* 4.编辑环境变量Path，追加%MAVEN_HOME%\bin\;
* 5.maven安装完成，检查是否安装成功
```bash
mvn -v
```
# 配置Eclipse的Maven环境
* 1.打开Window->Preferences->Maven->Installations，右侧点击Add
* 2.设置maven的安装目录，然后Finish
* 3.选中添加的maven，并Apply
* 4.打开Window->Preferences->Maven->User Settings，配置并Apply
```bash
user Settings
D:\DevInstall\maven\conf\settings.xml
Local Repository
D:\DevInstall\repository
```
* 5.maven配置安装完成
