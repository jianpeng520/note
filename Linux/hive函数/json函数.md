## 1.json的样子
```
{'name':'张三', 'age':18}
{'name':'李四', 'age':19 , 'score':[{'yuwen':88},{'shuxue',99}]}
```
## 2.json函数
get_json_object: 一个函数只能提取一个字段，能提取嵌套json
                 提取方式 $.key.key.key, 就能获取value
				 
json_tuple:一个函数只能可以提取多个字段，  不能提取嵌套json
           提取方式 key1, key2    就能获取value
## 3.提取uid和dt
```
create table s01(
id int,
operation string
);

insert into s01 values
(1,'{"uid":1001,"event_type":"click","content":"mainPage","dt":1641103948}'),
(2,'{"uid":8673,"event_type":"click","content":"payment","dt":1641107664}');
```
### --方法1：
select 
get_json_object(operation, '$.uid'),
get_json_object(operation, '$.dt')
from s01;
### --方法2：
select 
json_tuple(operation, 'uid', 'dt')
from s01;
## 4.提取country和add
```
create table s02(
dt string,
nums string
);

insert into s02 values('2022-1-3','{"datas":[{"country":"india","add":74624},{"country":"england","add":198475},{"country":"usa","add":1029412}]}');

select 
get_json_object(nums, '$.datas[*].country'),
get_json_object(nums, '$.datas[*].add'),

get_json_object(nums, '$.datas[0].country')
from s02;
```
