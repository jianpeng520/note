我们这个克隆机，jdbc模式下不能用本地模式，所以会走集群模式，有点慢，但是jdbc是有颜色的工具
## 1.新开一个窗口，linux中启动hive远程服务
`hiveserver2`

稍等一会，会显示4个 Hive Session ID ， 超过4个都会异常

这个窗口不用管他，也不用关闭
超过4个，就ctrl+C中断，重新来
## 2.再开启一个新会话窗口，然后使用beeline连接hive
beeline color=true
!connect jdbc:hive2://localhost:10000

用户：root
密码：123456
## 3.创建数据库
create database ods;
create database if not exists ods;
## 4.使用数据库
use ods;
## 5.在数据库中创建表格(if not exists可以忽略不写)
```
create  table  if not exists stu
(
 sno       int     comment  '学号',
 sname     string  comment  '姓名',
 score     float,
 dt        string
)comment '学生表';
```
## 6.插入数据
```
insert into stu values
(1, '张三', 88.8, '2021-1-1'),
(2, '李四', 98.8, '2022-1-1'),
(3, '黑七', 97.8, '2023-1-1');
```
## 7.查询
select * from stu;