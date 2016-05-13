# mysql 笔记


## delimiter 命令
MySQL中delimit命令。
这个命令与存储过程没什么关系。
其实就是告诉mysql解释器，该段命令是否已经结束了，mysql是否可以执行了。
即改变输入结束符。
默认情况下，delimiter是分号“;”。
在命令行客户端中，如果有一行命令以分号结束，
那么回车后，mysql将会执行该命令。
但有时候，不希望MySQL这么做。因为可能输入较多的语句，且语句中包含有分号。
默认情况下，不可能等到用户把这些语句全部输入完之后，再执行整段语句。
因为mysql一遇到分号，它就要自动执行。
这种情况下，就可以使用delimiter，把delimiter后面换成其它符号，如#或$$。
此时，delimiter作用就是对整个小段语句做一个简单的封装。
此命令多用在定义子程序，触发程序等mysql自己内嵌小程序中。

```sql

# 遇到$$后才执行整段代码

DELIMITER $$
CREATE PROCEDURE `pr_add`(a INT, b INT)
	BEGIN
		DECLARE c INT;
		IF a IS NULL THEN
			SET a = 0;
		END IF;
		IF b IS NULL THEN
			SET b = 0;
		END IF;
		SET c = a + b;
		SELECT c AS SUM;
	END$$
DELIMITER ;

```

## MySQL存储过程的基本函数
### 字符串类

- 返回字串字符集
**CHARSET(str) **
- 连接字串
**CONCAT (string2 [,... ]) **
- 返回substring首次在string中出现的位置,不存在返回0
**INSTR (string ,substring )** 
- 转换成小写
**LCASE (string2 )** 
- 从string2中的左边起取length个字符
**LEFT (string2 ,length )** 
- 取string2最后length个字符
**RIGHT(string2,length)** 
- string长度
**LENGTH (string )** 
从文件读取内容
**LOAD_FILE (file_name )** 
- 同INSTR,但可指定开始位置
**LOCATE (substring , string [,start_position ] )** 
- 重复用pad加在string开头,直到字串长度为length
**LPAD (string2 ,length ,pad )** 
- 去除前端空格
**LTRIM (string2 )** 
- 重复count次
**REPEAT (string2 ,count )** 
- 在str中用replace_str替换search_str
**REPLACE (str ,search_str ,replace_str )** 
- 在str后用pad补充,直到长度为length
**RPAD (string2 ,length ,pad)** 
- 除后端空格
**RTRIM (string2 )** 
- 符比较两字串大小
**STRCMP (string1 ,string2 )** 
- 从str的position开始,取length个字符
**SUBSTRING (str , position [,length ])** 
> 注：mysql中处理字符串时，默认第一个字符下标为1，即参数position必须大于等于1 

### 数学类
- 绝对值
**ABS (number2 )** 
- 十进制转二进制
**BIN (decimal_number )** 
- 向上取整
**CEILING (number2 )** 
- 进制转换
**CONV(number2,from_base,to_base)** 
- 向下取整
**FLOOR (number2 )** 
- 保留小数位数
**FORMAT (number,decimal_places )** 
- 转十六进制
**HEX (DecimalNumber )** 
> 注：HEX()中可传入字符串，则返回其ASC-11码，如HEX('DEF')返回4142143也可以传入十进制整数，返回其十六进制编码，如HEX(25)返回19

- 求最小值
**LEAST (number , number2 [,..])** 
- 求余
**MOD (numerator ,denominator )** 
- 求指数
**POWER (number ,power )** 
- 随机数
**RAND([seed])** 
- 四舍五入,decimals为小数位数]
**ROUND (number [,decimals ])** 

### 日期时间类
- 将time_interval加到date2
**ADDTIME (date2 ,time_interval )** 
- 转换时区
**CONVERT_TZ (datetime2 ,fromTZ ,toTZ )** 
- 当前日期
**CURRENT_DATE ( )** 
- 当前时间
**CURRENT_TIME ( )** 
- 当前时间戳
**CURRENT_TIMESTAMP ( )** 
- 返回datetime的日期部分
**DATE (datetime )** 
- 在date2中加上日期或时间
**DATE_ADD (date2 , INTERVAL d_value d_type )** 
- 使用formatcodes格式显示datetime
**DATE_FORMAT (datetime ,FormatCodes )** 
- 在date2上减去一个时间
**DATE_SUB (date2 , INTERVAL d_value d_type )** 
- 两个日期差
**DATEDIFF (date1 ,date2 )** 
- 返回日期的天
**DAY (date )** 
- 英文星期
**DAYNAME (date )** 
- 星期(1-7) ,1为星期天
**DAYOFWEEK (date )** 
- 一年中的第几天
**DAYOFYEAR (date )** 
- 从date中提取日期的指定部分
**EXTRACT (interval_name FROM date )** 
- 给出年及年中的第几天,生成日期串
**MAKEDATE (year ,day )** 
- 生成时间串
**MAKETIME (hour ,minute ,second )** 
- 英文月份名
**MONTHNAME (date )** 
- 当前时间
**NOW ( )** 
- 秒数转成时间
**SEC_TO_TIME (seconds )** 
- 字串转成时间,以format格式显示
**STR_TO_DATE (string ,format )** 
- 两个时间差
**TIMEDIFF (datetime1 ,datetime2 )** 
- 时间转秒数
**TIME_TO_SEC (time )** 
- 第几周
**WEEK (date_time [,start_of_week ])** 
- 年份
**YEAR (datetime )** 
- 月的第几天
**DAYOFMONTH(datetime)** 
- 小时
**HOUR(datetime)** 
- date的月的最后日期
**LAST_DAY(date)** 
- 微秒
**MICROSECOND(datetime)** 
- 月
**MONTH(datetime)** 
- 分返回符号,正负或0
**MINUTE(datetime)** 
- 开平方
**SQRT(number2)** 

