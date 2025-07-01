## 1.if语法：
if  条件表达式1:
    结果1
elif 条件表达式2:
    结果2
....
else:
    结果n
## 2.判断正负
n =  int(input("请输入数字："))  # input输入的内容为str类型
print(type(n))

if n >0 :
    print('正数')  # 缩进的距离要统一，一定要缩进
elif n < 0 :
    print('负数')
else:
    print('零')
## 3.判断一个元素在列表中是否存在(元素3在列表中是否存在)
list1 = [1,2,3,4,5,6,7]

if 3 in list1:
    print('存在')
else:
    print('不存在')
## 4.用户输入一个用户名，判断该用户是否存在，如果存在，就可以输入密码，判断密码是否正确
```python
u = {'张三':'123a' , '李四':'123b', '王五':'123c'}

name = input('请输入用户名：')

if name in u:
    print(f'{name}用户存在')
    pd=input(f'请输入{name}用户的密码:')

    if pd == u[name]:
        print('密码输入正确，登入成功')
    else:
        print('密码输入错误，登入失败')
else:
    print('该用户不存在')
```
