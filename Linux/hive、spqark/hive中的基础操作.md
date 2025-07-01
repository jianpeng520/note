## 1.进入hive
a.启动hadoop服务: `start-all.sh`
b.查看`jps`：进程数不少于6个
c.linux中启动hive元数据服务：
  前台启动，页面不能关闭：`hive service metastore`
  后台启动，页面不会显示：`hive service metastore &`
d.linux中输入：`hive`

## 2.查看当前有哪些数据库
show databases;

## 3.切换数据库
use bigdata;

## 4.显示当前是哪个数据库
set   hive.cli.print.current.db=true;  一次性开关

## 5.显示当前数据库有哪些表格
show tables;

## 6.查询表格
select * from emp;
select * from bigdata.emp;

## 7.去掉select查询的时候显示的表名
set   hive.resultset.use.unique.column.names=false;

## 8.统计每个部门的总工资
select deptno, sum(sal) from emp group by deptno;  分布式下很慢

## 9.当数据量很小的时候，可以不走分布式，走单机模式，也叫本地模式
set   hive.exec.mode.local.auto=true;
select deptno, sum(sal) from emp group by deptno;