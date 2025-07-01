#create #alter #drop
## 创建表create table
语法：
create table  表名(
 字段1  类型1，
 字段2  类型2，
 、、、
 字段n 类型n
)
创建一个王者英雄人物表格
```sql
create table 王者
(
 英雄id       int,
 英雄名字     varchar2(30),
 所属分路     varchar2(30),
 强度级别     varchar2(30),
 首次上线时间 date
);
```

## 修改表alter table
语法：
1.修改表的列名  alter table  表名 rename  column 旧列名 <font color="#92d050">into</font> 新列名
2.修改表的列类型 alter table 表名 <font color="#92d050">modify</font> 列名 数据类型
<font color="#92d050">3.删除列名</font> alter table 表名 drop <font color="#92d050">column</font> 列名
<font color="#92d050">4.新增列名</font> alter table 表名 add 列名 数据类型
<font color="#92d050">5.修改表名</font> alter table 表名 rename 旧表名 <font color="#92d050">to</font> 新表名

```sql
1.把字段首次上线时间重命名为上线时间
alter table 王者 rename  首次上线时间 into 上线时间
更正：
alter table 王者 rename column 首次上线时间 to 上线时间;

2.修改数据类型：把性别的数据类型改成varchar2
alter table 王者 modify sex varchar2(2)

3.删除性别字段
alter table 王者 remove sex
更正：
alter table 王者 drop column sex；

4.给王者表格新增一个字段性别
alter table 王者 add sex
更正：
alter table 王者 add sex varchar2(2)

5.把王者表名重名为王者英雄
alter table 王者 rename 王者英雄
更正：
alter table 王者 rename  to 王者英雄

```


## 删除表drop table
语法：
drop table 表名;

删除王者表格
drop table 王者