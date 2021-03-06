### 查询语句
1. 基本语句查询
`SELECT［DISTINCT］ column_name1,...|* FROM table_name [WHERE conditions];`
2. 在sql plus 中设置格式(COLUMN 可简写成 COL)
	* `COLUMN column_name HEADING new_name;`
	* `COLUMN column_name FORMAT dataformat;`
		* 字符型（a10:表示只显示10位字符）
		* 数值型（9999.9：表示最多只显示小数点前4位，小数点后1位）
	* `COLUMN column_name FORMAT CLEAR;`
3. 查询表中的所有字段：`SELECT * FROM table_name;`
4. 查询表中的指定字段：`SELECT column_name1, column_name2,... FROM table_name;`
5. 给字段设置别名：`SELECT column_name AS new_name,... FROM table_name;`(AS 可以省略，用空格隔开原来的字段名和新字段名即可)
6. 逻辑运算符的优先级：按 not、and、or 的顺序依次递减，比较运算符的优先级高于逻辑运算符
7. 模糊查询
	1. 通配符的使用（_, %）
		* 一个_只能代表一个字符
		* %可以代表0到多个任意字符
	2. 使用 `LIKE` 查询
8. 范围查询
	* `BETWEEN...AND`
	* `IN/NOT IN`
9. 对查询结果排序
	* `SELECT ... FROM ... [WHERE] ORDER BY column1 DEAC/ASC, ...;`
10. case...when 语句的使用
	* `CASE column_name WHEN value1 THEN result1,... [ELSE result] END;`
	* `CASE WHEN column_name=value1 THEN result1,...[ELSE result] END;`
11. decode 函数的使用
	* `decode(column_name, value1,result1,...defaultvalue);`（defaultvalue 相当于 case when 里的 else，也可省略）
