## 1.序列
`seq 1 10       #输出1到10`

`seq 1 2 10     #步长为2`

## 2.循环数字  vim  f1.sh
```
#!/bin/bash
for i in `seq 1 10`
do
  echo $i
done
```


## 3.循环文件夹  vim f2.sh
```
#!/bin/bash
for i in `ls /lx`
do
   echo $i
done
```


## 4.continue：跳过本次循环   vim f4.sh
```
#!/bin/bash
for i in `seq 1 10`
do
  if [ $i -eq 5 ]; then
     continue
  fi
  echo $i
done
echo 哈哈
```
## 5.break：结束循环   vim f4.sh
```
#!/bin/bash
for i in `seq 1 10`
do
  if [ $i -eq 5 ]; then
     break
  fi
  echo $i
done
echo  哈哈
```
## 6.exit：退出shell脚本   vim f4.sh
```
#!/bin/bash
for i in `seq 1 10`
do
  if [ $i -eq 5 ]; then
     exit
  fi
  echo $i
done
echo  哈哈
```