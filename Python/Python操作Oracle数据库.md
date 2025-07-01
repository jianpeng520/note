## 1、检查oracle的版本（查看版本号和位数）
```sql
select * from v$version
```
32位： TNS for 32-bit Windows: Version 11.2.0.1.0 - Production
64位： TNS for 64-bit Windows: Version 11.2.0.1.0 - Production
## 2、检查python的版本和位数
win + r
cmd
python
quit()
``
python使用版本： Python 3.7.9  
32位：MSC v.1900 32 bit (Intel)
64位：MSC v.1900 64 bit (Intel)

oracle的<font color="#ff0000">位数必须</font>和python的位数<font color="#ff0000">一致</font>。如果位数不一致，建议卸载python，重新安装

## 3、cmd窗口中下载cx_Oracle
win + r  
cmd
pip  install  cx_Oracle

## 4.cmd窗口检查是否下载
win + r  
cmd
pip list
## 5、如果下载pip cx_Oracle报错(黄色的警告不用管) ，异常问题
##如果上面的cx_Oracle成功，这一步跳过
`python -m pip uninstall pip`            ##卸载pip
`python -m ensurepip`                    ##清空临时数据
`python -m pip install pip==21.3.1`      ##重新下载pip

`pip  install  cx_Oracle` 
## 6.cmd中验证oracle连接(这一步能成功，代表cx_Oracle能正常使用)
win + r  
cmd
python
```python
import cx_Oracle
ora=cx_Oracle.connect('scott/123456@localhost:1521/orcl')
print(ora)  #<cx_Oracle.Connection to scott@localhost:1521/orcl> 代表成功
```
## 7.pycharm中连接oracle (解释器位置没选对)
```python
import cx_Oracle
ora=cx_Oracle.connect('scott/123456@localhost:1521/orcl')
print(ora)  #<cx_Oracle.Connection to scott@localhost:1521/orcl> 代表成功
```
## 8.python操作oracle
```python
import cx_Oracle
ora=cx_Oracle.connect('scott/123456@localhost:1521/orcl') #创建连接
print(ora)

cur=ora.cursor()   #创建游标
sql='select ename, sal, deptno from emp'   #准备sql语句
cur.execute(sql) #游标执行sql语句
#datas=cur.fetchone() #游标取第1行数据
datas=cur.fetchall()  #游标取所有数据,变成一个列表，每行数据就是元组，每个字段的值就是元组中的一个元素
print(datas)

for i in datas:
    print(i)

ora.close() #关闭连接
```
