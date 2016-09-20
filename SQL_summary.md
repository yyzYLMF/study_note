#SQL总结
##Author: Ryan yang
***
####1.mysql create table中经常出现的int(11)
int(11)中的数字表示最大显示宽度，当字段属性改为`UNSIGNED ZEROFILL`时会明显看到前面补0

####2.创建唯一索引的目的不是为了提高访问速度，而只是为了避免数据出现重复
	1. 方法一
	CREATE TABLE 'web_blog' (
		'id' int(8) unsigned NOT NULL,
		......
		UNIQUE KEY 'wblog_id' ('id')
	)
	2. 方法二
	CREATE UNIQUE INDEX wblog_id ON web_blog(id)
	
####3.mysql时间字段自动更新
ts TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP

####4.SQL常见语言（一）
a. ***去重***：select `DISTINCE` c1,c2,c3 from table1
b. ***不确定是否存在的插入***: insert into table (key,value) values ('123','456') `on duplicate key update` value='456';
插入一组数：若主键不存在则直接插入，记录值为values后面的值；若主键存在，则删除原来的并插入新的记录并使用on duplicate key update后面的值作为新值（数据库本质上是操作了两次：删除，添加）
c. ***删除整张表***: TRUNCATE TABLE 表名字 

####5.将Select结果横向打印
在select语句后面加`\G`，结果将每列占一行的显示（在列很多时使用） eg: select * from CubeDef \G;

####6.乱码问题
	a. 登陆后使用set names utf8可以防止select返回值中出现乱码
	
####7.创建数据库表原则（一般由两种类型的表组成）
	a. 描述现实世界对象的简单表，它们一般有一对一或一对多的关系。
	b. 描述两个现实世界对象的多对多关系的关联表
	
####8.MySQL批量删除某前缀数据库表
	a. Select CONCAT( 'drop table ', table_name, ';' ) FROM information_schema.tables Where table_name LIKE 'pre_%';
	b. 将生成的一组drop table pre_XXX 放到一个文件中并简单处理
	c. 在mysql中使用 source xxx.sql 执行即可