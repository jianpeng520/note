虽然我们的hadoop平台是搭建在linux上的，
但是hfds系统和linux是不能直接访问的，要通过命令才能交互,
hdfs的命令和linux基本相似，但是也有差别。
我们在linux上输入 `hadoop  fs`  命令   就能在linux上操作hdfs
如何查看命令是否执行成功，可以在==192.168.222.132:50070==中查看

## --1.hdfs文件保存的地方(这里看结果)
a. 192.168.222.132:50070
b. utilities - Browse the file system
## --2.查看hdfs有哪些文件(在linux中操作)
看Linux的根目录：  ls   /   
看hdfs的根目录：   hadoop fs  -ls  /
## --3.创建文件夹(在linux中操作)
hadoop fs -mkdir -p /year/month/day
hadoop fs -mkdir    /abc
## --4.创建文件(在linux中操作)
hadoop fs -touchz  /abc/1.txt

## --5.重命名(在linux中操作)
hadoop  fs  -mv   /abc/1.txt    /abc/2.txt

## --6.复制(在linux中操作)
hadoop  fs  -cp   /abc/2.txt    /year

## --7.把linux系统的文件上传到hdfs中(在linux中操作)
hadoop  fs  -put  /lx/红楼梦.txt    /abc  

## --8.查看hdfs文件的大小(在linux中操作)
hadoop fs  -du   /abc/红楼梦.txt


## --9.把linux的文件内容追加到hdfs的文件中(在linux中操作)
hadoop   fs   -appendToFile    /lx/双色球.txt    /abc/红楼梦.txt


## --10.查看hdfs文件内容(在linux中操作)
hadoop fs -cat   /abc/红楼梦.txt
hadoop fs -tail  /abc/红楼梦.txt


## --11.把hdfs的文件下载到linux中(在linux中操作)
hadoop  fs  -get   /abc/红楼梦.txt     /lx


## --12.删除hdfs的文件(在linux中操作)
hadoop fs -rm -r  /abc


## --13.给hdfs的文件授权
hadoop  fs  -chmod  -R  777  /year