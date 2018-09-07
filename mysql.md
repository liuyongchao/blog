```
nohup  mysqldump -uroot -p**** radar2017 URLCONTENT  -e --max_allowed_packet=1048576 --net_buffer_length=16384 > /tmp/content.sql &
```

```
nohup mysql -h 127.0.0.1 -u root -p*** -P 3306 -e "SELECT * from table"  >> /tmp/20180905/3days20180905.txt &
```
