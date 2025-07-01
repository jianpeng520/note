## 默认设置下：
hive在默认设置下，是不支持ACID事务的，所以不能直接对数据进行删除和修改，
对于删除和修改采用的是覆盖写入的方法

## --准备表

```
create table dept
(
deptno int,
dname string,
loc string
)
row format delimited fields terminated by '\t';
```

`load data local inpath '/lx/dept.txt' into table  dept;`

--a.删除deptno为40的部门
`insert overwrite table dept`
`select * from dept where deptno != 40;`

--b.把deptno=10的dname修改为深圳
```
insert overwrite table dept
select * from dept where deptno != 10
union all
select deptno, '深圳', loc from dept where deptno = 10;
```

## ACID模式下可以直接修改和删除：
hive在开启ACID模式下，并且把表格存储格式设为ORC格式，可以直接对表进行删除和修改


orc是列式存储格式：
1        2
张三   李四
18      20


### --a．首先先设置hive数据库为ACID模式（即打开事务处理模式）：
`set hive.support.concurrency=true;`

`set hive.txn.manager=org.apache.hadoop.hive.ql.lockmgr.DbTxnManager;`

### --b.然后创建一个 orc 格式的表格，并且设置表格属性为事务支持：
create table test
(
id int,
name string,
age int
) stored as orc
tblproperties('transactional'='true');

### --c.往表格中添加数据：
insert into test values(1001,'aa',18);


### --d.等数据添加完成后，来尝试着更改和删除它，此时操作就能成功了：
update test set age=20 where id=1001;

delete from test where id=1001;