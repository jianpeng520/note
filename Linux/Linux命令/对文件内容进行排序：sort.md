```--先创建练习文件
vi  /lx/4.txt
a
b
c
a
a
a
a
a
a
c
b
b
b
a
a
a
a
```
--2.对文件进行内容进行升序排序
sort  /lx/4.txt

--3.对文件进行内容进行降序排序
sort -r /lx/4.txt

--4.对文件内容去重
uniq  /lx/4.txt            ##不完美去重
sort  /lx/4.txt | uniq     ##完美去重

--5.统计每个字母的个数
sort  /lx/4.txt | uniq  -c