## 1 可以自己去检查mysql中的数据库名
sqoop list-tables \
--connect jdbc:mysql://localhost:3306/db2 \
--username root \
--password 123456
