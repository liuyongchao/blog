### elasticsearch下载安装
* 1.下载elasticsearch
```
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.4.0.tar.gz
tar -xzvf elasticsearch-6.4.0.tar.gz-xzvf elasticsearch-6.4.0.tar.gz
```
* 2.运行elasticsearch
```
elasticsearch-6.4.0\bin\elasticsearch
出现下面错误解决方法
err1: max virtual memory areas vm.max_map_count...
root用户操作
(1)修改vi /etc/sysctl.conf
(2)添加配置：vm.max_map_count=655360
(3)执行命令：sysctl -p
err2:max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]
root用户操作
修改vim /etc/security/limits.conf
# 在最后面追加下面内容
*** hard nofile 65536
*** soft nofile 65536
*** 是启动ES的用户
```

