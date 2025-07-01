对文件或文件夹的内容进行检索

-------文件准备   vi  /lx/3.txt
90821301
adsfhozpk
98901shq89
qpz7837oui
a
A
123bac
432qaa


1.查询包含数字的数据：              grep     '[0-9]'  /lx/3.txt
2.查询包含字母的数据：              grep     '[a-z]'  /lx/3.txt
3.查询包含字母的数据,不区分大小写： grep -i  '[a-z]'  /lx/3.txt
4.查询以数字开头的记录：grep   '^[0-9]'   /lx/3.txt
5.查询以字母结尾的记录：grep   '[0-9]$'   /lx/3.txt
6.查询包含a的记录：              grep     'a'  /lx/3.txt
7.查询只有a的记录：              grep -w  'a'  /lx/3.txt
8.查询只有a的记录，不区分大小写：grep -wi 'a'  /lx/3.txt


9.查询在/lx文件夹中，文件内容包含a的信息：grep   -r   'a'  /lx


10.查询在/lx文件夹中，文件内容包含a的文件名
grep   -r   'a'  /lx  | awk -F ':' '{print $1}' | uniq


 | : 管道符，把前一个命令处理的结果传给下一个命令继续处理
uniq：去重