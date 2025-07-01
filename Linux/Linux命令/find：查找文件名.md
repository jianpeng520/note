--1.语法
find  路径地址    -name   文件名
find  路径地址    -size   文件大小

--2.查找所有的txt文件
find  /  -name  "*.txt"

--3.把/lx目录下所有的txt文件打个包
```
tar -zcf  t1.tar.gz   `find  /lx  -name  "*.txt"`
```

`:反引号，和四则运算的括号一样，反引号内的内容是一个整体


--4.查找在10k以上的文件
find  /    -size   +10k
find  /    -size   -10k
find  /    -size   +10M


--5.查找txt文件中10k以上的 
-a:并且
-o:或者

find  /  -name  "*.txt"  -a  -size  +10k