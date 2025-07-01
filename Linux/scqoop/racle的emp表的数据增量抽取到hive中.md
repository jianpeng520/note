
## 1 增量方法一：
### 1.1 oracle中添加数据
insert into  emp(empno, sal) values(9527, 3000);
commit;
### 1.2 把增加的9527抽取到hive中
```shell
#--incremental append：增量追加
#check-column empno：检查hive中的empno
#--last-value 7934：hive中的empno最大值是7934，那么在抽数据的时候，只把oracle中大于7934的数据抽到hive中
```

sqoop import \
--incremental append \
--connect jdbc:oracle:thin:@192.168.1.76:1521/ORCL \
--username scott \
--password 123456 \
--table EMP \
--target-dir /user/hive/warehouse/bigdata.db/emp \
--fields-terminated-by ',' \
--check-column empno \
--last-value 7934 -m 1
## 2 增量方法二：
### 2.1 oracle中添加数据
insert into  emp(empno,hiredate, sal) values(9529, date'2025-04-14' ,5000);
commit;
### 2.2 把增加的2025-04-14这天的数据抽取到hive中
sqoop import \
--append \
--connect jdbc:oracle:thin:@192.168.1.76:1521/ORCL \
--username scott \
--password 123456 \
--table EMP \
--where "hiredate = date'2025-04-14'" \
--target-dir /user/hive/warehouse/bigdata.db/emp \
--fields-terminated-by ','   -m 1