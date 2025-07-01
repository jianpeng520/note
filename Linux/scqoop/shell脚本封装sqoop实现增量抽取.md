## 1 oracle中增加数据
insert into  emp(empno, sal) values(9527, 3000);
commit;




## 2 编写shell脚本      `vim  /lx/s1.sh`
```shell
#!/bin/sh

# 先知道 hive中emp表格当前最大的y字段是多少
y=`hive -e "set hive.exec.mode.local.auto=true;select max(empno) from bigdata.emp;"  |  awk  'NR > 1' `
```

sqoop import \
--incremental append \
--connect jdbc:oracle:thin:@192.168.1.76:1521/ORCL \
--username scott \
--password 123456 \
--table EMP \
--target-dir /user/hive/warehouse/bigdata.db/emp \
--check-column empno \
--last-value ${y} \
--fields-terminated-by ',' \
--null-string '\\N' \
--null-non-string '\\N' -m 1