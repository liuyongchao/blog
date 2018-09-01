## 一. 安装
* 前提: python3和pip3都已经安装。
```
[root@localhost]# pip3 install virtualenv virtualenvwrapper
#查看virtualenvwrapper.sh的位置
[root@localhost]# which virtualenvwrapper.sh
#假设输出是/usr/local/bin/virtualenvwrapper.sh
```
## 二. 使用这里以普通用户为例
* 2.1. 创建virtualenv文件夹
```
#目录位置可自定义
[opsky@localhost]$ mkdir $HOME/.local/virtualenv
```
* 2.2. 配置~/.bashrc
```
[opsky@localhost]$ vi ~/.bashrc
添加下面三行内容到文件末尾
#此目录为上述创建的目录
export WORKON_HOME=$HOME/.local/virtualenv
#默认建立python3虚拟环境
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
#此目录为上述which命令看到的
. /usr/local/bin/virtualenvwrapper.sh
```
* 2.3. 生效~/.bashrc
```
[opsky@localhost]$ . ~/.bashrc
```
* 2.4. 使用virtualenv
#创建名字为django0的虚拟环境
```
[opsky@localhost]$ mkvirtualenv django0
#进入django0虚拟环境
[opsky@localhost]$ workon django0
#退出django0虚拟环境
[opsky@localhost]$ deactivate
#创建名字为django1的虚拟环境
[opsky@localhost]$ mkvirtualenv django1
#删除django0虚拟环境
[opsky@localhost]$ rmvirtualenv django0
# 列出所有虚拟环境
[opsky@localhost]$ workon
```
* 2.5. 自定义python版本
```
mkvirtualenv -p参数可以自定义python版本

## 建立python2虚拟环境py2
```
[opsky@localhost]$mkvirtualenv -p /usr/bin/python2 py2
```
