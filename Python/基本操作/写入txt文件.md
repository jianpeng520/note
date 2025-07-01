## 1.两种写入方式
w:覆盖写入
a:追加写入

## 2.写入文件
```python
f=open('./2.txt', 'a' , encoding='utf-8')
f.write('我\n是\n谁\n')  #只能有1个参数
f.close()
```

--------------------读取json文件
#1.windows准备json文件   1.txt
{"name":"lilei","age":18,"sex":"男"}

### 1.1 读取json文件
```python
import json
f=open('./1.txt', encoding='utf-8')
rd=f.read()  #读取整个文件，形成一个json的字符串
print(rd)
d=json.loads(rd)  #解析json，变成字典
print(type(d))  #<class 'dict'>
print(d['name'])

f.close()
```

## 3.windows准备js文件   1.js
{"odata.metadata":"https://services.odata.org/V3/Northwind/Northwind.svc/$metadata#Products","value":[{"ProductID":1,"ProductName":"Chai","SupplierID":1,"CategoryID":1,"QuantityPerUnit":"10 boxes x 20 bags","UnitPrice":"18.0000","UnitsInStock":39,"UnitsOnOrder":0,"ReorderLevel":10,"Discontinued":false},{"ProductID":2,"ProductName":"Chang","SupplierID":1,"CategoryID":1,"QuantityPerUnit":"24 - 12 oz bottles","UnitPrice":"19.0000","UnitsInStock":17,"UnitsOnOrder":40,"ReorderLevel":25,"Discontinued":false}],"odata.nextLink":"Products/?$skiptoken=20"}

### 3.1 读取嵌套json
```python
import json
f=open('./1.js', encoding='utf-8')
rd=f.read()  #读取整个文件，形成一个json的字符串
print(rd)
d=json.loads(rd)  #解析json，变成字典
print(type(d))  #<class 'dict'>

#循环字典
for i in d['value']:
    #print(i)   #i也是一个字典
    print(i['ProductID'], i['ProductName'], i['UnitPrice'])
f.close()
```
