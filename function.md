### 函数
1. 函数的作用
	* 方便数据的统计
	* 处理查询结果
2. 函数分类
	1. 数值函数
		* 四舍五入：ROUND(n[,m]),省略m表示0，m>0 表示小数点后 m 位进行四舍五入，m<0 表示小数点前 m 位进行四舍五入（例：`select round(23.4),round(23.45,1),round(23.45,-1) from dual;`）
		* 取整函数：CEIL(n)向上取整，FLOOR(n)向下取整（例：`select ceil(23.45),floor(23.45) from dual;`）
		* 绝对值函数：ABS(n)
		* 取余数函数：MOD(m,n),m 代表被除数，n 代表除数，即m%n的值，如果两个中有一个为null，结果为null
		* 幂函数：POWER(m,n), m 的 n 次幂
		* 取平方根函数：SQRT(n), n 的平方根
		* 三角函数：SIN(n)、ASIN(n)、COS(n)、ACOS(n)、TAN(n)、ATAN(n), n 是弧度值

	2. 字符函数
		* 大小写转换函数：
			 * UPPER(char)：小写转大写
			 * LOWER(char)：大写转小写
			 * INITCAP(char)：首字母转大写
		* 获取子字符串函数：SUBSTR(char,[m[,n]])
			 * m 是取子串的开始位置，从1开始
			 * n 是截取子串的位数，当 n 省略时表示从 m 的位置截取到字符串末尾
			 * m 为0表示从字符串的字母开始首截取，m 为负数表示从字符串的尾部开始截取
		* 获取字符串长度函数：LENGTH(char)
		* 字符串连接函数：CONCAT(char1, char2),与||操作符的作用一样，即 char1||char2
		* 去除子串函数：
			* TRIM(c2 FROM c1),从字符串 c1 中去除**字符** c2，c2 省略是去除首尾空格
			* LTRIM(c1[,c2]),只去除头部开始的连续出现的 c2 字符，c2 省略是去除首部空格
			* RTRIM(c1[,c2]),只去除尾部开始的连续出现的 c2 字符，c2 省略是去除尾部空格
		* 替换函数：REPLACE(char,s_string[,r_string]),省略 r_string 用空格替换

	3. 日期函数
		* 系统时间：SYSDATE(默认格式：DD-MON-RR)
		* 日期操作
			* ADD_MONTHS(date,i)：返回在指定日期上添加的月份（i是整数，如是小数，会自动截取整数部分；如是负数，会在原日期上减去月份）
			* NEXT_DAY(date,char)：如果 char 的值是“星期一”，则返回 date 指定日期的下周一是哪天
			* LAST_DAY(date)：返回当月的最后一天
			* MONTHS_BETWEEN(date1,date2)：两个日期之间相隔的月份
			* EXTRACT(date FROM datetime)：获取相应部分(例：year from sysdate、month from sysdate、hour from timestamp '2016-12-27 14:33:34')

	4. 转换函数
		* 日期转换成字符的函数：TO_CHAR(date[,fmt[,params]]),date 表示将要转换的日期，fmt 表示转换的格式，params 表示日期的语言（通常不写）（例:`select to_char(sysdate,'YYYY-MM-DD') from dual;`）
			* YY YYYY YEAR
			* MM MONTH
			* DD DAY
			* HH24 HH12
			* MI SS
		* 字符转换成日期的函数：TO_DATE(char[,fmt[,params]])（例:`select to_date('2016-12-27','YYYY-MM-DD HH24:MI:SS') from dual;`,最后是按照系统默认格式显示日期）
		* 数字转换成字符的函数：TO_CHAR(number[,fmt])（例：`select to_char(12345.678,'$99,999.999') from dual;`）
			* 9：显示数字并忽略前面的0
			* 0：显示数字，位数不足，用0补齐
			* .或D：显示小数点
			* ,或G：显示千位符
			* $：美元符号
			* S：加正负号(前后都可以) 
		* 字符转换成数字的函数：TO_NUMBER(char[,fmt]),fmt 是转换的格式，可以省略
