--------------------------------行动算子
## 1.getNumPartitions：获取分区的个数
    rdd1 = sc.parallelize([1,2,3,4,5])
    print(rdd1.getNumPartitions()) #结果为：4


## 2.collect:分布式spark转换为本地对象

## 3.countByKey：统计key的个数
    list1 = [('a', 1), ('a', 1), ('e', 3), ('b', 3), ('a', 5)]  # 定义本地列表
    rdd1 = sc.parallelize(list1)

    print(rdd1.countByKey())  # {'a': 3, 'e': 1, 'b': 1})


## 4.reduce：计算
    list1 = [1,2,3,4,5,6,7,8,9,10]  # 定义本地列表
    rdd1 = sc.parallelize(list1)

    print(rdd1.reduce(lambda x,y : x + y))  #结果为：55


## 5.first：选取第1个
    list1 = [1,2,3,4,5,6,7,8,9,10]  # 定义本地列表
    rdd1 = sc.parallelize(list1)

    print(rdd1.first())  #结果为：1

## 6.take：无序状态下，取前n个数
    list1 = [10,2,3,4,5,6,7,8,9,1]  # 定义本地列表
    rdd1 = sc.parallelize(list1)

    print(rdd1.take(3))  #结果为：[10, 2, 3]

## 7.top：降序排序后，取前n个数
    list1 = [10,2,3,4,5,6,7,8,9,1]  # 定义本地列表
    rdd1 = sc.parallelize(list1)

    print(rdd1.top(3))  #结果为：[10, 9, 8]

## 8.count：统计个数
    list1 = [10,2,3,4,5,6,7,8,9,1]  # 定义本地列表
    rdd1 = sc.parallelize(list1)

    print(rdd1.count())  #结果为：10

## 9.takeOrdered：降序或升序排序后，取前n个数
    list1 = [10,2,3,4,5,6,7,8,9,1]  # 定义本地列表
    rdd1 = sc.parallelize(list1)

    print(rdd1.takeOrdered(3, lambda x:x))   #结果为：[1, 2, 3]
    print(rdd1.takeOrdered(3, lambda x:-x))  #结果为：[10, 9, 8]


## 10.foreach：循环输出结果： 和map的功能一样
    list1 = [10,2,3,4,5,6,7,8,9,1]  # 定义本地列表
    rdd1 = sc.parallelize(list1)
    print(rdd1.glom().collect())    #查看每个分区的数据
    print(rdd1.getNumPartitions())  #查看有几个分区

    print(rdd1.foreach(lambda x: print(x * 10)))


## 11.saveAsTextFile：保存文件
    list1 = [10,2,3,4,5,6,7,8,9,1]  # 定义本地列表
    rdd1 = sc.parallelize(list1,1) #控制分区的个数为1

    #rdd1.saveAsTextFile('file:///lx/a') #a是一个文件夹, 有几个分区就会在文件夹内生成几个文件
    rdd1.saveAsTextFile('hdfs://localhost:8020/a')  #保存到hdfs