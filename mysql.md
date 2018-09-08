```
nohup  mysqldump -uroot -p**** radar2017 URLCONTENT  -e --max_allowed_packet=1048576 --net_buffer_length=16384 > /tmp/content.sql &
```

```
nohup mysql -h 127.0.0.1 -u root -p*** -P 3306 -e "SELECT * from table"  >> /tmp/20180905/3days20180905.txt &
```
### shell脚本执行指定sql语句[单语句执行]
```
options=(
"全国采集量统计1"
"全国采集量统计2"
"全国采集量统计3"
"全国采集量统计4"
"全国采集量统计5"
"全国采集量统计6"
"全国采集量统计7"
)
mysqlFun(){
MYWEEK="`date +%W`"
MYDATE="`date +%Y%m%d%H%m%s`"
MYFILENAME="${MYWEEK}weeks_statis_${MYDATE}"
MYSQL_PATH=/home/naxxm/${MYFILENAME}.txt
SQL[0]="SELECT 1 from DUAL";
SQL[1]="SELECT 1 from DUAL";
SQL[2]="SELECT 2 from DUAL";
SQL[3]="SELECT 3 from DUAL";
SQL[4]="SELECT 4 from DUAL";
SQL[5]="SELECT 5 from DUAL";
echo ${SQL[$1]}
mysql -h 127.0.0.1 -P3308 -uroot -p123456 -e "${SQL[$1]}" >>$MYSQL_PATH
}

select var in ${options[@]};
do
        for i in `seq 0 $((${#options[*]}-1))`;do
                if [ "$var"x = "${options[${i}]}"x ];
                then mysqlFun ${i}
                        break
                fi
        done
        break
done
```
### shell脚本执行批量sql语句[多语句执行]
```
#!/bin/bash
MYWEEK="`date +%W`"
MYDATE="`date +%Y%m%d`"
MYFILENAME="${MYWEEK}weeks_statis_${MYDATE}"
MYSQL_PATH=/home/bigdata/weekstatis/${MYFILENAME}.txt
mysql -h 127.0.0.1 -P3306 -uroot -p****** >>$MYSQL_PATH <<EOF
select * from tb1;
select * from tb2;
select * from tb3;
...
EOF
echo "执行中,稍后请查看*week_statis_***.txt文件~"
```
