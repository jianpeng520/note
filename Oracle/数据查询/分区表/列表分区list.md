#partition-list
## 列表分区list
### --1.语法
<span style="background:#d3f8b6">create table 分区表名</span>
<span style="background:#d3f8b6">(</span>
<span style="background:#d3f8b6"> 字段名     数据类型,</span>
<span style="background:#d3f8b6"> .....</span>
<span style="background:#d3f8b6">)partition by list(分区字段名)</span>
<span style="background:#d3f8b6">(</span>
<span style="background:#d3f8b6"> partition  分区名1   values(值1),</span>
<span style="background:#d3f8b6"> partition  分区名2   values(值2),</span>
<span style="background:#d3f8b6"> ........</span>
<span style="background:#d3f8b6">);   </span>           

### --2.创建分区表
```sql
create  table emp_list1
(
 empno     int,
 ename     varchar2(20),
 job       varchar2(20),
 mgr       int,
 hiredate  date,
 sal       number,
 comm      number,
 deptno    int
)partition by list(deptno)
(
 partition  p10   values (10),
 partition  p20   values (20),
 partition  p30   values (30)
);              
```

### --3.分区表中插入数据
```sql
insert into emp_list1 select * from emp ;
commit;
```

### --4.查看有几个分区
```sql
select * from user_tab_partitions where table_name = 'EMP_LIST1';
```

如何查看每个分区存放数据的规则：
在user_tab_partitions表格中，点击high_value字段里面,<\Long>旁边的三个点

### --5.查看分区数据
```sql
select * from 分区表名   partition(分区名);
select * from emp_list1  partition(P10);  --查看10部门数据,分区扫描，效率高

select * from emp_list1  where deptno = 10; --查看10部门数据，全表扫描，效率低
```


### --6.如何手动给分区表增加分区(带 alter table的语法不用记)
--语法：
<span style="background:#d3f8b6">alter table 分区表名  add partition 分区名 values (值);</span>

```sql
--增加一个40的分区
alter table emp_list1 add partition p40 values (40);

select * from user_tab_partitions where table_name = 'EMP_LIST1'; --查看有几个分区
```

### --7.分区表中插入数据
```sql
insert into emp_list1(empno,ename, hiredate, deptno)values(9527, '周星星', date'1984-10-1', 40);
commit;

select * from emp_list1 partition (p40);  --查询分区数据
```


### --8.如何清空单个分区中的数据，不是全表数据
--语法：
<span style="background:#d3f8b6">alter table 分区表名   truncate partition 分区名;</span>

```sql
alter table emp_list1 truncate partition p40;
select * from emp_list1 partition (p40);
```


### --9.如何删除分区
--语法：
<span style="background:#d3f8b6">alter  table 分区表名  drop partition 分区名;</span>

```sql
alter table emp_list1 drop partition p40;
select * from emp_list1 partition (p40); --报错，分区已经被删除，查询不到了
```