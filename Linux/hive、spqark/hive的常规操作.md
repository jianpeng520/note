## 1.map类型
{'语文': 99.9,  '英语':59.9,  '数学':99.9}
{'key':value}
## 2.array类型
['吃','喝','玩','乐']
## 3.建表
```
create table ods.stu2
(
 sno     int,
 sname   string,
 aihao   array<string>,
 score   map<string,  double>
)row format delimited fields terminated by '\t'  --字段和字段之间的分隔符用tab
collection items terminated by ',' --数组之间用 ,
map keys terminated by '/'  ;      --键值对之间用/
```
## 4.数据的花式插入方法
### --方法1：
```
insert into ods.stu2
select 
1,  '张三',  
array('吹','拉','弹','唱'), 
map('语文',cast(99.9 as double), '数学', cast(89.9 as double)); --追加写入
```

```
insert overwrite table ods.stu2
select 
1,  '张三',  
array('吹','拉','弹','唱'), 
map('语文',cast(99.9 as double), '数学', cast(89.9 as double)); --覆盖写入
```
#### 追加写入：
每执行一次，就会增加一次数据，就会在hdfs中生成一个文件
#### 覆盖写入:
覆盖原来的文件

`select * from ods.stu2;`
### --方法2：
--①：linux中新建文件    `vim  /lx/stu2.txt`
```
2	李四	游戏,唱k,吃香菜	数学/99,英语/59
3	小芳	游戏,唱k,吃菜	语文/99,英语/59
```
--②：在hive中执行以下语句
`load data local inpath '/lx/stu2.txt' into table ods.stu2;`        --追加写入
`load data local inpath '/lx/stu2.txt' overwrite into table ods.stu2;` --覆盖写入
select * from ods.stu2;

以上语句可以放在spark sql中执行