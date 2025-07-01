## --方法1：(linux运行) 
hadoop fs -get  /user/hive/warehouse/ods.db/tong_emp1/*  /lx  
## --方法2：(linux运行) 
hive -e "select * from ods.tong_emp1" > /lx/emp_tong1.txt
## --方法3：(hive运行) 
insert overwrite local directory  '/lx/a'
row format delimited fields terminated by ','
select * from ods.tong_emp1;