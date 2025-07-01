转换算子相当于构建执行过程，他并不会执行。
碰到行动算子后才会触发执行， 行动算子用来触发转换算子

------------------------转换算子
## 1.parallelize：把python的本地对象转换为spark的分布式对象
	#1.创建一个本地列表，这是python对象
    list1=[1,2,3,4,5]
    #2.把本地列表转换为spark的分布式对象
    rdd1 = sc.parallelize(list1)
    #3.collect是行动算子，用于执行转换算子，并且把spark的分布式对象转换为本地对象
    print(rdd1.collect())
## 2.glom：查看每个分区的数据
#1.创建一个本地列表，这是python对象
    list1=[1,2,3,4,5]
    #2.把本地列表转换为spark的分布式对象
    rdd1 = sc.parallelize(list1)
    #3.collect是行动算子，用于执行转换算子，并且把spark的分布式对象转换为本地对象
    print(rdd1.glom().collect())   #显示每个分区的数据


## 3.textFile：读取文件，生成rdd
### 1.textFile读取文件，形成rdd
```python
#rdd1  = sc.textFile('/lx/1.txt')       #读取本地linux文件
    #rdd1 = sc.textFile('file:///lx/1.txt') #读取本地linux文件
    rdd1=sc.textFile('hdfs://localhost:8020/1.txt')  #读取hdfs上的文件(启动hadoop服务和上传文件到hdfs)
```
### 2.查看
`print(rdd1.collect()) #结果为： ['a b c', 'a c d', 'a a a', 'a b c', 'e f g']`
## 4.wholeTextFile：读取文件夹里面所有的文件数据
```python
rdd1 = sc.wholeTextFiles('/lx/abc')
rdd1 = sc.wholeTextFiles('file:///lx/abc')   #abc是文件夹
print(rdd1.collect())
```
## 5.map算子：映射算子，对数据进行转换映射
```python
	rdd1=sc.parallelize([1,2,3,4,5])   #本地列表变成spark分布式列表
    #lambda是匿名函数，没有函数名字
    #x是变量，逐个接收rdd1传过来的值， 每个值 * 10
    rdd2=rdd1.map(lambda x : x * 10)   #把每个数*10
    print(rdd2.collect())
    rdd1=sc.parallelize(['1 2 3', '4 5 6'])   #本地列表变成spark分布式列表
    #lambda是匿名函数，没有函数名字
    #x是变量，逐个接收rdd1传过来的值  x = '1 2 3', 使用split按空格把'1 2 3'切割成列表，变成了['1','2','3']
    rdd2=rdd1.map(lambda x : x.split(' '))
    print(rdd2.collect())  # 结果为：[['1', '2', '3'], ['4', '5', '6']]
```
## 6.flatMap：映射算子，对数据进行转换映射  。 但是它比map多个解嵌套功能
```python
rdd1=sc.parallelize(['1 2 3', '4 5 6'])   #本地列表变成spark分布式列表
    #lambda是匿名函数，没有函数名字
    #x是变量，逐个接收rdd1传过来的值  x = '1 2 3', 使用split按空格把'1 2 3'切割成列表，flatmap解嵌套，变成了 '1','2','3'
    rdd2=rdd1.flatMap(lambda x : x.split(' '))
    print(rdd2.collect())  # 结果为：['1', '2', '3', '4', '5', '6']
```
## 7.reduceByKey:针对kv类型的数据，按key分组，对v计算
```python
list1=[('a',1), ('b',2), ('a',3), ('c',4), ('a',5)] #定义本地列表
    rdd1=sc.parallelize(list1) #本地列表转spark数据集
    #会把key相同的划为一组，('a',1)('a',3)('a',5)
    #x和y都是变量 x=1,y=3, x+y=4 , x=4
    #y=5, x = 4, x+y=9
    rdd2=rdd1.reduceByKey(lambda x,y : x + y)
    print(rdd2.collect()) #结果为：[('b', 2), ('c', 4), ('a', 9)]
```
## 8.groupBy：只分组不计算
    
```python
##①：把key相同的数据划入同一组
    list1=[('a',1), ('b',2), ('a',3), ('c',4), ('a',5)] #定义本地列表
    rdd1 = sc.parallelize(list1)
    #x逐个接收rdd1传过来的值， 先接收 x = ('a',1)
    #x[0] 结果为 'a'  , groupBy按元组的0号元素分组
    rdd2 = rdd1.groupBy(lambda x:x[0])
    #下面的结果中，元组的0号元素是分组key，1号元素是数据，这个数据是一个对象，需要进行转换
    print(rdd2.collect()) #结果为[('b', <pyspark.rble object at 0x7f4c8817f8b0>), ('a', ),('c', )]
    #把元组中的对象1号元素转换为列表
    #x开始接收值。 x = ('b', <pyspark.rble object at 0x7f4c8817f8b0>)
    rdd3=rdd2.map(lambda x:(x[0], list(x[1])))
    print(rdd3.collect()) #结果为：[('b', [('b', 2)]), ('c', [('c', 4)]), ('a', [('a', 1), ('a', 3), ('a', 5)])]
```

## 9.mapValues：针对kv类型中的v，对v执行map操作
```python
##①：把key相同的数据划入同一组
    list1=[('a',1), ('b',2), ('a',3), ('c',4), ('a',5)] #定义本地列表
    rdd1 = sc.parallelize(list1)
    #x是('a',1)元组中的x[1],也就是1号元素， 把元组的1号元素都 * 10
    rdd2=rdd1.mapValues(lambda x : x * 10)
    print(rdd2.collect())  #结果为：[('a', 10), ('b', 20), ('a', 30), ('c', 40), ('a', 50)]
```
## 10.filter：筛选过滤
  
```python
  list1=[('a',1), ('b',2), ('a',3), ('c',4), ('a',5)] #定义本地列表
    rdd1 = sc.parallelize(list1)
    #x=('a',1) , 所以 x[1]=1, x[0]='a'
    #rdd2=rdd1.filter(lambda x:x[1]>=3)  ##①：筛选出v大于等于3的数据
    #print(rdd2.collect())  # 结果为：[('a', 3), ('c', 4), ('a', 5)]
    
    rdd2 = rdd1.filter(lambda x: x[0] >= 'b')  ##①：筛选出k大于等于b的数据
    print(rdd2.collect())  # 结果为：[('b', 2), ('c', 4)]
```
## 11.distinct：去重
```python
list1=[('a',1), ('a',1), ('b',3), ('b',3), ('a',5)] #定义本地列表
    rdd1 = sc.parallelize(list1)
    rdd2 = rdd1.distinct()
    print(rdd2.collect()) #结果为：[('a', 1), ('a', 5), ('b', 3)]
```
## 12.union:拼接
```python
rdd1= sc.parallelize([1,2,3,4])
    rdd2= sc.parallelize([4,5,6,7])
    rdd3= rdd1.union(rdd2)

    print(rdd3.collect())  #结果为：[1, 2, 3, 4, 4, 5, 6, 7]
```
## 13.join：针对kv类型，相当于表连接功能
```python
list1 = [('张三', '数学'), ('李四', '数学'), ('王五', '数学')]  # 定义本地列表
    list2 = [('张三', 99.9),  ('李四', 89),    ('小芳', 90)]  # 定义本地列表

    rdd1 = sc.parallelize(list1)
    rdd2 = sc.parallelize(list2)

    print(rdd1.join(rdd2).collect())           # 内连接[('李四', ('数学', 89)), ('张三', ('数学', 99.9))]
    print(rdd1.leftOuterJoin(rdd2).collect())  # 左连接[('李四', ('数学', 89)), ('王五', ('数学', None)), ('张三', ('数学', 99.9))]
    print(rdd1.rightOuterJoin(rdd2).collect()) # 右连接[('李四', ('数学', 89)), ('小芳', (None, 90)), ('张三', ('数学', 99.9))]
    print(rdd1.fullOuterJoin(rdd2).collect())  # 全连接[('李四', ('数学', 89)), ('小芳', (None, 90)), ('张三', ('数学', 99.9))]
```
## 14.intersection：交集
```python
list1 = [('张三', '数学'), ('李四', '数学'), ('王五', '数学')]  # 定义本地列表
    list2 = [('张三', '数学'),  ('李四', 89),    ('小芳', 90)]  # 定义本地列表

    rdd1 = sc.parallelize(list1)
    rdd2 = sc.parallelize(list2)

    print(rdd1.intersection(rdd2).collect()) #结果为：[('张三', '数学')]
```
## 15.groupByKey：针对kv类型， 自动按key进行分组
```python
#自动对元组中的元素按key进行分组
    rdd2 = rdd1.groupByKey()
    print(rdd2.collect()) #[('b', <pyspark.resultiterable.Resu1612b0>), ('a', <pyspar)..]

    #把v中的对象转成列表(元组的1号元素转成列表)
    rdd3 = rdd2.map(lambda x:(x[0], list(x[1])))
    print(rdd3.collect()) #[('b', [3, 3]), ('a', [1, 1, 5])]
```
## 16.sortBy：排序
```python
list1=[('a',1), ('d',1), ('e',3), ('b',3), ('c',5)] #定义本地列表
    rdd1 = sc.parallelize(list1)

    #x=('a',1)   x[0]='a',  x[1]=1
    print(rdd1.sortBy(lambda x: x[0]).collect()) #按key进行升序排序
    print(rdd1.sortBy(lambda x: x[0],ascending=False).collect())  # 按key进行降序排序
    
    print(rdd1.sortBy(lambda x: x[1]).collect()) #按v进行升序排序
    print(rdd1.sortBy(lambda x: x[1],ascending=False).collect())  # 按v进行降序排序
```
## 17.sortByKey ：按key进行排序
```python
list1=[('a',1), ('d',1), ('e',3), ('b',3), ('c',5)] #定义本地列表
    rdd1 = sc.parallelize(list1)


    print(rdd1.sortByKey().collect()) #按key进行升序排序
    print(rdd1.sortByKey(ascending=False).collect())  # 按key进行降序排序
```
## 18.在linux中有一个文件夹/lx/abc, 里面有很多文件，统计所有字母的个数
```python
[root@master abc]# pwd
/lx/abc
[root@master abc]# ls
1.txt  2.txt  3.txt
[root@master abc]# cat 1.txt
a b c
a c d
a a a
a b c
e f g

from pyspark import SparkConf, SparkContext
import re

# 1.快捷键：main 回车：程序入口类
if __name__ == '__main__':
    conf = SparkConf().setAppName("miniProject").setMaster("local[*]")
    sc = SparkContext(conf=conf)

    #①：读取文件夹,生成列表，有几个文件就有几个元组
    rdd1 = sc.wholeTextFiles('file:///lx/abc')
    print(rdd1.collect()) #[('file:/lx/abc/1.txt', 'a b c\na c d\na a a\na b c\ne f g\n'), ()...]

    #②：在元组中，0号元素是路径，1号元素是数据，我们只要1号元素的数据
    rdd2=rdd1.map(lambda x:x[1])
    print(rdd2.collect()) #['a b c\na c d\na a a\na b c\ne f g\n', 'a b c\ne f g\n'....]

    #③：按空格和\n把列表中的每个元素切割成列表 x= 'a b c\na c d\na a a\na b c\ne f g\n'
    #   \W:非字母非数字(空格和换行符)，  r是防止转义
    rdd3 = rdd2.flatMap(lambda x:re.split(r'\W+', x))
    print(rdd3.collect()) #['a', 'b', 'c', 'a', 'b', 'c', 'e', 'f', 'g', '',..]

    #④：筛选长度大于0的字母，去掉rdd3中的空''     x等于每个字母
    rdd4 = rdd3.filter(lambda x:len(x) >0)
    print(rdd4.collect()) #['a', 'b', 'c', 'a', 'b', 'c', 'e', 'f', 'g'...]

    #⑤：把上面的单个元素都变成kv类型，例如 a 变成 ('a', 1) ,假设每个字母个数为1
    rdd5 = rdd4.map(lambda x:(x,1))
    print(rdd5.collect()) #[('a', 1), ('b', 1), ('c', 1), ('a', 1), ('c', 1)...]

    #⑥：按key分组，对v执行reduce计算
    rdd6 = rdd5.reduceByKey(lambda x,y : x + y)
    print(rdd6.collect()) #[('b', 4), ('c', 6), ('d', 2), ('g', 2), ('a', 17), ('e', 3), ('f', 2)]


    sc.stop()
```

