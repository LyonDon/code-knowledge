`Insert into 表名 (" "," ") VALUE (" "," ",...," ")`

`Insert into 表名 (列名,列名) VALUE (" "," ",...," ")(......)` 插入多行

`INSERT SELECT` 从另一个表中选取相应的列插入原表中


`Insert select` 从另一个表中选取相应的列插入原表中

`UPDATE 表名 SET 列名=‘’ WHERE 列名=‘’`插入表中某一行对应列

`DELETE FROM 表名 WHERE 列名=‘’` 删除某一列

`ALTER TABLE 表名 ADD 列名 数据结构` 更新表列

`DROP	TABLE	表名`	删除表

`RENAME	TABLE	A TO B`	重命名表

`SECLET 列名	from	表1	inner join	表2	ON	表1.列=表2.列`	内连接（inner join）	只显示相同的

`SELECT	列名	FROM	表1	LEFT	JOIN	表2	ON	表1.列=表2.列` （左）外连接（LEFT JOIN） 以表一为基准，表二没有的用NULL值填充


~~~sql
//创建表
CREATE TABLE orders{
	orderID	int	OTNULL	AUTO	INCREMENT,
    ordername	char(50)	NOTNULL
    ordername	char(50)	NOTNULL
    PRIMARY	KEY (orderID)
}engine=InnoDB;
~~~



`USE`：选择数据库
`SELECT`：选择列
`DESC`：降序
`DISTINCT`：删除相同的元素
`NOT`：否定之后所跟的子句
`%`：通配符（多个字符）
`_`：通配符（单个字符）
`REGEXP`：类似于LIKE	LIKE+‘…%…’=REGEXP+‘’
`\\`：匹配特殊字符
`CONCAT`：拼接多个列，用，
`AS`:别名	SELECT A AS B	B是A的别名

处理日期使用Date（）
>Date()=……或 Date（）BTEWEEN……AND……

优先级|聚集函数
:--:|:--:
SELECT：返回列或者表达式|AVG()返回某列平均数
FROM：从中检索数据的表|COUNT（）返回某列行数（可指定列名，忽略空行）
WHERE：行级过滤|MAX（）返回某列最大值
GROUP BY：组级过滤|MIN（）返回某列最小值
HAVING：组级过滤（配合GROUP BY）|SUM（）返回某列元素和
ORDER BY：分组排序|
LIMIT：设置界限|



**主键可以由多个列组成**
**外键是表中的某一列，它是另一个表的主键，定义了两个表的关系**
