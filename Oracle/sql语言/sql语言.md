## [[DDL]] : 数据定义语言
(create table / alter table / drop / truncate) : 数据定义语言

## [[DML]]: 数据操纵语言
(insert into  / update / delete ) : 数据操纵语言

## DQL： 数据查询语言
(select) ： 数据查询语言

## TCL:  事务控制语言
(commit / rollback) :  事务控制语言

## DCL: 权限控制
(grant / revoke) : 权限控制


## 函数
所有sql基本函数应用在DDL，DML，DQL操作上，只写函数是报错的
```sql
floor(3.14)   --报错

--可以写
select floor(3.14) from dual'

```
### [[基本函数]]:
- 数字函数
- 字符串函数
- 日期函数
### [[逻辑判断函数]]
- decode
- case when
### [[聚合函数、排序函数]]
- group by
- order by
### [[分析函数]]
- over
### [[排名函数]]
- row_number
- rank
- dense_rank
### [[平移函数]]
- lag
- lead