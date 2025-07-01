就是把linux命令封装在一起，批量执行
通常会使用shell脚本封装一些sql等一些脚本语言
-----1.使用shell脚本  vim   /lx/s1.sh
#!/bin/bash

mkdir -p  /year/month/day               ##创建递归文件夹
touch  /year/month/day/1.txt            ##创建文件
echo  haha  >>  /year/month/day/1.txt   ##文件写入数据
wc -l /year/month/day/1.txt             ##统计文件的行数
wc -c /year/month/day/1.txt             ##统计文件的大小


-----1.1 调用脚本
--方法1：
bash   /lx/s1.sh         ##执行脚本


--方法2：
chmod  755  /lx/s1.sh    ##给脚本授权
/lx/s1.sh                ##执行脚本




-----2.有1个文件1.txt, 他有如下内容。 统计每个字母的个数
a b c
a a a
c c c
a b c
a a a
c c c
a b c
a a a
c c c
e f g



--2.1 编写shell脚本  vim /lx/s2.sh
#!/bin/bash

awk  '{print $1}' /lx/1.txt  >>  /lx/tmp.txt  ##提取第1列，写入临时文件
awk  '{print $2}' /lx/1.txt  >>  /lx/tmp.txt  ##提取第2列，写入临时文件
awk  '{print $3}' /lx/1.txt  >>  /lx/tmp.txt  ##提取第3列，写入临时文件

`#tmp.txt文件中就有一列字母了`

sort  /lx/tmp.txt    |    uniq -c  |    sort -nr
##排序                  |  去重统计 |    按数字降序排序

rm -rf /lx/tmp.txt     ##删除临时文件


--2.2 执行shell脚本 
bash  s2.sh