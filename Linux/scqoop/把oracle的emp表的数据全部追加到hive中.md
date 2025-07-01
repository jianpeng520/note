```shell
#EMP表是oracle中的，要大写
#-m 1 启动一个map，如果不写，就是4个map， sqoop只有map，没有reduce
#删除了hive的 bigdata.emp
#如果hive中没有emp，sqoop抽数据会自动创建emp表
#--delete-target-dir \      删除/user/root/EMP 临时文件夹
#抽取数据流向过程：   oracle  ->      /user/root/EMP 临时文件  ->    hdfs的目标路径 bigdata.emp中
```

sqoop import \
--hive-import \
--connect jdbc:oracle:thin:@192.168.1.76:1521/ORCL \
--username scott \
--password 123456 \
--table EMP \
--hive-database bigdata \
--delete-target-dir \
--fields-terminated-by ','  -m 1