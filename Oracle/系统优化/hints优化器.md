#hints
## hints优化器
--1.三种关联机制：当两表使用<font color="#ff0000">表连接</font>的时候，<span style="background:#d3f8b6">oracle会自己先试一遍这三种机制，然后选择一种最优的方案来执行</span>

<font color="#ff0000">嵌套循环(NL)</font>：从1张表中选择第1条数据，去和另一张表的每行数据进行逐行比对，从1张表中选择第2条数据，去和另一张表的每行数据进行逐行比对。

<font color="#ff0000">哈希连接(hash)</font>：会把表格转换为哈希表结构，然后在内存中完成比对。使用大表中没有索引的场景

<font color="#ff0000">排序合并(sort merge)</font>：两张表先进行排序，从1张表中选择第1条数据，去和另一张表的每行数据进行逐行比对，从1张表中选择第2条数据，去和另一张表的每行数据进行逐行比对

--2.工作中一般用哪种关联机制
在使用表连接的时候，oracle会自己先试一遍这三种机制，然后选择一种最优的方案来执行。所以，一般我们工作中选用的是默认的关联机制



--3.创建大表
```sql
begin
  for i in 1 .. 100000 loop
     insert into emp1 select * from emp; --这条语句会执行10w次
  end loop;
  commit;
end;
```

### 哈希连接：
语法   ：<font color="#ff0000"> select /*+ USE_HASH(e,d) */ *  </font> 

--4.使用表连接,查看执行计划
下面的语句中，oracle经过hints优化器后，选择的是hash join cost=2494, time = 30s

```sql
select * 
  from emp1 e
  join dept d
    on e.deptno = d.deptno;


select /*+ USE_HASH(e,d) */ *     
  from emp1 e
  join dept d
    on e.deptno = d.deptno;
```

### 嵌套循环连接
语法：<font color="#ff0000">select /*+ USE_NL(e,d) */ * </font>
--5.强制使用嵌套循环连接  cost=9900      time=2min
```sql
select /*+ USE_NL(e,d) */ * 
  from emp1 e
  join dept d
    on e.deptno = d.deptno;

```

### 排序合并连接
--6.强制使用排序合并连接  cost=84520      time=16min
```sql
select /*+ USE_MERGE(e,d) */ * 
  from emp1 e
  join dept d
    on e.deptno = d.deptno;
```



### 改变表扫描的先后顺序leading
默认是先扫描小表

```sql
--cost=2494
select * 
  from emp1 e
  join dept d
    on e.deptno = d.deptno;
    
    
--强制扫描大表 cost=21333
select /*+ LEADING(e,d) */ * 
  from emp1 e
  join dept d
    on e.deptno = d.deptno;
```
    
    
### 开并行,加快查询速度:parallel
开并行，就是抢夺更多资源去执行sql，具体开几个并行度，得看服务器资源

```sql
--没开并行度：cost=2494
select * 
  from emp1 e
  join dept d
    on e.deptno = d.deptno;
    
    
--开并行度：cost=5 。一般设为4,8,16
select /*+ parallel(8) */ * 
  from emp1 e
  join dept d
    on e.deptno = d.deptno;
```
