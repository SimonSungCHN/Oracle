### 操作数据
#### 添加数据
1. INSERT 语句：`INSERT INTO table_name (column1, column2, ...) values(value1, value2, ...);`(如果自动添加默认值，只需在创建表或者修改表时在字段后面添加 default ‘value’)

#### 复制表数据
1. 在建表时复制：`CREATE TABLE table_new AS SELECT column1, ...|* FROM table_old;`
2. 在添加时复制：`INSERT INTO table_new [(column1,...)] SELECT column1, ...|* FROM table_old;`

#### 修改表数据
1. UPDATE 语句：`UPDATE table_name SET column1=value1, ... [WHERE conditions];`

#### 删除表数据
1. DELETE 语句：`DELETE FROM table_name [WHERE conditions];`
