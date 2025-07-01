## 1.linux输入：登入mysql
mysql -uroot -p123456

## 2.mysql中输入
`create database ob;`
`use ob;`
```
create table if not exists ob.emp
(
 empno    int,
 ename    varchar(30),
 job      varchar(30),
 mgr      int,
 hiredate varchar(30),
 sal      double,
 comm     double,
 deptno   int
);
```
## 3.linux中输入： 获取json模版
`python2 /opt/software/datax/bin/datax.py -r hdfsreader -w mysqlwriter` 


4.linux编写json脚本    vim  /lx/hdfs_to_mysql.json

```
{
    "job": {
        "content": [
            {
                "reader": {
                    "name": "hdfsreader", 
                    "parameter": {
                        "column": [
							{"index" : 0  , "type" : "long"},
							{"index" : 1  , "type" : "string"},
							{"index" : 2  , "type" : "string"},
							{"index" : 3  , "type" : "string"},
							{"index" : 4  , "type" : "string"},
							{"index" : 5  , "type" : "double"},
							{"index" : 6  , "type" : "string"},
							{"index" : 7  , "type" : "long"}
						], 
                        "defaultFS": "hdfs://localhost:8020", 
                        "encoding": "UTF-8", 
                        "fieldDelimiter": ",",
						"nullFormat":"\\N",
                        "fileType": "text", 
                        "path": "/user/hive/warehouse/bigdata.db/emp"
                    }
                }, 
                "writer": {
                    "name": "mysqlwriter", 
                    "parameter": {
                        "column": ["*"], 
                        "connection": [
                            {
                                "jdbcUrl": "jdbc:mysql://192.168.222.132:3306/ob", 
                                "table": ["emp"]
                            }
                        ], 
                        "password": "123456", 
                        "preSql": ["truncate table emp"], 
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
`python2   /opt/software/datax/bin/datax.py    /lx/hdfs_to_mysql.json`
