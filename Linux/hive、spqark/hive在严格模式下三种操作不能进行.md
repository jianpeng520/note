--设置严格模式
set hive.mapred.mode=strict;


--1.严格模式下不能使用笛卡尔积连接
select * from bigdata.emp, ods.dept;

--2.严格模式下查询分区表必须要筛选分区数据
  select * from emp_part1;    --报错
  select * from emp_part1 where y=1981;
  
--3.严格模式下order by排序必须要加limit
  select * from bigdata.emp order by sal desc ;      --报错
  select * from bigdata.emp order by sal desc limit 10;