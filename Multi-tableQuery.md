### 多表查询
#### 等值连接
* 例：`select e.empno,e.ename,e.sal,d.dname from emp e,dept d where e.deptno = d.deptno;`

#### 不等值连接
* 例：`select e.empno,e.ename,e.sal,s.grade from emp e,salgrade s where e.sal between s.losal and s.hisal;`

#### 外链接
* 核心：通过外链接，把对于连接条件不成立的记录，任然包含在最后的结果中
* 左外链接：当连接条件不成立的时候，等号左边的表仍然被包含（例：`select d.deptno,d.dname,count(e.empno) from emp e left join dept d on e.deptno = d.deptno group by d.deptno,d.dname;`）
* 右外链接：当连接条件不成立的时候，等号右边的表仍然被包含（例：`select d.deptno,d.dname,count(e.empno) from emp e right join dept d on e.deptno = d.deptno group by d.deptno,d.dname;`）

#### 自连接
* 核心：通过别名，将同一张表视为多张表（例：`select e.ename,b.ename from emp e,emp b where e.mgr=b.empno;`）
* 存在的问题：不适合操作大表
层次查询
* 某些情况下，可以替代自连接，本质上是一个单表查询
* 实际可看成树的层次遍历（例：`select level,empno,ename,sal,mgr from emp connect by prior empno = mgr start with mgr is null order by level;`）
* PRIOR 关键字：上一层的员工号是这层的老板号
* START WITH：从哪一个开始进行层次遍历
