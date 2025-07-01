语法：select * from 表名

注意：sql的执行顺序
```sql
select *                      6
	from 表名           1
join                             2
where                         3  
group by                    4
having                        5
order by                     8
pivot                           7       --在selcet 后执行行转列

```

## 子查询
当写一个select不能完成需求的时候，需要select里面套select，这个就是子查询，俗称套娃

```sql
--【例1】在emp表中，查询比SMITH工资要高的其他员工的信息
--a.先知道smith的工资是多少
select sal from emp where ename = 'SMITH';  --工资是800

--b.找emp表中工资大于800的信息
select * from emp where sal > 800;

--c.把800换成smith的信息
select * from emp where sal > (select sal from emp where ename = 'SMITH');
```


```sql
--【例6】查询和10号部门中，岗位相同的其他员工的信息
--a.查询10好部门的岗位
select job from emp where deptno = 10

--b.查询和10号部门中，岗位相同的其他员工的信息
select * 
  from emp
 where job in (select job from emp where deptno = 10)
   and deptno != 10;
```