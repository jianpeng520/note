## 准备工作
a.启动hadoop服务： `start-all.sh`
b.检查进程数(>=6)：`jps`
b.启动hive元数据服务：`hive service metastore &`
c.linux中输入：`hive`
d.在hive窗口中，一键执行常用开关命令
  `set   hive.cli.print.current.db=true;`   --显示数据库名字
  `set   hive.exec.mode.local.auto=true;`   --开启本地模式
  `set   hive.resultset.use.unique.column.names=false;`--不显示表名，只显示字段名
## 操作hive的三种方法
### 第1种： 在cli操作
#### 0.linux中输入：
hive
#### 1.创建数据库
create database ods;
create database if not exists ods;
#### 2.使用数据库
use ods;
#### 3.数据类型
int:    整数类型，   4
bigint: 长整数类型， 8   他比int能存储更多位数 
float:  单精度       4
double: 双进度       8   他比float能存储更多的小数位
decimal:自定义小数， 和number的使用方法一样
string: 字符串，字符和日期一般在hive中都用string
#### 4.在数据库中创建表格(if not exists可以忽略不写)
```
create  table  if not exists stu
(
 sno       int     comment  '学号',
 sname     string  comment  '姓名',
 score     float,
 dt        string
)comment '学生表';
```
#### 5.查看表的字段信息
desc ods.stu;
#### 6.查看建表信息
show create table ods.stu;

他有一个这样的信息：
LOCATION
  'hdfs://localhost:8020/user/hive/warehouse/ods.db/stu'
  
我们可以在浏览器中输入 ：
192.168.222.132:50070   然后点击
/user/hive/warehouse/ods.db/stu
#### 7.插入数据
insert into stu values
(1, '张三', 88.8, '2021-1-1'),
(2, '李四', 98.8, '2022-1-1'),
(3, '黑七', 97.8, '2023-1-1');
#### 8.不能使用delete， truncate可以正常使用，drop可以正常使用
delete from ods.stu; 报错
#### 9.删除数据库
drop database ods cascade;  当数据库中有内容的时候，删除要加cascade