#partition-range
## 范围分区range
### --1.语法
<span style="background:#d3f8b6">create table 分区表名</span>
<span style="background:#d3f8b6">(</span>
<span style="background:#d3f8b6"> 字段名     数据类型,</span>
<span style="background:#d3f8b6"> .....</span>
<span style="background:#d3f8b6">)partition by range(分区字段名)</span>
<span style="background:#d3f8b6">(</span>
<span style="background:#d3f8b6"> partition  分区名1   values less than(值1),</span>
<span style="background:#d3f8b6"> partition  分区名2   values less than(值2),</span>
<span style="background:#d3f8b6"> ........</span>
<span style="background:#d3f8b6">);   </span>           


### --2.创建分区表
```sql
create  table emp_range1
(
 empno     int,
 ename     varchar2(20),
 job       varchar2(20),
 mgr       int,
 hiredate  date,
 sal       number,
 comm      number,
 deptno    int
)partition by range(hiredate)
(
 partition  p1980   values less than(date'1981-1-1'),
 partition  p1981   values less than(date'1982-1-1'),
 partition  p1982   values less than(date'1983-1-1'),
 partition  p1983   values less than(date'1984-1-1')
);              
```


### --3.往emp_range1中插入数据
```sql
insert into emp_range1 select * from emp where hiredate < date'1984-1-1';
commit;
```


### --4.查看有几个分区
```sql
select * from user_tab_partitions where table_name = 'EMP_RANGE1';
```

如何查看每个分区存放数据的规则：
在user_tab_partitions表格中，点击high_value字段里面,<\long>旁边的三个点

### --5.查看分区数据
```sql
select * from 分区表名   partition(分区名);
select * from emp_range1 partition(P1981);  --查看1981年的数据,分区扫描，效率高

select * from emp_range1 where to_char(hiredate, 'yyyy') = 1981; --查看1981年的数据，全表扫描，效率低
```


### --6.如何手动给分区表增加分区(带 alter table的语法不用记)
--语法：
<span style="background:#d3f8b6">alter table 分区表名  add partition 分区名 values less than(值);</span>


```sql
--增加一个1984的分区
alter table emp_range1 add partition p1984 values less than(date'1985-1-1');

select * from user_tab_partitions where table_name = 'EMP_RANGE1'; --查看有几个分区
```

### --7.分区表中插入数据
```sql
insert into emp_range1(empno,ename, hiredate, sal)values(9527, '周星星', date'1984-10-1', 800);
commit;

select * from emp_range1 partition (p1984);  --查询分区数据
```


### --8.如何清空单个分区中的数据，不是全表数据
--语法：
<span style="background:#d3f8b6">alter table 分区表名   truncate partition 分区名;</span>

```sql
alter table emp_range1 truncate partition p1984;
select * from emp_range1 partition (p1984);
```


### --9.如何删除分区
--语法：
<span style="background:#d3f8b6">alter  table 分区表名  drop partition 分区名;</span>

```sql
alter table emp_range1 drop partition p1984;
select * from emp_range1 partition (p1984); --报错，分区已经被删除，查询不到了
```