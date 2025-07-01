## 1.随便输入一个数字，判断正负  
### vim   f1.sh
```
#!/bin/bash
read  -p  "请输入心目中的数字：" n   #用n存储输入的值

if [ $n -gt 0 ];then    #then写在同一行，需要打分号
   echo 正数          
elif [ $n -lt 0 ]       
  then     
    echo 负数
else
  echo 零
fi
```
## 2.随便输入一个数字，判断正负   
### vim   f2.sh
```
#!/bin/bash
read  -p  "请输入心目中的数字：" n   #用n存储输入的值

if test $n -gt 0 ; then    #then写在同一行，需要打分号
   echo 正数          
elif test $n -lt 0       
  then     
    echo 负数
else
  echo 零
fi
```
## 3.随便输入一个文件和他的路径地址，如果不存在，就创建一个，如果存在就删除   
### vim f3.sh
```
#!/bin/bash

read -p "请输入文件名和他的路径地址:"  name

if [ -f $name ];then
  echo 该文件存在，请马上删除
  rm -rf $name
  echo 文件已删除
else
  echo 该文件不存在，请马上创建
  touch $name
  echo 文件已创建
fi
```
## 4.linux中小数计算
yum -y install bc              ##下载bc计算器

echo  "scale=2;5/3"  | bc      ##保留2位小数 scale=2

## 5、查询某一个文件的大小
如果文件的大小是小于1000的，那么就直接显示为 xxxB，如果是大于1000的，就显示为 xxxK  。保留2位小数   。 vim  /lx/s4.sh

```
#!/bin/bash
read -p "请输入文件的路径地址和文件名："  name

size=`wc -c  $name  | awk '{print $1}'` #获取文件大小，单位是B

if [ $size -gt 1000 ]; then
   size2=`echo  "scale=2;$size/1024"  | bc`  #B转为KB，但是没有单位
   echo  ${size2}kb
else
   echo  ${size}b
fi
```
## 6.如果内存硬或盘使用率超过90%，则发出提醒    
### 补充知识点：
`free | awk '$1=="Mem:"{printf "%d" , $3/$2*100}'  ` <span style="background:rgba(173, 239, 239, 0.55)">##提取内存的使用率</span>`
`printf`  <span style="background:rgba(173, 239, 239, 0.55)">##格式化输出</span>
`%d` <span style="background:rgba(173, 239, 239, 0.55)">##整数占位符</span>

`$3/$2*100`   <span style="background:rgba(173, 239, 239, 0.55)">把这个结果以整数的方式放入%d</span>

`df -h | awk '$NF=="/"{print $5}' | sed 's/%//'`    <span style="background:rgba(173, 239, 239, 0.55)">##提取硬盘的使用率</span>
### 代码：
`vim s5.sh`

`free | awk '$1=="Mem:"{printf "%d" , $3/$2*100}'  #提取内存的使用率`
`printf ：格式化输出`
`%d：整数占位符`
`$3/$2*100：把这个结果以整数的方式放入%d`
`df -h | awk '$NF=="/"{print $5}' | sed 's/%//'    #提取硬盘的使用率`
```
#!/bin/bash

f=`free | awk '$1=="Mem:"{printf "%d" , $3/$2*100}'`  #提取内存的使用率
d=`df -h | awk '$NF=="/"{print $5}' | sed 's/%//'`    #提取硬盘的使用率

if [ $f -ge 90 ];then
  echo 内存快爆了
fi

if [ $d -ge 90 ];then
  echo 硬盘快爆了
fi
```