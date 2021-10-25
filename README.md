# MySQL数据库(建议关键字使用大写)

启动和关闭MySQL数据库：

```
# net start mysql
# net stop mysql
```

连接数据库：

```
# mysql -u‘用户名’ -p‘密码’
# mysql -u‘用户名’ -p  #以密文输入密码
# mysql -h‘ip地址’ -p‘访问的计算机的数据库密码’
# mysql --host=ip --user=root --password==连接目标的密码
```

退出：

```
# exit
# quit
```

注释：

```
# 单行注释： --注释’ || ‘#注释’ ——MySQL独有
# 多行注释： /* 注释 */
```

SQL语句分类：

```sql
# DDL(Data Definition Language)数据定义语言 
		用来定义数据库对象:数据库，表，列等。关键字:create, drop,alter等
# DML(Data Manipulation Language)数据操作语言 
		用来对数据库中表的数据进行增删改。关键字:insert, delete, update等
# DQL(Data Query Language)数据查询语言 
		用来查询数据库中表的记录(数据)。关键字:select, where等
# DCL(Data Control Language)数据控制语言(了解) 
		用来定义数据库的访问权限和安全级别，及创建用户。关键字:GRANT，REVOKE等
```

查看默认字符集：

```sql
show variables like "%char%";
```

## 1、操作数据库：CRUD    DDL

### 1、1创建数据库：C(Create)

```sql
CREATE DATABASE '数据库名称'; # 创建数据库
CREATE DATABASE IF NOT EXISTS '数据库名称'; # 判断创建的数据库是否存在，如不存在则创建
CREATE DATABASE '数据库名称' CHARACTER SET '字符集'; # 创建数据库，并指定该数据库默认字符集
CREATE DATABASE IF NOT EXISTS '数据库名称' CHARACTER SET '字符集'; # 复合条件语句
```

### 1、2查询数据库：R(Retrieve)

```sql
SHOW DATABASES; # 查询所有数据库名称
SHOW CREATE DATABASE '数据库名称'; # 查询某个数据库的字符集
```

### 1、3修改数据库：U(Update)

```sql
ALTER DATABASE '数据库名称' CHARACTER SET '字符集'; # 修改数据库默认字符集
```

### 1、4删除数据库：D(Delete)

```sql
DROP DATABASE '数据库名称'; # 删除数据库
DROP DATABASE IF EXISTS '数据库名称'; # 判断删除数据库是否存在，如存在则删除
# ！！！慎用
```

### 1、5使用数据库

```sql
USE '数据库名称'; # 使用数据库
SELECT DATABASE(); # 查询当前正在使用的数据库
```

## 2、操作表：CRUD    DDL

### 2、1创建数据表：C(Create)

```sql
# 常用数据库类型
	1.int：整数类型
		*age int
	2.double：小数类型
		*score double(5,2) # 括号内代表5位小数，保留2位小数点
	3.date：日期 
		只包含年月日，yy-MM-dd
	4.datetime：日期
		包含年月日，时分秒 yy-MM-dd HH:mm:ss	
	5.timestamp：时间戳
		包含年月日，时分秒 yy-MM-dd HH:mm:ss	# 如果不赋值，或赋值位null，则默认使用当前系统时间
	6.varchar：字符串
		*name varchar(20) # 姓名最大20个字符
--------------------------------------------------------------------------------------------------
# 语法
CREATE TABLE '表名'(
	'列名1' '数据类型1',
   	'列名2' '数据类型2',
    ....
    '列名n' '数据类型n' # ！！！注意，最后一句不带 ','
);
# 案例 -->
create table student(
    -> id int,
    -> name varchar(20),
    -> age int,
    -> score double(4,1),
    -> birthday date,
    -> insert_time timestamp
    -> );
--------------------------------------------------------------------------------------------------
CREATE TABLE stu LIKE student; # 复制student表并重命名为stu
```

### 2、2查询数据表：R(Retrieve)

```sql
SHOW TABLES; #查询数据库中所有的表名称
DESC '表名'; #查询表结构
SHOW CREATE TABLE '表名'; # 查询某个数据表的字符集
```

### 2、3修改数据表：U(Update)

```sql
ALTER TABLE '表名' RENAME TO '新的表名'; # 重命名表名
ALTER TABLE '表名' CHARACTER SET '字符集'; # 修改数据表默认字符集
ALTER TABLE '表名' ADD '列名' '数据类型'; # 添加一列
ALTER TABLE '表名' CHANGE '旧列名' '新列名' '数据类型'; # 修改 列名称和其数据类型
ALTER TABLE '表名' MODIFY '列名' '数据类型'; # 修改 列名称的类型
ALTER TABLE '表名' DROP '列名'; #删除列
```

### 2、4删除数据表：D(Delete)

```sql
DROP TABLE '表名'; # 删除表
DROP TABLE IF EXISTS '表名'; # 判断删除表是否存在，如存在则删除
```

## 3、操作数据表中内容 DML

### 3、1增加数据

```sql
INSERT INTO '表名'('列名1','列名2'...'列名n') VALUES('值1','值2'...'值n'); 
	# 语法 列名与值一一对应
INSERT INTO '表名' VALUES('值1','值2'...'值n'); 
	# 如果表名后不定义列名，则默认给所有列添加值
	# 除了数字类型，其他类型需要使用引号(单双都可以)引起来
```

### 3、2删除数据

```SQL
DELETE FROM '表名' [WHERE 条件];
DELETE FROM '表名'; # 如果不加条件，则删除全部记录
TRUNCATE TABLE '表名'; # 删除表，然后创建一个一模一样的表
```

### 3、3修改数据

```SQL
UPDATE '表名' SET '列名1'='值1','列名2'='值2'...'列名n'='值n' [WHERE 条件];
	# 如果不加任何条件，则修改全部记录
```

## 

## 4、查询表中记录 DQL

```sql
SELECT * FROM '表名'; # 查询表名中的所有记录
# 语法
SELECT
	字段列表
FROM
	表名列表
WHERE
	条件列表
GROUP BY
	分组字段
HAVING
	分组之后的条件
ORDER BY
	排序
LIMIT
	分页限定

# 基础查询
1、多个字段查询
	 SELECT '字段名1'，'字段名2'...'字段名n' FROM '表名';
	 SELECT * FROM '表名'; # 查询所有字段可以用*代替
2、去除重复
	SELECT DISTINCT '字段名1' FROM '表名';
3、计算列
	SELECT '字段名1'，'字段名2','字段名1'+'字段名2' FROM '表名';
	# 如果有null参与运算，计算结果都为null
	SELECT '字段名1'，'字段名2','字段名1' + IFNULL('字段名2',0) FROM '表名';
	# 判断字段的值是否位null，是则替换为0 IFNULL()
4、起别名
	SELECT '字段名1'，'字段名2','字段名1' + IFNULL('字段名2',0) AS '别名' FROM '表名';
	# AS可省略

# 条件查询
1、where子句后跟条件
	SELECT * FROM student WHERE age>20; # 查询年龄大于20的人
	SELECT * FROM student WHERE age>=20;
	SELECT * FROM student WHERE age<20;
	SELECT * FROM student WHERE age<=20;
	SELECT * FROM student WHERE age=20;
	SELECT * FROM student WHERE age!=20; # '!=' <--> '<>  两种都是不等于
	
	SELECT * FROM student WHERE age>20 AND <30; # 查询20到30之间的年龄
	SELECT * FROM student WHERE age>20 && age<30; # 等同于上方，但不推荐
	SELECT * FROM student WHERE age BETWEEN 20 AND 30; # 等同于上方
	
	SELECT * FROM student WHERE age=22 OR age=30; # 查询年龄等于22或30岁的人
	SELECT * FROM student WHERE age IN (22,30); # 等同于上方
	
	SELECT * FROM student WHERE english = NULL; # 不对 null值不能使用 = (!=) 判断
	SELECT * FROM student WHERE english IS NULL; # 正确写法 查询英语为null的
	SELECT * FROM student WHERE english IS NOT NULL; # 查询英语不为null的
	
	SELECT * FROM student WHERE name LIKE '马%'; # 查询姓马的
	SELECT * FROM student WHERE name LIKE '_化%'; # 查询第二个字是化的
	SELECT * FROM student WHERE name LIKE '___'; # 查询三个字的
	SELECT * FROM student WHERE name LIKE '%马%'; # 查询姓名中有马的
			
2、运算符
	# >、>=、<、<=、=、!=、<>
	# BETWEEN AND
	# LIKE  单个占位符 '_' 多个占位符'%'
	# IS NULL
	# AND 或 &&
	# OR 或 ||
	# NOT 或 !
```

### 4、1排序查询

```sql
SELECT * FROM '表名' ORDER BY '字段'; # 根据字段排序  默认升序排列
SELECT * FROM '表名' ORDER BY '字段' ASC; # 根据字段排序 升序
SELECT * FROM '表名' ORDER BY '字段' DESC; # 根据字段排序 降序
SELECT * FROM '表名' ORDER BY '字段1' ASC，'字段2' DESC; # 字段1升序排列 字段2降序排列
# 排序方式: # ASC:升序，默认的 # DESC:降序
#例子
SELECT * FROM student ORDER BY math ASC，english ASC; # 按数学成绩排名，分数一样则按由于成绩排序
# 如果有多个排序条件，则当前边的条件值一样时，才会判断第二条件。
```

### 4、2聚合函数

```sql
# 将一列数据作为一个整体，进行纵向的计算  单行单列
# 注意：聚合运算排除NULL值
# 解决方式：	 1、选择不包含非空列进行计算（主键）	
				SELECT COUNT(*) FROM '表名'; # 只要有一列不为空就计算--不推荐
#			2、IFNULL 函数

SELECT COUNT('列名') FROM '表名'; 	# 1、count：计算个数	
SELECT MAX('列名') FROM '表名';		# 2、max：计算最大值
SELECT MIN('列名') FROM '表名';		# 3、min：计算最小值
SELECT SUM('列名') FROM '表名';		# 4、sum：求和
SELECT AVG('列名') FROM '表名';		# 5、avg：计算平均值
```

### 4、3分组查询

```sql
# 分组之后查询字段：分组字段、聚合函数
SELECT sex,AVG(math) FROM student GROUP BY sex; # 按照性别分组，分别查询男女同学平均分
SELECT sex,AVG(math),COUNT(id) FROM student GROUP BY sex; # 按照性别分组，分别查询男女同学平均分,人数
SELECT sex,AVG(math),COUNT(id) FROM student WHERE math > 70 GROUP BY sex; # 按照性别分组，分别查询男女同学平均分,人数 要求：分数低于70分，不参与分组
SELECT sex,AVG(math),COUNT(id) FROM student WHERE math > 70 GROUP BY sex HAVING COUNT(id) > 2; # 按照性别分组，分别查询男女同学平均分,人数 要求：分数低于70分，不参与分组，分组之后，人数要大于2
# WHERE 和 HAVING 的区别？
	1、 WHERE 在分组之前进行限定，如果不满足条件，则不参与分组。 HAVING 分组之后进行限定，如果不满足条件，则不会被查询出来。
	2、 WHERE 后不可以跟聚合函数。 HAVING 可以进行聚合函数的判断。
```

### 4、4分页查询

```sql
# LIMIT  开始的索引，每页查询的条数
SELECT * FROM '表名' LIMIT 0,3; # 每页显示三条，从第一页开始
SELECT * FROM '表名' LIMIT 3,3; # 每页显示三条，从第二页开始
SELECT * FROM '表名' LIMIT 6,3; # 每页显示三条，从第三页开始
# 公式：开始的索引 = (当前的页码 - 1) * 每页显示的条数
# LIMIT 是一个MYSQL"方言" 
```



## 5、约束

### 5、1主键约束 primary key

```sql
# 1、非空且唯一	2、一张表只能有一个字段为主键	 3、主键就是表中记录的唯一标识
# 创建表添加主键约束
CREATE TABLE '表名'(
	'字段1' '数据类型1' PRIMARY KEY,
	'字段2' '数据类型2' 
);
# 删除主键
ALTER TABLE '表名' DROP PRIMARY KEY;
# 创建表后添加主键
ALTER TABLE '表名' MODIFY '字段1' '数据类型1' PRIMARY KEY;
# 自动增长：如果某一列是数值类型的，使用 auto_increment 可以来完成值得自动增长
CREATE TABLE '表名'(
	'字段1' '数据类型1' PRIMARY KEY AUTO_INCREMENT,
	'字段2' '数据类型2' 
);
# 删除自动增长
ALTER TABLE '表名' MODIFY '字段1' '数据类型1';
# 添加自动增长
ALTER TABLE '表名' MODIFY '字段1' '数据类型1' AUTO_INCREMENT;
```



### 5、2非空约束 not null

```sql
# 创建表添加非空约束
CREATE TABLE '表名'(
	'字段1' '数据类型1' NOT NULL,
	'字段2' '数据类型2' NOT NULL 
);
# 删除非空约束
ALTER TABLE '表名' MODIFY '字段' '数据类型';
# 创建表后添加非空约束
ALTER TABLE '表名' MODIFY '字段' '数据类型' NOT NULL;
```

### 5、3唯一约束 unique

```sql
# 创建表添加非空约束
CREATE TABLE '表名'(
	'字段1' '数据类型1' ,
	'字段2' '数据类型2' UNIQUE 
);
# 注意mysql中，唯一约束限定的列的值可以有多个null
# 删除唯一约束(索引)
ALTER TABLE '表名' DROP INDEX '字段2';
# 创建表后添加非空约束
ALTER TABLE '表名' MODIFY '字段' '数据类型' UNIQUE;
```



### 5、4外键约束  foreign key  

```sql
# 创建表添加外键约束
CREATE TABLE '表名'(
	...
	'外键列',
    CONSTRAINT '外键名称' FOREIGN KEY ('外键列名称') REFERENCES '主表名称'('主表主键名') 
);

# 删除外键
ALTER TABLE '表名' DROP FOREIGN KEY '外键名称';
# 创建表之后添加外键
ALTER TABLE '表名' ADD CONSTRAINT '外键名称' FOREIGN KEY ('外键列名称') REFERENCES '主表名称'('主表主键名');

# 级联操作
 ALTER TABLE '表名' ADD CONSTRAINT '外键名称' FOREIGN KEY ('外键列名称') REFERENCES '主表名称'('主表主键名') ON UPDATE CASCADE; # 级联更新
  ALTER TABLE '表名' ADD CONSTRAINT '外键名称' FOREIGN KEY ('外键列名称') REFERENCES '主表名称'('主表主键名') ON DELETE CASCADE; #级联删除

```

## 6、多表查询

### 6、1内链接查询

```sql
1、隐式内链接
SELECT * FROM '表1','表2' WHERE '表1'.'外键' = '表2'.'主键';
2、隐式内链接
SELECT * FROM '表1' [INNER] JOIN '表2' ON '表1'.'外键' = '表2'.'主键'; # INNER 可选

```

### 6、2外链接查询

```sql
1.左外链接
SELECT * FROM '表1' LEFT [OUTER] '表2' JOIN '表1'.'外键' = '表2'.'主键';
# 查询的是左表所有数据及其交集部分
2.右外链接
SELECT * FROM '表1' RIGHT [OUTER] '表2' JOIN '表1'.'外键' = '表2'.'主键';
# 查询的是右表所有数据及其交集部分    
```

### 6、3子查询

```sql
# 查询中嵌套查询
SELECT '字段' FROM '表'; # 假设得出的结果为@
SELECT * FROM '表' WHERE '条件' = @;
-->
SELECT * FROM '表' WHERE '条件' = (SELECT '字段' FROM '表');

```



