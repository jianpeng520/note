	python读取接口数据(json格式),把数据写入到oracle数据库中
## 1.准备url
https://services.odata.org/V3/Northwind/Northwind.svc/Products/?$format=json



## 2.cmd中下载requests模块
win + r
cmd
pip install requests
pip list


## 3.在oracle中，如何查看表格是否存在
select count(1) from user_tables where table_name = 'PRODUCT'; --结果=1 or =0


## 4.oracle中如何查询一个单引号
select '''' from dual;  --两个单引号会转义，变成一个单引号                           '''
## 5.连接oracle,如果oracle中有product表格，我们就清空数据，如果不存在这个表格，就创建表格
```python
import cx_Oracle

ora=cx_Oracle.connect('scott/123456@localhost:1521/orcl') #连接oracle
cur=ora.cursor() #创建游标
sql="select count(1) from user_tables where table_name = 'PRODUCT'"  #准备sql语句
cur.execute(sql) #游标执行sql语句
datas=cur.fetchone()  #获取第1行数据
print(datas)     #datas=(1,) 是一个元组
data=datas[0]    #只要元组的0号元素
print(data)      #data=1

#如果data=1,证明PRODUCT表格在oracle中是存在的，那么就清空数据，否则就创建PRODUCT表格
if data == 1:
    #清空数据
    cur.execute('truncate table product')
else:
    #创建表格
    sql = """
         create table product
         (
          ProductID    int,
          ProductName  varchar2(100),
          UnitPrice    number
         )
    """
    cur.execute(sql)

ora.close()  #关闭连接
```
## 6.使用requests模块，读取网页接口数据

```python
import requests
import json

#准备网址
url='https://services.odata.org/V3/Northwind/Northwind.svc/Products/?$format=json'
#使用requests向浏览器发送请求
response=requests.get(url)
print(response)   #<Response [200]> 代表请求成功
#以text的方式获取数据，是一个字符串json格式
datas=response.text
print(datas)  # {"odata.metucts","value":[{"ProductID":1,"},{"ProductID":2,"}]
#使用json解析datas，把他变成一个字典
d=json.loads(datas)  #变成了字典
print(d) # {'odata.metucts','value":[{'ProductID':1,'},{'ProductID':2,'}]
#获取k='value"  所对应的v值
for i in d['value']:
    #print(i)  # i={'ProductID': 1, 'ProductName': 'Chai'..}
    ProductID=i['ProductID']     #结果为产品id = 4
    ProductName2=i['ProductName'] #结果为产品名字= Chef Anton's  。1个单引号写在oracle中会报错
    ProductName=ProductName2.replace("'", "''") #把一个单引号变成两个单引号 Chef Anton''s
    UnitPrice=i['UnitPrice']     #结果为产品价格=18.0000
    print(ProductID, ProductName, UnitPrice)  #打印结果为：4 Chef Anton''s Cajun Seasoning 22.0000
```

## 7.完整步骤
```python
import requests
import json
import cx_Oracle

#①--oracle部分操作
ora=cx_Oracle.connect('scott/123456@localhost:1521/orcl') #连接oracle
cur=ora.cursor() #创建游标
sql="select count(1) from user_tables where table_name = 'PRODUCT'"  #准备sql语句
cur.execute(sql) #游标执行sql语句
datas=cur.fetchone()  #获取第1行数据
print(datas)     #datas=(1,) 是一个元组
data=datas[0]    #只要元组的0号元素
print(data)      #data=1

#如果data=1,证明PRODUCT表格在oracle中是存在的，那么就清空数据，否则就创建PRODUCT表格
if data == 1:
    #清空数据
    cur.execute('truncate table product')
else:
    #创建表格
    sql = """
         create table product
         (
          ProductID    int,
          ProductName  varchar2(100),
          UnitPrice    number
         )
    """
    cur.execute(sql)

#②--requests部分操作
#准备网址
url='https://services.odata.org/V3/Northwind/Northwind.svc/Products/?$format=json'
#使用requests向浏览器发送请求
response=requests.get(url)
print(response)   #<Response [200]> 代表请求成功
#以text的方式获取数据，是一个字符串json格式
datas=response.text
print(datas)  # {"odata.metucts","value":[{"ProductID":1,"},{"ProductID":2,"}]
#使用json解析datas，把他变成一个字典
d=json.loads(datas)  #变成了字典
print(d) # {'odata.metucts','value":[{'ProductID':1,'},{'ProductID':2,'}]
#获取k='value"  所对应的v值
for i in d['value']:
    #print(i)  # i={'ProductID': 1, 'ProductName': 'Chai'..}
    ProductID=i['ProductID']     #结果为产品id = 4
    ProductName2=i['ProductName'] #结果为产品名字= Chef Anton's  。1个单引号写在oracle中会报错
    ProductName=ProductName2.replace("'", "''") #把一个单引号变成两个单引号 Chef Anton''s
    UnitPrice=i['UnitPrice']     #结果为产品价格=18.0000
    #print(ProductID, ProductName, UnitPrice)  #打印结果为：4 Chef Anton''s Cajun Seasoning 22.0000

    #把ProductID, ProductName, UnitPrice写入oracle中
    sql=f"insert into product values({ProductID}, '{ProductName}',{UnitPrice})"
    cur.execute(sql)

ora.commit() #提交数据
ora.close()  #关闭连接

```

