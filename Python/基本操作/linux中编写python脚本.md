`vim  /lx/stu_info.py`
```python
#encoding=utf-8
import sys

#sys.stdin会逐行读取外部文件传进来的数据， 存储在变量i中
for i in sys.stdin:  # i = 'lixiaoming  411326199402110030'  。 i是一个字符串
    i = i.strip().split('\t')   # i = ['lixiaoming', '411326199402110030']  。i被split切割成了列表

    name=i[0]   # name = 'lixiaoming'
    card=i[1]   # card = '411326199402110030'

    #身份证号码有15位和18位
    if len(card) == 15:   #如果是15位，性别在倒数第1位: card[-1]， 偶数为女，奇数为男
        n = int(card[-1])  #取出倒数第1位，并转成整数类型
        if n % 2 == 0:   # %是计算余数，相当于mod
            print('\t'.join([name, card, '女']))
        else:
            print('\t'.join([name, card, '男']))
            
    elif len(card) == 18: #如果是18位，性别在倒数第2位: card[-2]， 偶数为女，奇数为男
        n = int(card[-2])  # 取出倒数第2位，并转成整数类型
        if n % 2 == 0:  # %是计算余数，相当于mod
            print('\t'.join([name, card, '女']))
        else:
            print('\t'.join([name, card, '男']))
            
    else:  #长度即不是15，也不是18，证明不合法
        print('\t'.join([name, card, '号码不合法']))
```
## 3.在linux中验证py脚本是否正常
cat /lx/stu_info.txt  |  python3    /lx/stu_info.py
## 4.启动hadoop 服务 和 hive元数据服务
## 5.在hive中添加py文件
add file  /lx/stu_info.py;
## 6、hive中建表
```sql
create table ods.stu_info
(
 name string,
 card string
)
row format delimited fields terminated by '\t';
```
## 7.hive中执行
`load data local inpath '/lx/stu_info.txt' overwrite into  table  ods.stu_info;`
## 8.hive中使用udf自定义函数
```sql
select transform(name, card) using 'python3  stu_info.py' as (name, card, sex)
from ods.stu_info;
```
