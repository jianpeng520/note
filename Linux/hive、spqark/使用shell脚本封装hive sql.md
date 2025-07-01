## 0.linux中输出系统当前时间
```
`echo 程序已开始：date "+%Y-%m-%d %H:%M:%S"`
```
## 1.linux中编写shell      `vim  /lx/s1.sh`
```
db=$1  #接收第1个参数
tb=$2  #接收第2个参数

echo 程序已开始：`date "+%Y-%m-%d %H:%M:%S"`

hive -e"
--a：删除数据库
drop database if exists $db cascade;

--b.创建数据库
create database $db;

--c.数据库中删除表格
drop table if exists $db.$tb;

--d.创建表格
create table if not exists  $db.$tb
(
 deptno  int,
 job     string,
 sal     double
);

--e.开启本地模式
set   hive.exec.mode.local.auto=true;

--f.目标表中插入数据
insert overwrite table $db.$tb
select deptno,job,  sum(sal) from bigdata.emp
group by deptno,job;
"
echo 程序已结束：`date "+%Y-%m-%d %H:%M:%S"`
```
## 2.linux中调用shell     
`bash  /lx/s1.sh dwd emp2`