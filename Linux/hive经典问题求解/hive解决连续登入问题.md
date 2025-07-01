有一张表u，有2个字段，name：姓名 和 dt：登入时间，查询连续三天及以上登入的用户名
## a.建表
```
create table u
(
 name   string,
 dt     date
);
```
## b.插入数据
```
insert into u values
('张三', '2025-1-1'),
('张三', '2025-1-2'),
('张三', '2025-1-3'),
('李四', '2025-1-1'),
('李四', '2025-1-2'),
('李四', '2025-1-4'),
('李四', '2025-1-5');
```
## c.查询
`set mapred.reduce.tasks=-1;`    -- -1是自动判断
```sql
select name from
(
select 
name, dt, 
date_add(dt, -row_number()over(partition by name order by dt)) as r
from u
)t
group by name, r
having count(1) >= 3;
```