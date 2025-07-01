## DDL
```sql
--语法：
execute  immediate  'DDL语句' ;
```

举例：使用代码块创建一张表
```sql
begin
  --create table e1(sno  int, sname varchar2(20), score number);    --报错
  execute immediate 'create table e1(sno  int, sname varchar2(20), score number)'; 
end;
```
## DML
1.使用代码块往e1表中插入数据
```
begin
  insert into e1 values(1,  '张三',  88.7);
  commit;
end; 
```
2.使用动态sql往e1表中插入数据
```sql
begin
  execute immediate  'insert into e1 values(2,  ''李四'',  98.7)'; --这里写了2个单引号
  commit;
end;
```
	在上面代码中为什么'李四'会有两个单引号？
		最左边和最右边的单引号是字符串的语法格式
		中间的每2个单引号是一组，会转义，变成一个单引号
```
	select ''         ,   --显示为空
	       ''''       ,   --结果为'
	       ''''''         --结果为''
	  from dual;
```
<font color="#00b0f0">备注：在代码块中，首先是先编译代码，检查语法是否错误，表是否存在等问题，然后在执行代码</font>
<font color="#00b0f0">当加上execute immediate这句话后，不会先去检查表是否存在，  他会执行代码的时候编译代码并检查语法</font>
<font color="#00b0f0">如果没有给insert into语句加上execute immediate，会检查e2表是否存在，如果不存在，就报错</font>
```sql
--在一个代码块中完成建表和插入数据的工作
begin
  --a.建表
  execute immediate 'create table e2 (sno int, sname varchar2(20), score number)';
  
  --b.插入数据
  execute immediate 'insert into e2 values(1, ''张三'', 79)';
  commit;
end;
```

## DQL
### sql查询(非动态)
```sql
在代码块中， select查询出的结果一定要into给变量
常规变量中，一个变量只能存储1个值(不能存储多行多列值)
dbms_output.put_line(只能有1个参数);
```
举例：
--1.查询7788员工编号对应的姓名
```sql
declare
   v_name  varchar2(20);
begin
   select ename into v_name from emp where empno = 7788;
   dbms_output.put_line(v_name);
end;
```

--2.查询7788员工对应的姓名和工资
```sql
--要查询几个结果，就定义几个变量
--dbms_output.put_line只有1个参数
declare
   v_name  varchar2(20);
   v_sal   number;
begin
   --把ename赋值给v_name, 把sal赋值给v_sal
   select ename ,sal into v_name, v_sal from emp where empno = 7788;
   dbms_output.put_line(v_name || ',' || v_sal);
end;
```
--3.变量的变种定义法1：引用表格中某个字段的数据类型(查询7788员工对应的姓名和工资)
变量名  表名.字段名%type
```sql
declare
   v_name  emp.ename%type; --变量v_name的数据类型和emp表中ename的数据类型是一样的
   v_sal   emp.sal%type;
begin
   --把ename赋值给v_name, 把sal赋值给v_sal
   select ename ,sal into v_name, v_sal from emp where empno = 7788;
   dbms_output.put_line(v_name || ',' || v_sal);
end;
```
--4.变量的变种定义法2(复合数据类型)：引用表格中所有字段的数据类型
变量名   表名%rowtype    
v_emp    emp%rowtype 这句话的意思：
emp表有8个字段，每个字段都有数据类型，我们定义的变量v_emp引用了emp表的数据类型，相当于变量v_emp也能存储8个值

```sql
declare
  v_emp   emp%rowtype; --定义变量v_emp,他和emp表的数据类型一致，能存储1行8列值
begin
  select * into v_emp from emp where empno = 7788;--把7788查询出来的8个值赋值给变量v_emp
  dbms_output.put_line(v_emp.ename || ',' || v_emp.sal);
end;
```
--5.自定义复合数据类型
declare
   type t_emp is record(     
      name  varchar2(20),
      sal   number,
      dt    date
   ); --声明复合数据类型t_emp,里面包含三种数据类型;  类型不能直接拿来用，要通过变量来使用
   v_emp  t_emp;  --定义变量v_emp, 它是复合数据类型t_emp, 有三种类型，能存储3个值
```sql
begin
  --select ename, sal, hiredate into v_emp from emp where empno = 7788;
  --select ename, sal into v_emp from emp where empno = 7788;   --错误：值过多
  select ename, sal into v_emp.name, v_emp.sal from emp where empno = 7788;
  dbms_output.put_line(v_emp.name  || ',' || v_emp.sal);
end;
```
### sql(动态查询)
```sql
--select中动态sql的完整语法
execute immediate 'select 结果1, 结果2 from emp where 字段1 = :1 and 字段2 =  :2 .....'
into  变量3, 变量4...
using 变量1, 变量2...

:1他是占位符，可以用:a代替或者:其他代替

①：使用using把变量1的值传递给:1 ， 把变量2的值传递给:2
②：select查询出结果1，结果2
③：结果1的值into给变量3， 结果2的值into给变量4
```

```
--输入员工编号，查询它的工资
declare
  v_empno  int  := &请输入员工编号;
  v_sal    number;
begin
  execute immediate 'select sal from emp where empno = :1'
  into  v_sal
  using v_empno;
  dbms_output.put_line(v_sal);
end;
```

```
--输入员工的部门和入职日期，查询它的姓名和工资
declare
  v_dno    int  := &请输入部门编号;
  v_dt     date := date'&请输入入职日期';
  v_name   varchar2(20);
  v_sal    number;
begin
  execute immediate 'select ename, sal from emp where deptno = :1 and hiredate = :2'
  into  v_name, v_sal
  using v_dno, v_dt;
  dbms_output.put_line(v_name || ',' || v_sal);
end;
```