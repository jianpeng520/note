## 1.查看oracle ip地址
win + r
cmd
ipconfig
192.168.2.65

## 2.hive中建表
```
create table bigdata.dept
(
 deptno   int,
 dname    string,
 loc      string
)row format delimited  fields terminated by ',';
```

## 3.linux中输入： 获取json模版
`python2 /opt/software/datax/bin/datax.py -r oraclereader -w hdfswriter` 

## 4.linux编写json脚本    
`vim  /lx/oracle_to_hdfs.json`
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
                    "name": "hdfswriter", 
                    "parameter": {
                        "column": [
							{"name":"deptno", "type":"int"},
							{"name":"dname" , "type":"string"},
							{"name":"loc",    "type":"string"}
						], 
                        "compress": "", 
                        "defaultFS": "hdfs://localhost:8020", 
                        "fieldDelimiter": ",", 
                        "fileName": "dept", 
                        "fileType": "text", 
                        "path": "/user/hive/warehouse/bigdata.db/dept", 
                        "writeMode": "append"
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
`python2   /opt/software/datax/bin/datax.py     /lx/oracle_to_hdfs.json`