## 1 在hive中创建分区表
```sql
create table bigdata.part_emp1
(
empno int,
ename string,
job string,
mgr int,
hiredate string,
sal float,
comm float,
deptno int
)partitioned by (y int)
row format delimited fields terminated by ',';
```
## 2 方法一：
sqoop import \
--hive-import \
--connect jdbc:oracle:thin:@192.168.1.76:1521/ORCL \
--username scott \
--password 123456 \
--table EMP \
--where "to_char(hiredate,  'yyyy') = 1980" \
--hive-database bigdata \
--hive-table part_emp1 \
--hive-partition-key y \
--hive-partition-value 1980 \
--delete-target-dir \
--fields-terminated-by ',' -m 1
## 3 方法二：
sqoop import \
--hive-import \
--connect jdbc:oracle:thin:@192.168.1.76:1521/ORCL \
--username scott \
--password 123456 \
--query "select * from EMP where to_char(hiredate,  'yyyy') = 1981 and \$CONDITIONS" \
--target-dir  /user/root/EMP \
--delete-target-dir \
--hive-database bigdata \
--hive-table part_emp1 \
--hive-partition-key y \
--hive-partition-value 1981 \
--fields-terminated-by ',' -m 1