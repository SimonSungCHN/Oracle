### 存储过程、存储函数
#### 存储过程和存储函数
1. 指存储在数据库中供所有用户程序调用的子程序叫存储过程、存储函数
2. 存储过程和存储函数的**相同点**：完成特定功能的程序
3. 存储过程和存储函数的**区别**：存储函数能用 return 语句返回值，存储过程不能

#### 创建和使用存储过程
1. 用CREATE PROCEDURE 命令建立存储过程和存储函数
2. 语法：`CTEATE [OR REPLACE] PROCEDURE 过程名（参数列表 AS PLSQL子程序体;`
```
	--创建不带参数存储过程：打印 Hello  World
create or replace procedure sayhelloworld
as
	--说明部分
begin
  dbms_output.put_line('Hello world');
end;
/
```
3. 调用存储过程
	1. 在sql plus中 exec sayhelloworld();
	2. 在另外的 PL/SQL 程序调用；（`begin sayhelloworld();sayhelloworld(); end; /`）
4. 带参数的存储过程（注意：**一般**不在存储过程或者存储函数中 commit 和 rollback）
```
	--创建带参数的存储过程(in 代表输入参数)
create or replace procedure raisesalary(eno in number)
as
  psal emp.sal%type;
begin
  select sal into psal from emp where empno = eno;
  update emp set sal = sal + 100 where  empno = eno;
  dbms_output.put_line('涨前'||psal||'涨后'||psal + 100);
end;
/
```
	* 调用（`begin raisesalary(7839);raisesalary(7566);commit; end; /`）

#### 存储函数
1. 函数（Function）为一命名的存储程序，可带参数，并返回一计算值
2. 函数和过程的结构类似，但必须有一个 RETURN 子句，用于返回函数值
3. 创建存储函数的语法：`CTEATE [OR REPLACE] FUNCTION 函数名（参数列表）RETURN 函数值类型 AS PLSQL子程序体;`
```
	--创建存储函数：查询某个员工的年收入
create or replace function queryempincome(eno in number)
as
  psal emp.sal%type;
  pcomm emp.comm%type;
begin
  select sal, comm into psal, pcomm from emp where empno = eno;
  return psal * 12 + nvl(pcomm, 0);
end;
/
```

#### in 和 out 参数
1. 一般来讲，存储过程和存储函数的区别在于存储函数可以有一个返回值；而存储过程没有返回值
2. 过程和函数都可以通过 out 指定一个或多个输出参数。我们可以利用 out 参数，在过程和函数中实现返回多个值
	* 存储过程和存储函数都可以有 out 参数
	* 存储过程和存储函数都可以有多个 out 参数
	* 存储过程可以通过 out 参数来实现返回值
3. 用存储过程还是存储函数的原则：如果只有一个返回值，用存储函数；否则，就用存储过程
```
	--out参数：查询某个员工姓名、月薪和职位
create or replace procedure queryempinform(eno in number,
                                           pename out varchar2,
                                           psal   out number,
                                           pjob   out varchar2)
as
begin
  select ename, sal, empjob into pename, psal, pjob from emp where empno = eno;
end;
/
```

#### 在 out 参数中使用光标（查询某个部门中所有员工的所有信息）
1. 申明包结构
2. 包头
3. 包体：需要实现包头中声明的所有方法
```
	--包头
create or replace package mypackage
as
  type empcursor is ref cursor;
  procedure queryEmpList(dno in number, empList out empcursor);
end mypackage;
```

```
	--包体
create or replace package body mypackage
as
  procedure queryEmpList(dno in number, empList out empcursor)
as
begin
	--打开光标
  open emplist for select * from emp where deptno = dno;
end queryEmpList;
end mypackage;
```
4. 在应用中访问包中的存储过程：需要带上包名

