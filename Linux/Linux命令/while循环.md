## 1.循环数字   vim  w1.sh
```
#!/bin/bash
n=1
while [ $n -le 10 ]
do
   echo $n
   let n++   #相当于n=n+1
done
```
## 2.循环某个文件的内容    vim  w2.sh
```
#!/bin/bash
a=/lx/3.txt
while read  p
do
   echo $p
done < $a
```
原理：
p是变量，read逐行读取/lx/3.txt这里面的数据，把数据存到变量p中，然后输出p
## 3.有一个文件1.txt，有如下内容， 找出里面长度超过20的数据     
### vim   /lx/f6.sh
```
sdfdsafsdafdsfsd
wjfsjfsdjflsdajfsdlf
dsfd
e87932sdlkfjdsfifksdkfipiosdf233ff
ss
```
sed   -n '4p'  /lx/1.txt            ##打印第4行
wc -l /lx/1.txt | awk '{print $1}'  ##统计文件的行数
### ---方法1：
```
#!/bin/bash

line=`wc -l /lx/1.txt | awk '{print $1}'`  #统计文件的行数

for i in `seq 1  $line`
do
    data=`sed   -n  "${i}p"  /lx/1.txt`       #提取第i行数据, data就是每行数据
	
	if [ ${#data} -gt 20 ];then                  #${#data} 变量的长度
	   echo $data
	fi
done
```
### ---方法2：
```
#!/bin/bash

a=/lx/1.txt    #指定要读取的文件的路径

while  read   p    #p就是每行数据
do
   if [ ${#p} -gt 20 ]; then
      echo $p
   fi
done < $a
```