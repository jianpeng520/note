crond是定时调度服务， 它有一个干活的牛马， 叫crontab



--1.理解时间关系，这个不能执行
*     *      *      *    *
```
分   小时   天     月   星期

*     *      *      *    *   echo  haha >>  /lx/1.txt    #每分钟执行一次echo
*/10  *      *      *    *   echo  haha >>  /lx/1.txt    #每10分钟执行一次echo
30    *      *      *    *   echo  haha >>  /lx/1.txt    #每小时30分执行一次echo
30    9      *      *    *   echo  haha >>  /lx/1.txt    #每天的9:30执行一次echo
30    9      10     *    *   echo  haha >>  /lx/1.txt    #每个月10号的9:30执行一次echo
30    9      10     2    *   echo  haha >>  /lx/1.txt    #2月10号的9:30执行一次echo
```


--2.编辑定时调度任务：crontab -e
* 14 * * * echo  haha >>  /lx/1.txt

--3.删除定时任务
crontab -r

--4.查看定时任务
crontab -l

--5.启动定时任务
service crond start

--6.查看定时服务状态
service crond status

--7.停止定时服务
service crond stop

--8.重启定时服务
service crond restart