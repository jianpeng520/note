#partition-interval
## 自动扩展分区 interval
### --按天自动扩展分区
#### --1.创建自动扩展分区表(指定当前数据中最小的一个日期)
```sql
create  table emp_d1
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
interval(numtodsinterval(1, 'day'))   --numtodsinterval将数字转换为day类型
(
 partition p19801217  values less than (date'1980-12-18')
);
```

#### --2.分区表中插入数据
```sql
insert into emp_d1 select * from emp;
commit;
```

#### --3.查看有几个分区
```sql
select * from user_tab_partitions where table_name = 'EMP_D1';
```


### --按年自动扩展分区
#### --1.创建自动扩展分区表(指定当前数据中最小的一个日期)
```sql
create  table emp_d2
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
interval(numtoyminterval(1, 'year'))
(
 partition p19801217  values less than (date'1980-12-18')
);
```

#### --2.分区表中插入数据
```sql
insert into emp_d2 select * from emp;
commit;
```

#### --3.查看有几个分区
```sql
select * from user_tab_partitions where table_name = 'EMP_D2';
```



### --按月自动扩展分区
#### --1.创建自动扩展分区表(指定当前数据中最小的一个日期)
```sql
create  table emp_d3
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
interval(numtoyminterval(1, 'month'))
(
 partition p19801217  values less than (date'1980-12-18')
);
```

#### --2.分区表中插入数据
```sql
insert into emp_d3 select * from emp;
commit;
```

#### --3.查看有几个分区
```sql
select * from user_tab_partitions where table_name = 'EMP_D3';
```


### --按天自动扩展分区：日期是这种形式：20251212
#### --1.创建自动扩展分区表(指定当前数据中最小的一个日期)
```sql
create  table emp_d4
(
 empno     int,
 ename     varchar2(20),
 job       varchar2(20),
 mgr       int,
 hiredate  int,
 sal       number,
 comm      number,
 deptno    int
)partition by range(hiredate)
interval(1)
(
 partition p19801217  values less than (19801218)
);
```

#### --2.分区表中插入数据
```sql
insert into emp_d4 
select 
       e.empno,
       e.ename,
       e.job,
       e.mgr,
       to_char(e.hiredate, 'yyyymmdd'),
       e.sal,
       e.comm,
       e.deptno
from emp e;
commit;
```

#### --3.查看有几个分区
```sql
select * from user_tab_partitions where table_name = 'EMP_D4';
```