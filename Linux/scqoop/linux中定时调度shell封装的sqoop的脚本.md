## 1 准备shell脚本            `vim  /lx/s4.sh`
```shell
#!/bin/sh
. /etc/profile
. ~/.bash_profile

# 先知道 hive中emp表格当前最大的y字段是多少
y=`hive -e "set hive.exec.mode.local.auto=true;select max(empno) from bigdata.emp;"  |  awk  'NR > 1' `

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
```
## 2 给shell脚本授权
chmod   755   /lx/s4.sh
## 3 oracle中增加数据
insert into  emp(empno, sal) values(9530, 4000);
commit;
## 4 启动定时调度服务
service  crond  restart
## 5 linux中编辑定时调度任务     crontab  -e
5 10 * * * . /etc/profile;/bin/sh /lx/s4.sh
