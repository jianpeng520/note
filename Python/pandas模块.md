## 1.cmd中下载pandas
win + r
cmd
pip install pandas
pip list
## 2.读取excel文件， 查询需要的行
```python
import pandas as pd

#读取excel文件， 生成二维数据DataFrame
df = pd.read_excel(r'C:\Users\baozi\Desktop\python素材\e_d.xlsx',  sheet_name='emp')
#print(df)
#print(df.head(2))         #读取前2行数据
#print(df.tail(2))         #读取后2行数据
print(df.iloc[0])          #读取第1行数据
print(df.iloc[0,0])        #读取第1行第1列的数据
print(df.iloc[2:5])        #读取第3行到第5行的数
```
## 3.pandas中的排序sort_index 和 sort_value
```python
import pandas as pd

df = pd.read_excel(r'C:\Users\baozi\Desktop\python素材\e_d.xlsx',  sheet_name='emp')
#print(df.sort_index())                #按索引进行升序排序
#print(df.sort_index(ascending=False)) #按索引进行降序排序

#print(df.sort_values(['sal'], ascending=True))   #按工资进行升序排序
#print(df.sort_values(['sal'], ascending=False))   #按工资进行降序排序

print(df.sort_values(['deptno', 'sal'],  ascending=[False, True]))  #按部门进行降序排序，按工资进行升序排序
```
## 4.去重drop_duplicates
```python
import pandas as  pd

df = pd.read_excel(r'C:\Users\baozi\Desktop\python素材\e_d.xlsx',  sheet_name='emp_cp')
#print(df)
#print(df.drop_duplicates(keep = 'first'))  #保留第1个重复值
#print(df.drop_duplicates(keep = 'last'))    #保留最后一个重复值
#print(df.drop_duplicates(keep = False))     #删除重复值

#print(df['deptno'].drop_duplicates(keep = 'first'))           #单个字段去重
print(df[['deptno', 'job']].drop_duplicates(keep = 'first'))   #多个字段去重
```
## 5.pandas实现sql表连接效果merge
```python
import pandas as  pd
emp = pd.read_excel(r'C:\Users\baozi\Desktop\python素材\e_d.xlsx',  sheet_name='emp')
dept = pd.read_excel(r'C:\Users\baozi\Desktop\python素材\e_d.xlsx',  sheet_name='dept')
#print(pd.merge(emp,dept, on = 'deptno', how = 'inner'))    #实现内连接效果
#print(pd.merge(emp,dept, on = 'deptno', how = 'left'))     #实现左连接效果
#print(pd.merge(emp,dept, on = 'deptno', how = 'right'))    #实现右连接效果
#print(pd.merge(emp,dept, on = 'deptno', how = 'outer'))    #实现全连接效果
print(pd.merge(emp,dept, left_on='deptno', right_on='deptno', how = 'inner'))  #内连接
```
## 6.pandas实现sql的上下拼接效果concat
```python
import pandas as  pd
d1 = pd.read_excel(r'C:\Users\baozi\Desktop\python素材\e_d.xlsx',  sheet_name='dept')
d2 = pd.read_excel(r'C:\Users\baozi\Desktop\python素材\e_d.xlsx',  sheet_name='dept')
#print(pd.concat([d1, d2], axis = 0))       #上下拼接
print(pd.concat([d1, d2], axis = 1))   #按行索引进行左右拼接
```
## 7.批量合并excel文件
```python
import os
import pandas as pd

ml=os.listdir(r'C:\Users\baozi\Desktop\python素材\合并excel文件') #列出目录下的所有文件名
print(ml)  #结果为['1.xlsx', '2.xlsx', '3.xlsx']

li = []  #定义空列表，用来存储数据
for i in ml:
    print(i) # i就是单个文件名 i = 1.xlsx
    df = pd.read_excel(rf'C:\Users\baozi\Desktop\python素材\合并excel文件\{i}', sheet_name = 'sheet1')
    #print(df)  #读取每个文件的内容
    li.append(df) #把二维数据追加到列表中
    df2=pd.concat(li, axis = 0)
    print(df2)
    df2.to_excel(r'C:\Users\baozi\Desktop\python素材\合并excel文件\结果.xlsx', index = False)
```
## 8.批量合并csv文件
```python
import os
import pandas as pd

ml=os.listdir(r'C:\Users\baozi\Desktop\python素材\合并csv文件') #列出目录下的所有文件名
#print(ml)  #结果为['1.csv', '2.csv', '3.csv']
li = []  #定义空列表
for i in ml: # i = 1.csv
    df = pd.read_csv(rf'C:/Users/baozi/Desktop/python素材/合并csv文件/{i}', encoding='gbk')
    li.append(df)
df2=pd.concat(li, axis = 0)
#print(df2)
df2.to_csv(r'C:/Users/baozi/Desktop/python素材/合并csv文件/结果.csv', index = False, encoding='gbk')
```
## 9.拆分excel文件
```python
import pandas as pd

df = pd.read_excel(r'C:\Users\baozi\Desktop\python素材\拆分excel文件\合并.xlsx', sheet_name= 'Sheet1')
df2 = df.groupby('部门')
print(df2) # 结果是一个对象<pandas.core.groupby.generic.DataFrameGroupBy object at 0x16023E70>
for i,j in df2:
    print(i)  #i是分组字段中的数据, j是该组中的所有数据
    j.to_excel(rf'C:\Users\baozi\Desktop\python素材\拆分excel文件\{i}.xlsx', index = False)
```
## 10.拆分csv文件
```python
import pandas as pd

df = pd.read_csv(r'C:\Users\baozi\Desktop\python素材\拆分csv文件\合并.csv',encoding='gbk')
df2 = df.groupby('班级')
print(df2) # 结果是一个对象<pandas.core.groupby.generic.DataFrameGroupBy object at 0x16023E70>
for i, j in df2:
    #print(j)  #i是分组字段中的数据, j是该组中的所有数据
    j.to_csv(rf'C:\Users\baozi\Desktop\python素材\拆分csv文件\{i}.csv', index = False,encoding='gbk')
```

