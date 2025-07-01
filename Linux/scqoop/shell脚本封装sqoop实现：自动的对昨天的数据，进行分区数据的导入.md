## 1 在hive中创建分区表
```sql
create table bigdata.emp_part1
(
empno int,
ename string,
job string,
mgr int,
hiredate string,
sal float,
comm float,
deptno int
)partitioned by (y string)  
row format delimited fields terminated by ',';
```
## 2 oracle中插入数据
insert into  emp(empno,hiredate, sal) values(9529, date'2025-04-14' ,5000);
commit;


## 3 linux中编写shell脚本   `vim   /lx/s2.sh`
```shell
#!/bin/sh
dt=`date -d "yesterday" +%Y-%m-%d`

sqoop import \
--hive-import \
--connect jdbc:oracle:thin:@192.168.1.76:1521/ORCL \
--username scott \
--password 123456 \
--table EMP \
--where "hiredate=date'${dt}'" \
--delete-target-dir \
--hive-database bigdata \
--hive-table emp_part1 \
--hive-partition-key y \
--hive-partition-value ${dt} \
--fields-terminated-by ',' \
--null-string '\\N' \
--null-non-string '\\N' -m 1
```

