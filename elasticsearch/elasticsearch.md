### elasticsearch下载安装
* 1.下载elasticsearch
```
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.5.4.tar.gz
tar -zxvf elasticsearch-6.5.4.tar.gz
```
* 2.运行elasticsearch
```
./elasticsearch-6.5.4/bin/elasticsearch -d
出现下面错误解决方法
err1: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
root用户操作
(1)修改vi /etc/sysctl.conf
(2)添加配置：vm.max_map_count=262144
(3)执行命令：sysctl -p
err2:max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]
root用户操作
修改vim /etc/security/limits.conf
# 在最后面追加下面内容
*** hard nofile 65536
*** soft nofile 65536
*** 是启动ES的用户
err3:: max number of threads [1024] for user [elasticsearch] is too low, increase to at least [4096]
sudo vim /etc/security/limits.d/90-nproc.conf
修改后重启服务
*          soft    nproc     4096
root       soft    nproc     unlimited
appop       soft    nproc     unlimited
```
err4: system call filters failed to install; check the logs and fix your configuration or disable system call filters at your own risk
sudo vim /etc/security/limits.d/90-nproc.conf
```
vim elasticsearch.yml
bootstrap.memory_lock: false
bootstrap.system_call_filter: false
```
* 3.检查是否运行成功
```
curl http://localhost:9200/
```
* 4.配置远程服务
```
#配置为本机IP（推荐），也可配置为0.0.0.0
network.host: 10.246.3.129
http.port: 9200
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

### kibana可视化
* 1.下载解压kibana
```
wget https://artifacts.elastic.co/downloads/kibana/kibana-6.5.4-linux-x86_64.tar.gz
tar -zxvf kibana-6.5.4-linux-x86_64.tar.gz
```
* 2.配置config/kibana.yml
```
#配置目标elasticsearch.url
elasticsearch.url: "http://localhost:9200"
#配置远程访问
server.port: 5601
server.host: 0.0.0.0
```
* 3.启动kibana
```
nohup ./kibana-6.5.4-linux-x86_64/bin/kibana &
```
* 4. 访问 http://localhost:5601
### logstash
* 1.下载logstash
```
wget https://artifacts.elastic.co/downloads/logstash/logstash-6.5.4.tar.gz
tar -zxvf logstash-6.5.4.tar.gz
```
* 2.配置logstash.conf
```
#0 创建statement_filepath对应的文件`statement_filepath => "/home/naxxm/logstash-6.4.0/config/urlcontent_ckm.sql"`（注意路径修改）
select * from table where lastime >= :sql_last_value
#00 创建last_run_metadata_path对应的文件`last_run_metadata_path => "/home/naxxm/logstash-6.4.0/run_metadata.d/urlcontent_ckm_last_ir_sid.txt"`，创建对应文件夹即可（注意路径修改）
#000  驱动下载mysql-connector-java-5.1.36-bin.jar
#1.mysql.conf配置
input {
        stdin{
        }
        jdbc {
                jdbc_driver_library => "/home/naxxm/logstash-6.4.0/config/mysql-connector-java-5.1.36-bin.jar"
                jdbc_driver_class => "com.mysql.jdbc.Driver"
                jdbc_connection_string => "jdbc:mysql://127.0.0.1:3306/radar2"
                jdbc_user => "root"
                jdbc_password => "123456"
                #statement => "select * from URLCONTENT_CKM_2018 where ir_inserttime >= :sql_last_value" 
                statement_filepath => "/home/naxxm/logstash-6.4.0/config/urlcontent_ckm.sql"
                use_column_value => true
                tracking_column => ir_inserttime
                tracking_column_type => timestamp
                record_last_run => true
                last_run_metadata_path => "/home/naxxm/logstash-6.4.0/run_metadata.d/urlcontent_ckm_last_ir_sid.txt"
                clean_run => false
                lowercase_column_names => true
                jdbc_paging_enabled => true
                jdbc_page_size => 50000
                schedule => "* * * * *"
                jdbc_default_timezone => "Asia/Shanghai"
                type => "urlcontent_ckm"
        }
}
filter {
        json {
                source => "message"
                remove_field => ["message"]
        }
}
output {
    if[type] == "urlcontent_ckm" {
        elasticsearch {
                hosts => ["127.0.0.1:9200"]
                index => "urlcontent_ckm"
                document_id => "%{ir_hkey}"
                document_type =>"urlcontent_ckm_type"
                template_overwrite => true
        }
    }
    if[type] == "urlcontent_ckm" {
        elasticsearch {
                hosts => ["127.0.0.1:9200"]
                index => "urlcontent_ckm_2018"
                document_id => "%{ir_hkey}"
                document_type =>"urlcontent_ckm_2018_type"
                template_overwrite => true
        }       
    }
        stdout {
                codec => rubydebug
        }
}
```
* 2. 建立索引及mapping
```
PUT /urlcontent_ckm
POST urlcontent_ckm/urlcontent_ckm_type/_mapping
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
        "ir_annulment_date": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_annulment_reason": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_annulment_treatment": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_association": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_bank": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
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
        "ir_bid_addr": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_bid_date": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_bid_money": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_bid_org": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_bid_type": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_bidding_addr": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_bidding_agency": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_bidding_agency_addr": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_bidding_content": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_bidding_date": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_bidding_fileid": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_bidding_id": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_bidding_money": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_bidding_name": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_bidding_org": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_bidding_per": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_bidding_source": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_bidding_tel": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_bidding_type": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
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
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
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
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_doclength": {
          "type": "long"
        },
        "ir_expert": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_first_addr": {
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
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
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
        "ir_inserttime": {
          "type": "date"
        },
        "ir_lasttime": {
          "type": "date"
        },
        "ir_loadtime": {
          "type": "date"
        },
        "ir_media": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
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
        "ir_open_addr": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_open_time": {
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
        "ir_policy": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_price": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_public_date": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_qualification": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_sale_end_time": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_sale_start_time": {
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
        "ir_sign_date": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
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
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_small_enterprise": {
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
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_startid": {
          "type": "long"
        },
        "ir_status": {
          "type": "long"
        },
        "ir_subcontract": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_table_tag": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_tableflag": {
          "type": "long"
        },
        "ir_urlbody": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
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
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "ir_wcmid": {
          "type": "long"
        },
        "type": {
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
* 3.运行logstash
```
bin/logstash -f logstash.conf
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
```
### 创建索引
```
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
