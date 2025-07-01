## 1.启动spark虚拟机，其他都不用管

## 2.linux中进入mysql
mysql  -uroot -p123456
show databases;
## 3.cmd中下载PyMysql
win + r
cmd
pip install  PyMysql
pip list
## 4.python在mysql中建表
```python
import pymysql

conn = pymysql.connect(host='192.168.222.132', user='root', password='123456', database='ob',
                    charset='utf8')
print(conn)  #<pymysql.connections.Connection object at 0x02100050>

cur=conn.cursor()  #创建游标
sql="""
      create table if not exists emp2
      (
       empno    int,
       ename    varchar(30),
       sal      double
      )
"""  
cur.execute(sql) #游标执行sql语句

conn.close()  #关闭连接
```
