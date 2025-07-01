## 设置reduce的个数
set mapred.reduce.tasks=3;  --设reduce个数
set mapred.reduce.tasks;    --查看reduce个数

一个reduce就是生成一个文件

## 0.搞张表
create table emp2 like bigdata.emp;

## 1.order by:全局排序,只有一个reduce参与排序,最终的结果是有序的
set hive.mapred.mode=nonstrict; --设置为非严格模式

insert overwrite table emp2
select * from bigdata.emp order by sal desc;

select * from emp2;  --最终结果是有序的

可以去hdfs查看有几个文件   /user/hive/warehouse/ods.db/emp2


## 2.sort by：在各自的reduce内参与排序,最终的结果是无序的
insert overwrite table emp2
select * from bigdata.emp sort by sal desc;

select * from emp2;--最终结果是无序的

可以去hdfs查看有几个文件   /user/hive/warehouse/ods.db/emp2


## 3.distribute by:按照指定的字段，通过hash算法，把数据发给reduce，
在reduce内它是不负责排序的，最终结果是无序的。
mod(字段哈希值,reduce的个数)=余数相同的会被划入同一个reduce内，也就是同一个文件中
mod(sal,3)


insert overwrite table emp2
select * from bigdata.emp distribute by sal;

select * from emp2;--最终结果是无序的

可以去hdfs查看有几个文件   /user/hive/warehouse/ods.db/emp2



## 4.cluster by:按照指定的字段，通过hash算法，
把数据发给reduce，在reduce内进行升序排序，最终结果是无序的。
mod(字段哈希值,reduce的个数)=余数相同的会被划入同一个reduce内


insert overwrite table emp2
select * from bigdata.emp cluster by sal;

select * from emp2;--最终结果是无序的

可以去hdfs查看有几个文件   /user/hive/warehouse/ods.db/emp2