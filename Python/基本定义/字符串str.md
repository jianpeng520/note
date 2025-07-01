## 字符串切片查询
s='0123456789abcdefg'
print(s)        # 输出所有内容
print(s[0:5])   # 下标是从0开始的。  0 <= s < 5    是一个左闭右开区间
print(s[0:8:2]) #0 <= s < 8  ,步长为2   s[起始下标:结束下标:步长]
print(s[5:])    # 从下标为5开始，全部截取
print(s[:])     # 打印全部
print(s[::2])   # 打印全部,步长为2
print(s[::-1])  # 倒序输出
print(s[:5])    # 0 <= s < 5
## 字符串函数
s=' abcd '
print(s.strip()) # 去除左右两边的空格
print(s.lstrip()) # 去除左边的空格
print(s.rstrip()) # 去除右边的空格

---

print(s.upper()) # 转大写
print(s.lower()) # 转小写
print(s.title()) # 首字母大写

---

print(s.replace('a','1'))  # 把所有的a替换为1

---

print(s.find('e'))  # 查找字符a在字符串中的位置, 找不到结果会返回-1

print(s.index('a')) # 查找字符a在字符串中的位置,找不到会报错

---

print(len(s)) # 统计字符串的长度