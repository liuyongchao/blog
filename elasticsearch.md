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
### elasticsearch查询语法
```bash
#查询各index文档总数
GET /_cat/indices?v
#查询文档总数
GET /_cat/count?v
#查询指定index文档数
GET /_cat/count/urlcontent_copy?v
```




### logstash
```
#1.mysql.conf配置
stdin{

        }
jdbc {
    jdbc_driver_library => "/home/naxxm/logstash-6.4.0/config/mysql-connector-java-5.1.36-bin.jar"
    jdbc_driver_class => "com.mysql.jdbc.Driver"
    jdbc_connection_string => "jdbc:mysql://127.0.0.1:3306/bigdata"
    jdbc_user => "root"
    jdbc_password => "123456"
    #statement => "select IR_SID,CAST(IR_CONTENT AS CHAR(10000) CHARACTER SET utf8) as IR_CONTENT from URLCONTENT_2018 where IR_SID>= :sql_last_value limit 10"
    statement_filepath => "/home/naxxm/logstash-6.4.0/config/URLCONTENT_COPY.sql"
    use_column_value => true
    tracking_column => "ir_sid"
    jdbc_paging_enabled => "true"
    jdbc_page_size => "50000"
    schedule => "* * * * *"
    #type => "jdbc"
    jdbc_default_timezone =>"Asia/Shanghai"
  }
}
filter {
        json {
        source => "message"
        remove_field => ["message"]
        }

}
output {
  stdout {
    codec => rubydebug
  }
  elasticsearch {
    hosts => "127.0.0.1:9200"
    index => "urlcontent_copy"
    document_id => "%{ir_sid}"
    document_type =>"urlcontent_copy_type"
    template_overwrite => true
    template => "/home/naxxm/logstash-6.4.0/config/URLCONTENT_COPY.json"
  }
}
#2.创建索引
POST /urlcontent_copy
#3.创建ik分词mapping
curl -XPOST http://localhost:9200/urlcontent_copy/urlcontent_copy_type/_mapping
{
        "properties": {
          "@timestamp": {
            "type": "date"
          },
          "@version": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "ir_abstract": {
           "type": "text",
            "analyzer": "ik_max_word",
            "search_analyzer": "ik_max_word"
          },
          "ir_bbskey": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "ir_bbsnum": {
            "type": "long"
          },
          "ir_bbstopic": {
            "type": "long"
          },
          "ir_catalog": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "ir_catalog1": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
		  "ir_catalog2": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "ir_channel": {
           "type": "text",
            "analyzer": "ik_max_word",
            "search_analyzer": "ik_max_word"
          },
          "ir_charset": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "ir_content": {
            "type": "text",
            "analyzer": "ik_max_word",
            "search_analyzer": "ik_max_word"
          },
		  "ir_urlbody": {
            "type": "text",
            "analyzer": "ik_max_word",
            "search_analyzer": "ik_max_word"
          },
          "ir_doclength": {
            "type": "long"
          },
          "ir_extname": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "ir_format": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "ir_groupname": {
            "type": "text",
            "analyzer": "ik_max_word",
            "search_analyzer": "ik_max_word"
          },
          "ir_hkey": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "ir_imageflag": {
            "type": "long"
          },
          "ir_keywords": {
            "type": "text",
            "analyzer": "ik_max_word",
            "search_analyzer": "ik_max_word"
          },
          "ir_lasttime": {
            "type": "date"
          },
          "ir_loadtime": {
            "type": "date"
          },
          "ir_mimetype": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "ir_pagelevel": {
            "type": "long"
          },
          "ir_pagerank": {
            "type": "long"
          },
          "ir_pkey": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "ir_serviceid": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "ir_sid": {
            "type": "long"
          },
          "ir_simflag": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "ir_simrank": {
            "type": "long"
          },
          "ir_sitename": {
           "type": "text",
            "analyzer": "ik_max_word",
            "search_analyzer": "ik_max_word"
          },
          "ir_startid": {
            "type": "long"
          },
          "ir_status": {
            "type": "long"
          },
          "ir_tableflag": {
            "type": "long"
          },
          "ir_urlcontent": {
           "type": "text",
            "analyzer": "ik_max_word",
            "search_analyzer": "ik_max_word"
          },
          "ir_urldate": {
            "type": "date"
          },
          "ir_urllevel": {
            "type": "long"
          },
          "ir_urlname": {
            "type": "text",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
          "ir_urlsize": {
            "type": "long"
          },
          "ir_urltime": {
            "type": "date"
          },
          "ir_urltitle": {
            "type": "text",
            "analyzer": "ik_max_word",
            "search_analyzer": "ik_max_word"
          },
          "ir_urltopic": {
           "type": "text",
            "analyzer": "ik_max_word",
            "search_analyzer": "ik_max_word"
          },
          "ir_wcmid": {
            "type": "long"
          },
		  "ir_nreserved1": {
            "type": "long"
          },
		  "ir_nreserved2": {
            "type": "long"
          },
		  "ir_nreserved3": {
            "type": "long"
          },
		   "ir_vreserved1": {
             "type": "text",
			"fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
		   "ir_vreserved2": {
             "type": "text",
			"fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
		   "ir_vreserved3": {
             "type": "text",
			"fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
		   "ir_vreserved4": {
             "type": "text",
			"fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
		  "ir_sreserved1": {
             "type": "text",
			"fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
		  "ir_sreserved2": {
            "type": "text",
			"fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
		  "ir_sreserved3": {
            "type": "text",
			"fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
		  "ir_authors": {
            "type": "text",
			"fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          },
		  "ir_srcname": {
            "type": "text",
            "analyzer": "ik_max_word",
            "search_analyzer": "ik_max_word"
          },
		   "ir_district": {
            "type": "text",
			"fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
		     
		   }
        }
  }
```
### 查词
```
curl -H "Content-Type: application/json" -XPOST http://localhost:9200/urlcontent_copy/urlcontent_copy_type/_search?pretty  -d'
{
    "query" : { "match" : { "ir_content" : "测绘" }},
    "highlight" : {
        "pre_tags" : ["<font color='red'>"],
        "post_tags" : ["</font>"],
        "fields" : {
            "ir_content" : {}
        }
    }
}
'
```
