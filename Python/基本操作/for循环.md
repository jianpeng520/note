## 1.语法：
for i in 范围:
    循环....
## 2.循环数字
for i in range(10):  #0<=i<10
    print(i)
## 3.循环数字
for i in range(1,11):  #1<=i<11
    print(i)
## 4.循环数字
for i in range(1,11, 2):  #1<=i<11 , 步长为2
    print(i)
## 5.循环列表
list1=[1,2,3,4,'a','b']
for i in list1 :
    print(i)
## 6.循环字典
u = {'张三':'123a' , '李四':'123b', '王五':'123c'}
for i in u :
    print(i, u[i])  # i就是key，u[i]就是value
## 7.打印下面的图案
```python
*
**
***
****
***
**
*
```
```python
for i in range(1, 8):
    if i <= 4:
        print('*' * i) # *在字符串中代表重复的次数
    else:
        print('*' * (8 - i))
```

## 8.九九乘法表
1 * 1 = 1
1 * 2 = 2    2 * 2 = 4
```python
for i in range(1,10):
    for j in range(1, i + 1):
        print(f'{j} * {i} = {j * i}', end = '\t')
    print() #print默认是 end = '\n'  , 起到换行作用
```
## 循环的两个关键字
### 1.continue:跳过本次循环
for i in range(1,11):
    if i  == 5 :
        continue
    print(i)
### 2.break：结束循环
for i in range(1,11):
    if i  == 5 :
        break
    print(i)