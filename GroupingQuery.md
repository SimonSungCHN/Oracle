### 分组查询
#### 分组函数
* 作用于一组数据，并对一组数据返回一个值
* AVG（平均值，例：`select avg(sal) from emp;`）
* SUM（合计，例：`select sum(sal) from emp;`）
* MIN（最小值，例：`select min(sal) from emp;`）
* MAX（最大值,例：`select max(sal) from emp;`）
* COUNT（个数，例：`select count(*) from emp;`）
* WM_CONCAT：（行转列，例：`swlwct deptno, wm_concat(ename) from emp group by deptno;`，得到的结果是按部门进行归类）
* **注：分组函数会自动忽略空值，当使用NVL函数使得分组函数无法忽略空值（例：`select count(nvl(comm,0)) from emp;`）**
#### 分组数据
* GROUP BY：可以使用 GROUP BY 字句将表中的数据分成若干组（例：`select deptno,avg(sal) from emp group by deptno;`）
* **注：在 SELECT 列表中所有未包含在组函数中的列都应该包含在 GROUP BY 子句中，例：`select a,b,c,组函数(x) from table group by a,b,c;`包含在 GROUP BY 子句中的列不必包含在 SELECT 列表中，例：`select avg(sal) from emp group by deptno;`**
#### 过滤分组
* HAVING：例：`select deptno,avg(sal) from emp group by deptno having avg(sal)>2000;`
* WHERE 与HAVING 的区别：不能在 WHERE 子句中使用组函数（注意），但可以在 HAVING 子句中使用组函数，从 SQL 优化的角度上看，尽量使用 WHERE，因为 HAVING 先分组后过滤，WHERE 先过滤后分组
#### 排序
* ORDER BY
#### GROUP BY 语句的增强（可用于报表）
* group by rollup(a,b) = group by a,b + group a + group by null
