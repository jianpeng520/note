## 1.map
select map('张三', 18, '李四',15);

## 2.size:统计map元素的个数
select name, score, size(score) from stu

## 3.显式map中的key
select name, score, map_keys(score) from stu;

## 4.显式map中的value
select name, score, map_values(score) from stu;

## 5.explode：可以炸数组和map (posexplode不能炸map)
select explode(score) from stu;

## 6.lateral view:临时表
select name, k, v from stu
lateral view explode(score) t as k,v;

解释：
explode(score)炸开map数据，使用lateral view临时存储数据，表名叫t
字段是k和v。 最后查询
## 7.来一个映射的例子：查询出每个人的平均分数和考试的科目数量
```
create table scores
(
id int,
score map<string,int>
)row format delimited fields terminated by ','
collection items terminated by '-'
map keys terminated by ':';
```
### Linux创建score.txt文件，
1,chinese:80-math:79
2,chinese:79-math:88-english:76
3,math:77-chinese:99
### hive插入创建的文件
`load data local inpath '/lx/scores.txt' into table scores;`

### 查询
```
select id, count(1), avg(fenshu)
from scores
lateral view explode(score) t as kemu, fenshu
group by id;
```
## 8：查询出每个用户，第一次操作系统，是哪年哪月哪日，显示用户的id和年月日时间
```
create table user_log
(
id int,
logs string
)row format delimited fields terminated by '\t';
```

### Linux创建user_log.txt
```
1	{"datas":[{"userid":1001,"eventtype":"click","content":"mainpage","time":1641119482},{"userid":1001,"eventtype":"click","content":"goods","time":1641314562},{"userid":1001,"eventtype":"click","content":"collection","time":1641208761},{"userid":1001,"eventtype":"click","content":"shopping","time":1641520194}]}
2	{"datas":[{"userid":4567,"eventtype":"click","content":"mainpage","time":1641517654},{"userid":4567,"eventtype":"click","content":"goods","time":1641319987},{"userid":4567,"eventtype":"click","content":"collection","time":1641708991},{"userid":4567,"eventtype":"click","content":"shopping","time":1641027531}]}
```
### hive插入
`load data local inpath '/lx/user_log.txt' into table user_log;`

①：json中提取用户id和时间
```
select 
get_json_object(logs,  '$.datas[*].userid'),
get_json_object(logs,  '$.datas[*].time')
from user_log;
```
②：使用正则去掉[和]  
`\\:转义符`
`\\[ : 变成 [`
`| : 或`
```
select 
regexp_replace(get_json_object(logs,  '$.datas[*].userid'), '\\[|\\]'  , ''),
regexp_replace(get_json_object(logs,  '$.datas[*].time'), '\\[|\\]'  , '')
from user_log;
```

③：使用split按逗号切割成数组
```
select 
split(regexp_replace(get_json_object(logs,  '$.datas[*].userid'), '\\[|\\]'  , ''), ','),
split(regexp_replace(get_json_object(logs,  '$.datas[*].time'), '\\[|\\]'  , ''), ',')
from user_log;
```
④：使用sort_array对时间进行升序排序

```
select 
split(regexp_replace(get_json_object(logs,  '$.datas[*].userid'), '\\[|\\]'  , ''), ','),
sort_array(split(regexp_replace(get_json_object(logs,  '$.datas[*].time'), '\\[|\\]'  , ''), ','))
from user_log;
```

⑤：取数组的0号元素
```
select 
split(regexp_replace(get_json_object(logs,  '$.datas[*].userid'), '\\[|\\]'  , ''), ',')[0],
sort_array(split(regexp_replace(get_json_object(logs,  '$.datas[*].time'), '\\[|\\]'  , ''), ','))[0]
from user_log;
```
⑥：使用cast把时间转换为数值型时间
```
select 
split(regexp_replace(get_json_object(logs,  '$.datas[*].userid'), '\\[|\\]'  , ''), ',')[0],
cast(sort_array(split(regexp_replace(get_json_object(logs,  '$.datas[*].time'), '\\[|\\]'  , ''), ','))[0] as bigint)
from user_log;
```
⑦：使用from_unixtime把时间戳转换为日期
```
select 
split(regexp_replace(get_json_object(logs,  '$.datas[*].userid'), '\\[|\\]'  , ''), ',')[0],
from_unixtime(cast(sort_array(split(regexp_replace(get_json_object(logs,  '$.datas[*].time'), '\\[|\\]'  , ''), ','))[0] as bigint))
from user_log;
```
## 9：查询出比赛胜利场次最多的国家名字
```
create table teams
(
dt string,
result map<string,int>
)row format delimited fields terminated by ','
collection items terminated by '-'
map keys terminated by ':';
```
### Linux创建teams.txt
2020/1/7,韩国:3-日本:4
2020/1/8,日本:2-澳大利亚:1
2020/1/9,韩国:1-澳大利亚:3
### 插入hive
`load data local inpath '/lx/teams.txt' into table teams;`

①：先使用lateral view 和 explode炸开map数据
```
select dt, guojia, fenshu 
from  teams
lateral view explode(result) v as guojia, fenshu
```
②：计算每个日期的最高得分
```
select dt, guojia, fenshu ,
       max(fenshu)over(partition by dt) as max_fen
from  teams
lateral view explode(result) v as guojia, fenshu
```
③：where筛选出每个日期最高得分的数据(在hive中，出现嵌套select，要起表别名)
```
select * from
(
 select dt, guojia, fenshu ,
       max(fenshu)over(partition by dt) as max_fen
 from  teams
 lateral view explode(result) v as guojia, fenshu
)t
where fenshu  = max_fen
```
④：统计每个国家的胜利场次
```
select guojia, count(1) as cnt from
(
 select dt, guojia, fenshu ,
       max(fenshu)over(partition by dt) as max_fen
 from  teams
 lateral view explode(result) v as guojia, fenshu
)t
where fenshu  = max_fen
group by guojia
order by cnt desc  limit 1;
```
## 10. uid是用户编号，dt是用户操作时间，用户操作需要一个session，
但是如果操作间隔超过30分钟，那么就会分配一个新的session，
查询出每个用户每天，都一共有多少个session产生。
```
create table user_event
(
uid int,
dt string
)row format delimited fields terminated by ',';
```
### Linux创建user_event.txt

```
1001,1641173111
1001,1641175434
1001,1641177659
1001,1641273990
1001,1641274391
1001,1641274412
1001,1641275029
1001,1641275684
1001,1641278765
1001,1641279754
1002,1641174235
1002,1641175434
1002,1641177865
1002,1641285743
1002,1641285873
1002,1641287665
1002,1641289801
1002,1641298721
1002,1641299981
1003,1641175621
1003,1641177651
1004,1641171231
1005,1641177958
1006,1641174521
1006,1641276565
1006,1641278791
1006,1641279891
1006,1641280975
1006,1641281123
1006,1641281232
1006,1641281253
```
### 插入hive
`load data local inpath '/lx/user_event.txt' into table user_event;`

①：时间戳怎么转成年月日
`select from_unixtime(cast(dt as bigint), 'yyyy-MM-dd') from user_event;`

②：使用lag偏移日期，目的是判断两个日期的时间间隔有没有超过30分钟(每个用户每天分组)
```
select 
uid, dt, 
from_unixtime(cast(dt as bigint), 'yyyy-MM-dd') as ymd,
lag(dt)over(partition by uid, from_unixtime(cast(dt as bigint), 'yyyy-MM-dd') order by dt) as dt2
from user_event
```
③：按每个用户每天分组（case when实现）
```
select 
uid, ymd,
sum(case when (dt - dt2)/60 >= 30 then 1 else 0 end) + 1
from
(
 select 
 uid, dt, 
 from_unixtime(cast(dt as bigint), 'yyyy-MM-dd') as ymd,
 lag(dt)over(partition by uid, from_unixtime(cast(dt as bigint), 'yyyy-MM-dd') order by dt) as dt2
 from user_event
)t
group by uid, ymd;
```
④：按每个用户每天分组（if实现）
```
select 
uid, ymd,
sum(if((dt - dt2)/60 >= 30, 1, 0)) + 1
from
(
 select 
 uid, dt, 
 from_unixtime(cast(dt as bigint), 'yyyy-MM-dd') as ymd,
 lag(dt)over(partition by uid, from_unixtime(cast(dt as bigint), 'yyyy-MM-dd') order by dt) as dt2
 from user_event
)t
group by uid, ymd;
```
## 11.在hive中，有一个if函数，它和case when的用法是一样的
```
select 
ename, sal,
if(sal >= 3000, 'A', 'B'),
if(sal >= 3000, 'A', if(sal >= 2000 , 'B' , 'C')),
if(sal >= 3000, 'A', if(sal >= 2000 , 'B' , if(sal >= 1500, 'C', 'D')))
from bigdata.emp;
```