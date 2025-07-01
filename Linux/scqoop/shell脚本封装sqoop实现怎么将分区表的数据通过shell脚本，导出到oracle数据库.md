## 1 hive中准备分区表
bigdata.part_emp1
## 2 oracle中准备一张表
drop table emp1;
create table emp1
as
select * from emp where 1 = 2;
## 3 编写shell脚本     `vim   /lx/s3.sh`
```shell
#!/bin/sh

#报错就加：--input-null-string  '\\N' --input-null-non-string  '\\N'
# 先知道分区表有几个分区 
fenqu=`hive -e "show partitions bigdata.part_emp1" | awk 'NR > 1'`

for f in ${fenqu}
do

sqoop export \
--connect jdbc:oracle:thin:@192.168.1.76:1521/ORCL \
--username scott \
--password 123456 \
--table EMP1 \
--export-dir /user/hive/warehouse/bigdata.db/part_emp1/${f} \
--fields-terminated-by ',' 
done
```