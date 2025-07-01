字典是是一组用{}括起来的数据集合，字典由key和value构成，
key和value之间是通过：号隔开，    每组之间是用逗号隔开

d={'张三':63, '李四':90,'王五':80}

print(d.keys()) # 打印字典所有的key
print(d.values())  # 打印字典所有的value

print(d['张三']) # 打印字典d中，key为张三所对应的value

d['张三']=100 # 把张三的value改为100
print(d)

d['老六']=99  # 字典增加元素
print(d)

##d.clear()  # 清除所有的元素
##print(d)

del(d['张三']) # 去除key为张三的
print(d)