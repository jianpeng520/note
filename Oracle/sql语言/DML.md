#insert #delete #update
## DML
	每次完成一次DML操作需要提交commit
	不提交可能有以下问题：
	1.a会话不提交b会话提交不了，会锁住
	2.当前的DML操作只在当前会话生效，其他会话select结果不变
	回滚操作只允许在commit之前操作，可回滚到上一次提交的状态，表示在提交状态b和上一次提交状态a之间的所有操作都是无效的，所以及时提交很重要
## 增加 insert into 
语法1: insert into table 表名 values(字段1,字段2,...,字段n)
语法2：insert into table 表名(字段1,字段2,字段3) values(字段1,字段2,字段3)   %%可筛选字段%%
语法3：insert into table 表名 as select * from 表名 where 条件   %%每次查询限定一条语句%%
```sql
1.插入3, '刘德华' , '冰雨' , 9, date'2005-2-10'
insert into mingxing values(3, '刘德华' , '冰雨' , 9, date'2005-2-10');
```
```sql
2.插入3, '刘德华' , '冰雨' , 9, date'2005-2-10'
insert into mingxing(id, name, movie, score, dt) values(3, '刘德华' , '冰雨' , 9, date'2005-2-10');
```
```sql
3.插入5, '邓紫棋', '泡沫', 9, date'2014-4-9'
insert into mingxing select 5, '邓紫棋', '泡沫', 9, date'2014-4-9' from dual;
```
## 删除 delete from 
语法1：delete from 表名 %%删除表的所有数据%%，也可以用truncate删除所有数据，详细看[^1]
语法2：delete from 表名 where 条件
```sql
1.delete from mingxing ;
2.delete from mingxing where name = '坤坤' and name = '华晨宇'; 

```
## 修改 update 
<font color="#92d050">语法：update  表名 set 字段1 = 值1， 字段2 = 值2 </font>
```sql
update mingxing set movie = '稻香' , score = 11 where name = '周杰伦' ;
```



[^1]: **oracle中truncate和delete的区别**
	
	1.truncate只能删除整个表的数据，不能进行提交或回滚操作
	
	2.delete可以删除某条数据或者整个表的数据，需要进行提交或回滚操作
	
	3.delete是根据条件逐行删除数据，并记录日志，所以删除效率没有truncate高
	
	4.如果要删除整个表的数据，可以选择truncate ;如果只是删除某条数据，用delete
