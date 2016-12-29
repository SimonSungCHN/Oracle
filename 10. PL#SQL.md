### PL/SQL（Procedure Language/SQL）
#### 打印 Hello World
如果要在屏幕上输出信息，需要将 serveroutput 开关打开：`set serveroutput on`

```
declare
    --说明部分（变量，光标或者例外）
begin
	--程序体
dbms_output.putline('Hello World');
end;
/
```

#### PL/SQL 程序
* PL/SQL 是 Oracle 对 sql 语言的过程化扩展，是面向过程的语言，指在 SQL 命令语言中增加了过程处理语句（如分支、循环等），是 SQL 语言具有过程处理能力
* 简单、高效、灵活、实用

#### PL/SQL 的程序结构
```
declare
    --说明部分（变量说明，光标申明或者例外申明）
begin
	--语言序列（DML 语句）
exception
	--例外处理语句
end;
/
```

#### 说明部分（变量的定义：变量在前，类型在后）
1. 基本数据类型变量
	* 类型：char, varchar2, date, number, boolean, long
	* 举例：newchar char(15); flag boolean:=true; newnum number(7,2);
2. 引用型变量
	* 举例：`pename emp.ename%type;`
```
declare
	--定义引用型变量：查询并打印7839的姓名和薪水
	--pename varchar2(20);
	--psal    number;
  pename emp.ename%type;
  psal   emp.sal%type;
begin
	--得到7839的姓名和薪水
  select ename, sal into pename, psal from emp where empno = 7839;
	--打印姓名和薪水
  dbms_output.put_line(pename||'的薪水是'||psal);
end;
/
```
3. 记录型变量
	* 举例：`emp_rec emp%rowtype;`
	* 记录型变量分量的引用：`emp_res.ename:='ADAMS';`
```
declare
	--定义记录型变量（注意代表一行）：查询并打印7839的姓名和薪水
  emp_rec emp%rowtype;
begin
	--得到7839一行的信息
  select * into emp_rec from emp where empno = 7839;
	--打印姓名和薪水
  dbms_output.put_line(emp_rec.ename||'的薪水是'||emp_rec.sal);
end;
/
```

#### IF 语句
1. 
```
IF 条件 THEN 语句1;
语句2;
END IF;
``` 
2. 
```
IF 条件 THEN 语句序列1;
ELSE 语句序列2;
END IF;
```
3. 
```
IF 条件 THEN 语句;
ELSIF THEN 语句;
ELSE 语句;
END IF;
```
4. 判断用户从键盘输入的数字
```
	--接收一个键盘输入（字符串）,（num是一个地址值，含义是在该地址上保存了输入的值）
  accept num prompt'请输入一个数字';
declare
	--定义变量保存用户从键盘输入的数字
  pnum number:=&num;
begin
	--执行 if 语句进行条件判断
  if pnum = 0 then dbms_output.put_line('您输入数字是0');
  elsif pnum = 1 then dbms_output.put_line('您输入数字是1');
  elsif pnum = 2 then dbms_output.put_line('您输入数字是2');
  else dbms_output.put_line('其他数字');
  end if;
end;
/
```

#### 循环语句
1. 
```
WHILE total <=25000 LOOP
...
total:=total + salary;
END LOOP;
```
2. 
```
Loop
EXIT [when 条件];
...
End loop;
```
3. 
```
FOR I IN 1..3 LOOP
语句序列;
END LOOP;
```
4. 使用循环打印数字的1~10
```
	--使用 while 循环
declare
	--定义循环变量
  pnum number:=1;
begin
  while pnum <=10 
   loop
	--循环体
    dbms_output.put_line(pnum);
	--变量+1
   pnum := pnum + 1;
  end loop;
end;
/
```
```
	--使用 loop 循环（推荐）
declare
	--定义循环变量
  pnum number:=1;
begin
  loop
	--退出条件：循环变量大于10
   exit when pnum > 10;
   dbms_output.put_line(pnum);
	--变量+1
   pnum := pnum + 1;
  end loop;
end;
/
```
```
	--使用 for 循环
declare
	--定义循环变量
  pnum number:=1;
begin
  for pnum in 1..10
   loop
    dbms_output.put_line(pnum);
   end loop;
end;
/
```

#### 光标（游标）
1. 定义：就是一个结果集（Result Set）
2. 光标的语法
```
CURSOR 光标名 [(参数名 数据类型[,参数名 数据类型]...)]
IS SELECT 语句;
```
3. 光标的属性
	* %found：如果 fetch 取到值返回 true，否则返回 false
	* %notfound：与 %found 相反
	* %isopen：如果光标打开返回 true，否则返回false
	* %rowcount：影响的行数（如果 fetch 取走10行，那么 %rowcount就是10）
4. 光标的限制
	* 默认情况下，oracle数据库只允许在同一个会话中，打开300个光标
	* 修改光标数的限制：`alter system set open_cursors = 400 scope = both;`（scope的取值：both（两个同时更改），memory（只更改当前实例不更改参数文件），spfile（只更改参数文件不更改当前实例，数据库需要重启））
5. fetch 的作用
	1. 把当前指针指向的记录返回
	2. 将指针指向吓一跳记录
6. 使用光标查询员工姓名和工资，并打印
```
declare
	--定义一个具体的光标
  cursor cemp is select ename, sal from emp;
	--为光标定义对应的变量
  pename emp.ename%type;
  psal   emp.sal%type;
begin
	--打开光标
  open cemp;
	--循环
  loop
	--取一条记录
   fetch cemp into pename, psal;
   exit when cemp%notfound;
	--打印
   dbms_output.put_line(pename||'的薪水是'||psal);
  end loop;
	--关闭光标
  close cemp;
end;
/
```
7. **注：对于 oracle，默认的事务隔离级别是 read committed，当 PL/SQL 语句对数据库进行了查询之外的操作，那么要在`end;`前 加`commit;`。事务的ACID：原子性、一致性、隔离性、持久性**
8. 带参数的光标
```
	--查询某个部门中员工的姓名
declare
	--定义带参数的光标
  cursor cemp(dno number) is select ename from emp where deptnp = dno;
	--为光标定义对应的变量
  pename emp.ename%type;
begin
	--打开光标
  open cemp(10);
	--循环
  loop
	--取一条记录
   fetch cemp into pename;
   exit when cemp%notfound;
	--打印
   dbms_output.put_line(pename);
  end loop;
	--关闭光标
  close cemp;
end;
/
```
#### 例外
1. 例外是程序设计语言提供的一种功能，用来增强程序的健壮性和容错性
2. 系统例外
	* no_data_found（没有找到数据）
```
declare
  pename emp.ename%type;
begin
  select ename into pename from emp where empno = 1234;
exception
  when no_data_found then dbms_output.put_line('没有找到该员工');
  when others then dbms_output.put_line('其他例外');
end;
/
```
	* Too_many_rows（select...into 语句匹配多个行）
```
declare
  pename emp.ename%type;
begin
  select ename into pename from emp where deptno = 10;
exception
  when too_many_rows then dbms_output.put_line('select into 匹配了多行');
  when others then dbms_output.put_line('其他例外');
end;
/
```
	* Zero_Divide（被零除）
```
declare
  penum number;
begin
  pnum:= 1/0;
exception
  when zero_divide then dbms_output.put_line('0不能做除数');dbms_output.put_line('again：0不能做除数');
  when others then dbms_output.put_line('其他例外');
end;
/
```
	* Value_error（算术或转换错误）
```
declare
  penum number;
begin
  pnum:= 'abc';
exception
  when value_error then dbms_output.put_line('算术或者转换错误');
  when others then dbms_output.put_line('其他例外');
end;
/
```
	* Timeout_on_resource（在等待资源时发生超时）
3. 自定义例外
	1. 定义变量，类型是 exception
	2. 使用 raise 抛出自定义例外
```
declare
	--定义光标
  cursor cemp is select ename from emp where deptno = 50;
	--为光标定义对应的变量
  pename emp.ename%type;
	--自定义例外
  no_emp_found exception;
begin
	--打开光标
  open cemp;
	--直接取一条记录
  fetch cemp into pename;
  if cemp%notfound then
	--抛出例外
   raise no_emp_found;
  end if;
	--关闭光标
	--oracle 自动启动pmon(process monitor), 自动关闭光标
  close cemp;
exception
  when no_emp_found then dbms_output.put_line('没有找到员工');
  when others then dbms_output.put_line('其他例外');
end;
/
```

#### 程序设计方法
1. 瀑布模型：需求分析 --> 设计（1.概要设计；2.详细设计） --> 编码（coding） --> 测试（Testing）--> 上线
