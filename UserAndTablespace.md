### 用户和表空间
#### 用户
1. 使用 system 用户登录
	1. 系统用户
		* sys, system：sys 权限大于 system
 		* sysman：操作企业管理器
 		* scott：密码 tiger
	2. 用户登录
		[username/password] [@server] [as sysdba|sysoper]
		例：system/123456 @orcl as sysdba (orcl就是自己设置的服务名，切换用户时前面加 connect)
2. 查看登录用户
	* `show user` 命令
	* dba_users 数据字典 （命令：`desc dba_users`）
3. 启用 scott 用户
	1. 启用用户的语句：`ALTER USER username ACCOUNT UNLOCK;`(例：`alter user scott account unlock`)
	2. 使用 scott 用户登录SQL Plus

#### 表空间
1. 表空间概述
	* 数据库与表空间
	* 表空间与数据文件
2. 表空间的分类
	* 永久表空间：永久化存储的数据
	* 临时表空间：中间执行的过程，过后自动释放
	* UNDO 表空间：保存被修改之前的数据，可进行数据回滚
3. 查看用户的表空间
	* dba_tablespaces、user_tablespaces 数据字典
	* dba_users、user_users 数据字典
	* 设置用户的默认或临时表空间：`ALTER USER username DEFAULT|TRMPORARY TABLESPACE tablespace_name;`(例：`alter user system default tablespace system;`)
4. 创建表空间
	* `CREATE [TEMPORARY] TABLESPACE tablespace_name TEMPFILE|DATAFILE 'xx.dbf' SIZE xx;`(例：创建永久表空间--->`create tablespace test1_tablespace datafile 'test1file.dbf' size 10m;` 创建临时表空间--->`create temporary tablespace temptest1_tablespace tempfile 'tempfile.dbf' size 10m;`)
	* dba_data_files、dba_temp_files 数据字典 file_name 字段可查看文件路径
5. 修改表空间
	1. 修改表空间的状态
		* 设置联机或脱机状态：`ALTER TABLESPACE tablespace_name ONLINE|OFFLINE;`（dba_tablespaces 数据字典 status 字段可查看状态）
		* 设置只读或可读写状态：`ALTER TABLESPACE tablespace_name READ ONLY|WRITE;`（dba_tablespaces数据字典 status 字段可查看状态）
6. 增加数据文件
	* 增加数据文件：`ALTER TABLESPACE tablespace_name ADD DATAFILE 'xx.dbf' SIZE xx;`
	* 删除数据文件：`ALTER TABLESPACE tablespace_name DROP DATAFILE 'xx.dbf';`（不能删除第一个数据文件）
7. 删除表空间
	* `DROP TABLESPACE tablespace_name [INCLUDING CONTENTS];`(把数据文件一起删掉要加 including contents)
