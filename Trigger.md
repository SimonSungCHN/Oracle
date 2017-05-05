### 触发器
#### 触发器的概念
1. 数据库触发器是一个与表相关联的、存储的PL/SQL 程序
2. 每当一个特定的数据操作语句（insert、update、delete）在指定的表上发出的，Oracle 自动地执行触发器中定义的语句序列
3. 创建触发器
```
	--插入新员工后自动打印
create trigger saynewemp 
after insert
on emp
declare
begin
  dbms_output.put_line('成功插入新员工');
end;
/
```

#### 触发器的应用场景
1. 复杂的安全性检查
```
	--禁止在非工作时间插入数据
create or replace trigger securityemp 
before insert
on emp
declare
begin
  if to_char(sysdate, 'day') in ('星期六', '星期日') or
     to_number(to_char(sysday, 'hh24')) not between 9 and 18 then 
	--禁止 insert 新员工(错误代码 -20000 ~ -20999)
     raise_application_error(-20001, '禁止在非工作时间插入新员工');
  end if;
end;
/
```

2. 数据确认
```
	--涨后的薪水不能少于涨前的薪水
create or replace trigger checksalary 
before update
on emp
for each row
declare
begin
  if :new.sal < :old.sal then 
	
     raise_application_error(-20002, '涨后的薪水不能少于涨前的薪水');
  end if;
end;
/
```

3. 实现审计功能(基于值的审计功能)
```
	--涨工资，当涨后的薪水超过6000时，审计该员工的信息（表 audit_info 用来审计）
create or replace trigger do_audit_emp_salary 
after update
on emp
for each row
declare
begin
  if :new.sal > 6000 then 
	insert into audit_info(information) values(:new.empno||' '||:new.ename||' '||:new.sal);
  end if;
end;
/
```

4. 完成数据的备份和同步
```
	--当给员工涨完工资后，自动备份新的工资到备份表中（表 audit_info 用来审计）
create or replace trigger sync_salary 
after update
on emp
for each row
declare
begin
	--当主表更新后，自动更新备份表（emp_back）
   update emp_back set sal = :new.sal where empno = :new.empno;
  end if;
end;
/
```

#### 触发器的类型
1. 语句级触发器：在指定的操作语句操作之前或之后执行一次，不管这条语句影响了多少行，针对是表
2. 行级触发器：除法语句作用的每一条记录都被触发。在行级触发器中使用 :old 和 :new 伪记录变量，识别值的状态，针对是行
	* :old 和 :new 代表同一条记录
	* :old 表示操作该行之前这一行的值
	* :new 表示操作该行之后这一行的值
3. 例：一次性向一张表插入3条数据，用语句级触发器只调用1次，用行级触发器会调用3次

#### 创建触发器的语法
```
CREATE [or REPLACE] TRIGGER 触发器名
{BEFORE|AFTER}
{DELETE|INSERT|UPDATE[OF 列名]}
ON 表名
[FOR EACH ROW [WHEN(条件)]]  (指明触发器的类型，有这句话就是行级触发器；没有就是语句级触发器)
PLSQL 块
```
