### 管理表
#### 认识表
1. 表的概念
	* 基本存储单位
	* 二维结构
	* 行和列
2. 约定
	* 每一行数据必须具有相同数据类型
	* 列名唯一
	* 每一行数据的唯一性

#### 数据类型
1. 字符型
	* char(n:max2000)、 nchar(n:max1000)
	* varchar2(n:max4000)、 nvarchar2(n:max2000)
2. 数值型
	* number(p,s)：p-->有效数字，s-->小数点后的位数（例：number(5,2)，可有数字123.45）
	* float(n)：存储二进制
3. 日期型(sysdate 自动获取当前时间)
	* date：公元前4712/1/1到公元9999/12/31，精确到秒
	* timestamp：精确到小数秒
4. 其他类型
	* blob：以二进制存放4GB 数据
	* clob：以字符串存放4GB 数据

#### 管理表
1. 创建表
```
CREATE TABLE table_name
(
column_name datatype, ...
);
```
2. 修改表
	* 添加字段：`ALTER TABLE table_name ADD column_name datatype;`
	* 更改字段数据类型：`ALTER TABLE table_name MODIFY column_name datatype;`
	* 删除字段：`ALTER TABLE table_name DROP COLUMN column_name;`
	* 修改字段名：`ALTER TABLE table_name RENAME COLUMN column_name TO new_column_name;`
	* 修改表名：`RENAME table_name TO new_table_name;`
3. 删除表
	* `TRUNCATE TABLE table_name;`(删除表中全部数据，不删除表，比 delete 快)
	* `DROP TABLE table_name;`(删除表和表中数据)
