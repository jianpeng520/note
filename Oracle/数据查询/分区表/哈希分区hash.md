#partition-hash
## 哈希分区hash
### --1.语法
<span style="background:#d3f8b6">create table 分区表名</span>
<span style="background:#d3f8b6">(</span>
<span style="background:#d3f8b6"> 字段名     数据类型,</span>
<span style="background:#d3f8b6"> .....</span>
<span style="background:#d3f8b6">)partition by hash(分区字段名)</span>
<span style="background:#d3f8b6">(</span>
<span style="background:#d3f8b6"> partition  分区名1  ,</span>
<span style="background:#d3f8b6"> partition  分区名2 ,</span>
<span style="background:#d3f8b6"> ........</span>
<span style="background:#d3f8b6">);   </span>           

### --2.创建哈希分区表
```sql
create table stu_hash
(
 sno     int,
 sname   varchar2(20)
)partition by hash(sno)
(
 partition p1,
 partition p2,
 partition p3,
 partition p4
);
```


### --3.哈希分区表中插入数据 (直接抄就行)
```sql
begin   --程序开始
  for i in 1 .. 10000 loop  --for循环，从1开始，一直到10000，循环10000次，i就是变量 。i=1,2,3..10000
     insert into stu_hash values(i, '张三');  --这句话会被执行1万次，i=1,2,3..10000
  end loop; 
  commit;
end;      --程序结束
```


### --4.查询每个分区的数据
```sql
select count(1) from stu_hash;   --数据总量：1w

select count(1) from stu_hash partition(p1);   --数据总量：2471
select count(1) from stu_hash partition(p2);   --数据总量：2527
select count(1) from stu_hash partition(p3);   --数据总量：2521
select count(1) from stu_hash partition(p4);   --数据总量：2481
```
