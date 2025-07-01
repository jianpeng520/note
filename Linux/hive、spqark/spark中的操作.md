spark和mapreuce一样，都属于计算引擎，
spark基于内存计算
mr基于硬盘计算

spark同样也需要启动<span style="background:#affad1">hadoop服务</span>和<span style="background:#affad1">hive</span><span style="background:#affad1">元数据服务</span>

## 1.新开一个窗口，linux中输入
spark-sql

## 2.查看当前有哪些数据库
show databases;

## 3.切换数据库
use bigdata;

## 4.显示当前数据库有哪些表格
show tables;

## 5.查询表格
```sql
select * from emp;
select * from bigdata.emp;
```
## 6.统计每个部门的总工资
select deptno, sum(sal) from emp group by deptno;