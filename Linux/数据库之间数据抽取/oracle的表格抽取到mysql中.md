## 1.查看oracle ip地址
win + r
cmd
ipconfig
192.168.2.65
## 2.mysql中创建表
`mysql -uroot -p123456`

`use ob;`
```
create table if not exists ob.dept
(
 deptno   int,
 dname    varchar(30),
 loc      varchar(30)
);
```

## 3.linux中输入： 获取json模版
`python2 /opt/software/datax/bin/datax.py -r oraclereader -w mysqlwriter` 
## 4.linux编写json脚本    
`vim  /lx/oracle_to_mysql.json`
```
{
    "job": {
        "content": [
            {
                "reader": {
                    "name": "oraclereader", 
                    "parameter": {
                        "column": [], 
                        "connection": [
                            {
                                "jdbcUrl": ["jdbc:oracle:thin:@192.168.2.65:1521/orcl"], 
                                "querySql": ["select * from dept where deptno != 10"]
                            }
                        ], 
                        "password": "123456", 
                        "username": "scott"
                    }
                }, 
                "writer": {
                    "name": "mysqlwriter", 
                    "parameter": {
                        "column": [
						   "deptno",
						   "dname",
						   "loc"
						], 
                        "connection": [
                            {
                                "jdbcUrl": "jdbc:mysql://192.168.222.132:3306/ob", 
                                "table": ["dept"]
                            }
                        ], 
                        "password": "123456", 
                        "preSql": ["truncate table dept"], 
                        "session": [], 
                        "username": "root", 
                        "writeMode": "insert"
                    }
                }
            }
        ], 
        "setting": {
            "speed": {
                "channel": "1"
            }
        }
    }
}
```
## 5.linux运行json脚本   
`python2   /opt/software/datax/bin/datax.py     /lx/oracle_to_mysql.json`