### 约束
#### 约束的作用
1. 定义规则
2. 确保完整性（数据精确性和可靠性）

#### 非空约束
1. 在创建表时设置非空约束
	* `CREATE TABLE table_name(column_name datatype NOT NULL, ...);`
2. 在修改表时添加非空约束
	* `ALTER TABLE table_name MODIFY column_name datatype NOT NULL;`
3. 在修改表时去除非空约束
	* `ALTER TABLE table_name MODIFY column_name datatype NULL;`

#### 主键约束
1. 作用：确保表当中每一行数据的唯一性（非空，唯一，一张表只能设计一个主键约束，主键约束可以由多个字段构成（联合主键或复合主键））
2. 在创建表时设置主键约束
	* `CREATE TABLE table_name(column_name datatype PRIMARY KEY, ...);`
	* `CREATE TABLE table_name(column_name datatype, ... CONSTRAINT constraint_name PRIMARY KEY(column_name1,...));`（可在 user_constraints 数据字典查约束）
3. 在修改表时添加主键约束
	* `ALTER TABLE table_name ADD CONSTRAINT constraint_name PRIMARY KEY(column_name1,...);`
4. 更改约束的名称
	* `ALTER TABLE table_name RENAME CONSTRAINT old_name TO new_name;`
5. 删除主键约束
	* `ALTER TABLE table_name DISABLE|ENABLE CONSTRAINT constraint_name;`(禁用/启用，可在 user_constraints 数据字典 status 查状态)
	* `ALTER TABLE table_name DROP CONSTRAINT constraint_name;`(真实删除约束)
	* `ALTER TABLE table_name DROP PRIMARY KEY[CASCADE];`(存在外键时用 cascade 会删除相关数据)

#### 外键约束
1. 在创建表时设置外键约束
	* `CREATE TABLE table1(column_name datatype REFERENCES table2(column_name),...);`

	**注：table2是主表，table1是从表，设置外键约束时，主表的字段必须是主键，主从表中相应的字段必须是同一个数据类型，从表中外键字段的值必须来自主表中的相应字段的值，或者为null值**
	* `CREATE TABLE table_name(column_name datatype, ... CONSTRAINT constraint_name FOREIGN KEY (column_name) REFERENCES table_name(column_name) [ON DELETE CASCADE]);`

	**注：ON DELETE CASCADE 的作用：主表中删除数据时，从表相应数据也被删除**
2. 在修改表时添加外键约束
	* `ALTER TABLE table_name ADD CONSTRAINT constraint_name FOREIGN KEY(column_name) REFERENCES table_name(column_name) [ON DELETE CASCADE];`
3. 删除外键约束
	* `ALTER TABLE table_name DISABLE|ENABLE CONSTRAINT constraint_name;`(禁用/启用，可在 user_constraints 数据字典 status 查状态)
	* `ALTER TABLE table_name DROP CONSTRAINT constraint_name;`(真实删除约束)

#### 唯一约束
1. 作用：保证字段值的唯一性
2. 唯一约束和主键约束的区别：
	* 主键字段值必须是非空的
	* 唯一约束允许有一个空值
	* 主键在每张表中只能有一个
	* 唯一约束在每张表中可以有多个
3. 在创建表时设置唯一约束
	* `CREATE TABLE table_name(column_name datatype UNIQUE,...);`

	**注：table2是主表，table1是从表，设置外键约束时，主表的字段必须是主键，主从表中相应的字段必须是同一个数据类型，从表中外键字段的值必须来自主表中的相应字段的值，或者为null值**
	* `CREATE TABLE table_name(column_name datatype, ... CONSTRAINT constraint_name UNIQUE(column_name));`
2. 在修改表时添加唯一约束
	* `ALTER TABLE table_name ADD CONSTRAINT constraint_name UNIQUE(column_name);`
3. 删除唯一约束
	* `ALTER TABLE table_name DISABLE|ENABLE CONSTRAINT constraint_name;`(禁用/启用，可在 user_constraints 数据字典 status 查状态)
	* `ALTER TABLE table_name DROP CONSTRAINT constraint_name;`(真实删除约束)

#### 检查约束
1. 作用：表中的值更具有实际意义
2. 在创建表时设置检查约束
	* `CREATE TABLE table_name(column_name datatype CHECK(expressions), ...);`
	* `CREATE TABLE table_name(column_name datatype, ... CONSTRAINT constraint_name CHECK(expressions));`
3. 在修改表时添加检查约束
	* `ALTER TABLE table_name ADD CONSTRAINT constraint_name CHECK(expressions);`
4. 删除检查约束
	* `ALTER TABLE table_name DISABLE|ENABLE CONSTRAINT constraint_name;`(禁用/启用，可在 user_constraints 数据字典 status 查状态)
	* `ALTER TABLE table_name DROP CONSTRAINT constraint_name;`(真实删除约束)
