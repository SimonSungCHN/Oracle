### 子查询
#### 子查询注意的10个问题
1. 子查询语法中的小括号
2. 子查询的书写风格
3. 可以使用子查询的位置：where, select, having, from
	* select 后面的子查询只能是单行子查询
4. 不可以使用子查询的位置：group by
5. 强调：from 后面的子查询
6. 注意：主查询和子查询可以不是同一张表
7. 一般不在子查询中使用排序；但在 Top-N 分析问题中，必须对子查询排序（例：`select rownum,empno,ename,sal from (select rownum,empno,ename,sal from emp order by sal desc) where rownum<=3;`）
	* ROWNUM:行号、伪列
	* 行号需要主要的两个问题
		1. 行号永远按照默认的顺序生成
		2. 行号只能使用<, <=；不能使用>, >=


8. 一般先执行子查询，再执行主查询；但相关子查询例外
	* 例：`select empno,ename,sal,(select avg(sal) from emp where deptno=e.deptno) avgsal from emp e where sal>(select avg(sal) from emp where deptno=e.deptno);`
9. 单行子查询只能使用单行操作符；多行子查询只能使用多行操作符
	* 单行子查询：只有一个数据
	* 单行操作符：=, >, >=, <, <=, <>
	* 多行子查询：两个以上的数据
	* 多行操作符：in（等于列表中的任何一个）、any（和子查询返回的任意一个值比较）、all（和子查询返回的所有值比较）
10. 注意：子查询中是 null 值问题
	* 当行子查询中null值问题：子查询不返回任何行
	* 多行子查询
		* 当子查询中有 null 值时，不要使用 not in，不然得不到正确的结果，要在后面加条件'xx is not null'
#### 分页查询
* 每页显示4条记录，显示第二页，按照月薪降序：`select r,empno,ename,sal from(select rownum r,empno,ename,sal from (select empno,ename,sal from emp order by sal desc) where rownum<=8) where r>=5;`
* 判断sql语句优劣
	1. `explain plan for + (sql语句);`
	2. `select * from table(dbms_xplan.display);`
