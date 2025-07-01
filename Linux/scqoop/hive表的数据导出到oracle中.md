
## 1 把增加的2025-04-14这天的数据抽取到hive中
```shell
null-string  '\\N'          ：  oracle字符串空到hive空的转换
null-non-string  '\\N'  ：  oracle非字符串空到hive空的转换
如果不加上面的空转换， 在hive中空会显示为小写null， 这个不算空，是字符串。  
如果加上上面的转换，是大写NULL， 这个是空
```

sqoop import \
--append \
--connect jdbc:oracle:thin:@192.168.1.76:1521/ORCL \
--username scott \
--password 123456 \
--table EMP \
--where "hiredate = date'2025-04-14'" \
--target-dir /user/hive/warehouse/bigdata.db/emp \
--fields-terminated-by ',' \
--null-string  '\\N' \
--null-non-string  '\\N' -m 1 
## 2 在oracle中建表(有表就清除数据)
`truncate    table    emp;`




## 3 hive中去掉为小写null的数据
insert overwrite table bigdata.emp select * from bigdata.emp where ename != 'null';

## 4 把hive数据导出到oracle中
--input-null-string  '\\N'          ：  hive字符串空到oracle空的转换
--input-null-non-string  '\\N'  ：  hive非字符串空到oracle空的转换

sqoop export \
--connect jdbc:oracle:thin:@192.168.1.76:1521/ORCL \
--username scott \
--password 123456 \
--table EMP \
--export-dir /user/hive/warehouse/bigdata.db/emp \
--fields-terminated-by ',' \
--input-null-string  '\\N' --input-null-non-string  '\\N'