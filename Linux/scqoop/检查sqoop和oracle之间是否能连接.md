```shell
#强行换行：一个空格+\
#\符号后面不要打空格
#检查windows ip地址
#在linux中执行
#如果sqoop连不上oracle，需要检查windows防火墙，梯子不要打开， 修改oracle的两个配置文件。改成host=计算机名。 可以看csdn博客  https://blog.csdn.net/qq_39462284/article/details/146841293#/

sqoop list-tables \
--connect jdbc:oracle:thin:@192.168.1.76:1521/ORCL \
--username scott \
--password 123456


#如何sqoop连不上oracle，需要检查windows防火墙，梯子不要打开， 修改oracle的两个配置文件。改成host=计算机名。 可以看csdn博客
```
