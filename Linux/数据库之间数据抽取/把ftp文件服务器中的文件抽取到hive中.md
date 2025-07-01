## 1.hive中建表
```
create table stu
(
sno   int,
sname string,
score double
)
row format delimited  fields terminated by ',';
```

## 2.linux中输入： 获取json模版
`python2  /opt/software/datax/bin/datax.py  -r  ftpreader   -w  hdfswriter`

## 3.linux编写json脚本    
`vim  /lx/ftp_to_hdfs.json`
```
{
    "job": {
        "content": [
            {
                "reader": {
                    "name": "ftpreader", 
                    "parameter": {
                        "column": [
                            {"index": 0,  "type": "long"},
							{"index": 1,  "type": "string"},
							{"index": 2,  "type": "double"}
                        ], 
                        "encoding": "UTF-8", 
                        "fieldDelimiter": ",", 
                        "host": "192.168.2.65", 
                        "password": "1234", 
                        "path": ["/"], 
                        "port": "21", 
                        "protocol": "ftp", 
                        "username": "root"
                    }
                }, 
                "writer": {
                    "name": "hdfswriter", 
                    "parameter": {
                        "column": [
							{"name":"sno",    "type":"int"},
							{"name":"sname" , "type":"string"},
							{"name":"score",  "type":"double"}
						], 
                        "compress": "", 
                        "defaultFS": "hdfs://localhost:8020", 
                        "fieldDelimiter": ",", 
                        "fileName": "stu", 
                        "fileType": "text", 
                        "path": "/user/hive/warehouse/bigdata.db/stu", 
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

## 4.linux运行json脚本   
`python2   /opt/software/datax/bin/datax.py  /lx/ftp_to_hdfs.json`