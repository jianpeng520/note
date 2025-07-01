对文件中的数据进行增删改查

--------文件准备   vi  /lx/1.txt
www.baidu.com
WwW.souhu.cn
WWW.tengxun.com

--------增  
1.在第2行的前面l临时增加一个哈哈 :   sed  '2i 哈哈'  /lx/1.txt
2.在第2行的后面l临时增加一个哈哈 :   sed  '2a 哈哈'  /lx/1.txt

3.在第2行的前面l真的增加一个哈哈 :   sed -i   '2i 哈哈'  /lx/1.txt
4.在第2行的后面l真的增加一个哈哈 :   sed -i   '2a 呵呵'  /lx/1.txt

--------删
1.临时删除第2行：    sed   '2d'     /lx/1.txt
2.临时删除第2-3行：  sed   '2,3d'   /lx/1.txt

3.真的删除第2行：    sed -i  '2d'     /lx/1.txt
4.真的删除第2-3行：  sed -i  '2,3d'   /lx/1.txt

--------改
1.把第1行和第2行的w临时替换为1：        sed  '1,2s/w/1/'   /lx/1.txt
2.把第1行和第2行所有的小w临时替换为1：  sed  '1,2s/w/1/g'  /lx/1.txt
3.把所有的w都替换为1：                  sed  's/w/1/gi'    /lx/1.txt


4.把第1行和第2行的w真的替换为1：        sed  -i  '1,2s/w/1/'   /lx/1.txt
5.把第1行和第2行所有的小w真的替换为1：  sed  -i  '1,2s/w/1/g'  /lx/1.txt
6.把所有的w都替换为1：                  sed  -i  's/w/1/gi'    /lx/1.txt


--------查
1.查询第2行数据：    sed  -n '2p'    /lx/1.txt
2.查询第2-3行数据：  sed  -n '2,3p'  /lx/1.txt


