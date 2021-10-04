# MySql

---

## Day01

### DDL操作数据库

````mysql
-- 查询所有的数据库列表
SHOW DATABASES;

/*
	对数据库操作的分类包含:
		CRUD
		C create 创建
		R retrieve 查询
		U update 修改
		D delete 删除
		使用数据库
*/

/*
	创建数据库 方式1: 指定名称的数据库
	latin1 编码
*/
CREATE DATABASE db1;

/*
	指定字符集的方式创建数据库
	utf8
*/
CREATE DATABASE db1_1 CHARACTER SET utf8;

/*
	查看数据库
*/
-- 切换数据库
USE db1_1;

-- 查询当前正在使用的数据库
SELECT DATABASE();

-- 查询MySql中有哪些数据库
SHOW DATABASES;

-- 修改数据库的字符集
-- 语法格式 alter database 数据库名 character set utf8;
ALTER DATABASE db1 CHARACTER SET utf8;

-- 查询当前数据库的基本信息
SHOW CREATE DATABASE db1;


-- 删除数据库
-- 语法格式 drop database 数据库名称 将数据库从MySql中永久删除
DROP DATABASE db1_1;   -- 慎用 


````

### DDL_操作表

```mysql
/*
	创建表的语法格式
	create table 表名(
	   字段名称1 字段类型(长度),
	   字段名称2 字段类型,
	   字段名称3 字段类型 最后一个列不要添加逗号
	);
	
	MySql中常见的数据类型
		int 整型
		double 浮点型
		varchar 字符串
		date 日期类型 年月日 没有时分秒 yyyy-MM-dd
		datetime 日期时间类型 yyyy-MM-dd HH:mm:ss
		
*/

-- 创建商品分类表
/*
	表名 category
		cid int 分类id
		cname varchar 分类的名称
*/

-- 选择要使用的数据库
USE db1;

-- 创建分类表
CREATE TABLE category(
	cid INT,
	cname VARCHAR(20)
);

-- 创建测试表
/*
	表名 test1
		tid int 
		tdate date
*/
CREATE TABLE test1(
	tid INT,
	tdate DATE
);

-- 快速创建一个表结构相同的表（复制表结构）
-- 语法结构 create table 新表名称 like 旧表名称

-- 创建一个与test1表结构相同的 test2表
CREATE TABLE test2 LIKE test1;

-- 查看表结构
DESC test1;

-- 查看表
-- 查看当期那数据库中所有的数据表名
SHOW TABLES;

-- 查看创建表的 sql
SHOW CREATE TABLE category;

-- 查看表结构
DESC category;


-- 表的删除

-- 方式1: 将数据库中的某一张表永久删除
-- 语法格式: drop table 表名
DROP TABLE test1;

-- 方式2: 判断表是否存在, 如果存在就删除 如果不存在就不执行删除
DROP TABLE IF EXISTS test2;


/*
	修改表的名称
	修改表的字符集
	修改表中的某一列 (数据类型 名称 长度)
	向表中添加一列
	删除表中的某一列
*/

-- 修改表名称 语法格式: rename table 旧表名 to 新表名
RENAME TABLE category TO category1;

-- 修改表的字符集为 gbk 
-- 语法格式: alter table 表名 character set 字符集
ALTER TABLE category1 CHARACTER SET gbk; 

-- 向表中添加一个字段 关键字: add
-- 语法格式: alter table 表名 add 字段名称 字段类型(长度)
-- 添加分类描述字段
ALTER TABLE category1 ADD cdesc VARCHAR(20);


-- 修改表中列的类型或者长度 关键字 modify
-- 语法格式: alter table 表名 modify 字段名称 字段类型
-- 修改cdesc 字段的长度为 50
ALTER TABLE category1 MODIFY cdesc VARCHAR(50); -- 修改字段长度
ALTER TABLE category1 MODIFY cdesc CHAR(20); -- 修改字段类型

-- 修改列的名称 关键字: change
-- 语法格式: alter table 表名 change 旧列名 新列名 类型(长度)
-- 修改cdesc字段 名称改为 description varchar(30)
ALTER TABLE category1 CHANGE cdesc description VARCHAR(30);


-- 删除列 关键 drop
-- 语法格式: alter table 表名 drop 列名
ALTER TABLE category1 DROP description;
```

### DML_操作表中的数据（增删改）

```mysql
/*
	DML 对表中的数据进行 增删改
	增加
		语法格式: insert into 表名 (字段名1,字段名2...) values(字段值1,字段值2...)
*/
-- 创建学生表
CREATE TABLE student(
	sid INT,
	sname VARCHAR(20),
	age INT,
	sex CHAR(1),
	address VARCHAR(40)
);

-- 向学生表中插入数据

-- 方式1 插入全部字段 将所有字段名都写出来
INSERT INTO student (sid,sname,age,sex,address) VALUES(1,'孙悟空',18,'男','花果山');

-- 方式2 插入全部字段 不写字段名
INSERT INTO student VALUES(2,'孙悟饭',5,'男','地球');

-- 方式3 插入指定字段
INSERT INTO student (sid,sname) VALUES(3,'蜘蛛精');

-- 注意事项
	-- 1.值与字段必须对应 个数&数据类型&长度 都必须一致
	INSERT INTO student (sid,sname,age,sex,address) VALUES(4,'孙悟空',18,'男','花果山');

	-- 2.在插入 varchar char date 类型的时候,必须要使用 单引号 或者双引号进行包裹
	INSERT INTO student (sid,sname,age,sex,address) VALUES(4,'孙悟空',18,'男','花果山');

	-- 3.如果插入空值 可以忽略不写 或者写 null
	INSERT INTO student (sid,sname) VALUES(5,'唐僧');
	INSERT INTO student (sid,sname,age,sex,address) VALUES(6,'八戒',NULL,NULL,NULL);
	

/*
	修改操作
		语法格式1: update 表名 set 列名 = 值
		语法格式2: update 表名 set 列名 = 值 [where 条件表达式: 字段名 = 值]
*/

-- 修改表中的所有的学生性别为女
UPDATE student SET sex = '女';  -- (慎用!!)

-- 带条件的修改 将sid为 1的数据,性别改为男
UPDATE student SET sex = '男' WHERE sid = 1;

-- 一次性修改多个列
-- 修改sid 为 5的这条数据, 年龄改为20, 地址改为 大唐
UPDATE student SET age = 20 , address = '大唐' WHERE sid = 5;


/*
	删除
		语法格式1: delete from 表名;
		语法格式2: delete from 表名 [where 条件];
*/

-- 删除 sid为 6的数据
DELETE FROM student WHERE sid = 6;

-- 删除所有数据
DELETE FROM student;


-- 删除所有数据的方式 两种
	-- 1. delete from 表; 不推荐, 对表中的数据逐条删除. 效率低
	-- 2. truncate table 表; 推荐, 删除整张表, 然后再创建一个一模一样的新表.

INSERT INTO student VALUES(1,'孙悟空',20,'男','花果山');
TRUNCATE TABLE student;
```

### DQL_操作表中的数据（查）

```mysql
/*
	DQL 
		简单查询
			select 列名 from 表名;
*/

-- 查询emp 表中的所有数据
SELECT * FROM emp; -- * 表示所有的列

-- 查询所有数据 只显示 id 和 name
SELECT eid, ename FROM emp;

-- 查询所有的数据,然后给列名 改为中文
SELECT * FROM emp;

-- 别名查询 使用关键字 as

SELECT 
	eid AS '编号',
	ename AS '姓名',
	sex AS '性别',
	salary AS '薪资',
	hire_date AS '入职时间',
	dept_name '部门名称'  -- as 可以省略

FROM emp;

-- 查询一共有几个部门
SELECT dept_name FROM emp;

-- 去重操作 关键字 distinct
SELECT DISTINCT dept_name FROM emp;

-- 将我们的员工薪资数据 +1000 进行展示
SELECT ename, salary+1000 AS salary FROM emp;

-- 注意: 查询操作 不会对数据表中的数据进行修改,只是一种显示的方式.

/*
	条件查询
	语法格式: select 列名 from 表名 where 条件表达式
	
	比较运算符
		>  <  <=   >=   =  <> !=
		BETWEEN  ...AND...
		IN(集合)
		LIKE
		IS NULL
		
	逻辑运算符
		And
		Or
		Not
*/
# 查询员工姓名为黄蓉的员工信息
-- 1.查哪张表	2.查哪些字段	3.查询条件
SELECT * FROM emp WHERE ename = '黄蓉'


# 查询薪水价格为5000的员工信息
SELECT * FROM emp WHERE salary = 5000;


# 查询薪水价格不是5000的所有员工信息
SELECT * FROM emp WHERE salary != 5000;
SELECT * FROM emp WHERE salary <> 5000;

# 查询薪水价格大于6000元的所有员工信息
SELECT * FROM emp WHERE salary > 6000;

# 查询薪水价格在5000到10000之间所有员工信息
SELECT * FROM emp WHERE salary BETWEEN 5000 AND 10000;
SELECT * FROM emp WHERE salary >= 5000 AND salary  <= 10000;

# 查询薪水价格是3600或7200或者20000的所有员工信息
SELECT * FROM emp WHERE salary = 3600 OR salary = 7200 OR salary = 20000;
-- 方式2 使用 in() 匹配括号中的参数
SELECT * FROM emp WHERE salary IN(3600,7200,20000);

/*
	like '_精'
		% 通配符 ,表示匹配任意多个字符串
		_ 通配符 ,表示匹配一个字符
*/

# 查询含有'精'字的所有员工信息
SELECT * FROM emp WHERE ename LIKE '%精%';

# 查询以'孙'开头的所有员工信息
SELECT * FROM emp WHERE ename LIKE '孙%';

# 查询第二个字为'兔'的所有员工信息
SELECT * FROM emp WHERE ename LIKE '_兔%';

# 查询没有部门的员工信息
-- select * from emp where dept_name = null; 错误方式
SELECT * FROM emp WHERE dept_name IS NULL;

# 查询有部门的员工信息
SELECT * FROM emp WHERE dept_name IS NOT NULL;

-- 条件查询 先取出表中的每条数据,满足条件的就返回,不满足的就过滤.
```

## Day02

### DDL_表单操作

```mysql
/*
	排序 
	使用 order by子句
	语法结构: 	select 字段名 from 表名 [where 字段名 = 值] order by 字段名称 [ASC/DESC]
		ASC 升序排序 (默认升序)
		DESC 降序排序
*/

-- 单列排序 按照某一个字段进行排序
-- 使用salary 字段 对emp表进行排序
SELECT * FROM emp ORDER BY salary; -- 默认升序
SELECT * FROM emp ORDER BY salary DESC; -- 降序排序

-- 组合排序 同时对多个字段进行排序
-- 在薪资的排序基础上,再去使用 id字段进行排序
SELECT * FROM emp ORDER BY salary DESC ,eid DESC;

-- 组合排序的特点: 如果第一个字段 值相同,就按照第二个字段进行排序.

/*
	聚合函数
		作用:将一列数据作为一个整体,进行纵向的计算的
	
	常用的聚合函数
		count(字段) 统计记录数
		sum(字段) 求和操作
		max(字段) 求最大值
		min(字段) 求最小值
		avg(字段) 求平均值
	
	语法格式
		select 聚合函数(字段名) from 表名 [where 条件]
*/

#1 查询员工的总数
SELECT COUNT(*) FROM emp; 
SELECT COUNT(1) FROM emp;
SELECT COUNT(eid) FROM emp;

-- count函数 在统计的时候回忽略空值
-- 注意 不要使用带空值的列 进行 count
SELECT COUNT(dept_name) FROM emp;

#2 查看员工总薪水、最高薪水、最小薪水、薪水的平均值
SELECT 
	SUM(salary) AS '总薪水',
	MAX(salary) '最高薪水',
	MIN(salary) '最小薪水',
	AVG(salary) '平均薪水'
FROM emp

#3 查询薪水大于4000员工的个数
SELECT COUNT(*) FROM emp WHERE salary > 4000;

#4 查询部门为'教学部'的所有员工的个数
SELECT COUNT(*) FROM emp WHERE dept_name = '教学部';

#5 查询部门为'市场部'所有员工的平均薪水
SELECT AVG(salary) FROM emp WHERE dept_name = '市场部';



/*
	分组查询 使用 group by 子句
	
	语法格式: select 分组字段/聚合函数 from 表名 group by 分组字段
*/

SELECT * FROM emp GROUP BY sex;

# 通过性别字段 进行分组,求各组的平均薪资
SELECT sex, AVG(salary) FROM emp GROUP BY sex;

#1.查询所有部门信息 
SELECT dept_name AS '部门名称' FROM emp GROUP BY dept_name;

#2.查询每个部门的平均薪资
SELECT dept_name,AVG(salary) FROM emp GROUP BY dept_name;

#3.查询每个部门的平均薪资, 部门名称不能为null
SELECT 
	dept_name AS '部门名称',
	AVG(salary) AS '部门平均薪资'
FROM emp 
WHERE dept_name IS NOT NULL 
GROUP BY dept_name;

# 查询平均薪资大于6000的部门.
-- 1. 首先要分组求平均薪资
-- 2. 求出 平均薪资大于 6000的部门

-- 在分组之后 进行条件过滤 使用:  having 判断条件

SELECT 
	dept_name,
	AVG(salary)
FROM emp 
WHERE dept_name IS NOT NULL GROUP BY dept_name 
HAVING	AVG(salary) > 6000;

/*
	where 与 having的区别
		where 
			1.在分组前进行过滤
			2.where后面不能跟 聚合函数
		having
			1.是在分组后进行条件过滤
			2.having 后面可以写 聚合函数

*/

/*
	limit 通过limit 去指定要查询的数据的条数 行数
	
	语法格式
		select 字段 from 表名 limit offset, length;
	参数说明:
		offset: 起始行数 默认从0 开始计数
		length: 返回的行数 (要查询几条数据)
*/

# 查询emp表中的前 5条数据
SELECT * FROM emp LIMIT 0,5;
SELECT * FROM emp LIMIT 5;

# 查询emp表中 从第4条开始,查询6条
SELECT * FROM emp LIMIT 3 , 6;

-- limit 分页操作, 每页显示3条
SELECT * FROM emp LIMIT 0,3; -- 第一页
SELECT * FROM emp LIMIT 3,3; -- 第二页
SELECT * FROM emp LIMIT 6,3; -- 第三页 3-1=2 2*3=6

-- 分页公式 起始行数 = (当前页码 - 1) * 每页显示条数
```

### 约束

```mysql
/*
	约束
		约束是指对数据进行一定的限制,来保证数据的完整性 有效性 正确性
		
	常见的约束
		主键约束	primary key 
		唯一约束	unique
		非空约束	not null
		外键约束	foreign key
*/

/*
	主键约束
		特点	不可重复 唯一 非空
		作用	用来表示数据库中的每一条记录
		
	语法格式
		字段名	字段类型 primary key
*/

-- 方式1 创建一个带有主键的表
CREATE TABLE emp2(
	eid INT PRIMARY KEY,
	ename VARCHAR(20),
	sex CHAR(1)
);

DESC emp2;

-- 方式2 创建
DROP TABLE emp2; -- 删除表

CREATE TABLE emp2(
	eid INT,
	ename VARCHAR(20),
	sex CHAR(1),
	PRIMARY KEY(eid) -- 指定eid为主键
);

-- 方式3 创建表之后 再添加主键
CREATE TABLE emp2(
	eid INT,
	ename VARCHAR(20),
	sex CHAR(1)
);

-- 通过DDL语句 添加主键约束
ALTER TABLE emp2 ADD PRIMARY KEY(eid);


-- 删除主键 DDL语句
ALTER TABLE emp2 DROP PRIMARY KEY;

/*
	主键的自增
		关键字:  auto_increment 主键的自动增长(字段类型必须是 整数类型)
*/

-- 创建主键自增的表
CREATE TABLE emp2(
	-- 主键自增
	eid INT PRIMARY KEY AUTO_INCREMENT,
	ename VARCHAR(20),
	sex CHAR(1)
);
-- 添加数据 观察主键变化
INSERT INTO emp2(ename,sex) VALUES('张三','男');
INSERT INTO emp2(ename,sex) VALUES('李四','男');
INSERT INTO emp2 VALUES(NULL,'翠花','女');
INSERT INTO emp2 VALUES(NULL,'艳秋','女');

-- 修改自增起始值 

-- 重新创建自增主键的表, 自定义自增的其实位置
CREATE TABLE emp2(
	eid INT PRIMARY KEY AUTO_INCREMENT,
	ename VARCHAR(20),
	sex CHAR(1)
)AUTO_INCREMENT=100;

/*
	DELETE和TRUNCATE对自增长的影响
	
	delete 删除表中所有数据, 将表中的数据逐条删除.
	truncate 删除表中的所有数据, 是将整个表删除,然后再创建一个结构相同表.
*/

-- delete 方式删除所有数据 
DELETE FROM emp2; -- delete 删除对自增是没有影响

INSERT INTO emp2(ename,sex) VALUES('张百万','男'); -- 102
INSERT INTO emp2(ename,sex) VALUES('艳秋','女'); -- 103


-- truncate 删除所有数据
TRUNCATE TABLE emp2; -- 自增从1开始

INSERT INTO emp2(ename,sex) VALUES('张百万','男'); -- 1
INSERT INTO emp2(ename,sex) VALUES('艳秋','女'); -- 2


/*
	非空约束
		特点 表中某一列不予许为空
		
	语法格式
		字段名 字段类型 not null
*/

CREATE TABLE emp2(
	eid INT PRIMARY KEY AUTO_INCREMENT,
	-- 将 ename字段 添加了 非空约束
	ename VARCHAR(20) NOT NULL,
	sex CHAR(1)
);

/*
	唯一约束
		特点: 表中的某一列不能够重复 (对null值 不做唯一判断)

	语法格式
		字段名 字段类型 unique
*/
-- 创建 emp3表 为ename 字段添加 唯一约束
CREATE TABLE emp3(
	eid INT PRIMARY KEY ,
	ename VARCHAR(20) UNIQUE,
	sex CHAR(1)
);

-- 测试唯一约束
INSERT INTO emp3 VALUES(1,'张百万','女');

-- Duplicate entry '张百万' for key 'ename' 不能重复
INSERT INTO emp3 VALUES(2,'张百万','女');

-- 唯一约束的值 可以为 null
INSERT INTO emp3 VALUES(2,NULL,'女');


-- 主键约束和唯一约束的区别
	-- 1.主键约束 他是唯一且不能为空的
	-- 2.唯一约束 唯一,但是可以为空
	-- 3.一个表中只能有一个主键,但是可以有多个唯一约束

-- 外键约束 多表中再学习

/*
	默认值
		特点 用来指定 某一列的默认值
		
	语法格式
		字段名 字段类型 default 默认值
*/
--  创建 emp4表, 指定 sex默认为 女
CREATE TABLE emp4(
	eid INT PRIMARY KEY,
	ename VARCHAR(20),
	sex CHAR(1) DEFAULT '女'
);

INSERT INTO emp4(eid,ename) VALUES(1,'杨幂');
INSERT INTO emp4(eid,ename) VALUES(2,'柳岩');

-- 不使用默认值
INSERT INTO emp4(eid,ename,sex) VALUES(3,'蔡徐坤','男');
```

### MySql事务

```mysql
-- 创建账户表
CREATE TABLE account(
	-- 主键
	id INT PRIMARY KEY AUTO_INCREMENT,
	-- 姓名
	NAME VARCHAR(10),
	-- 余额
	money DOUBLE
);

-- 添加两个用户
INSERT INTO account (NAME, money) VALUES ('tom', 1000), ('jack', 1000);

-- tom账户 -500元
UPDATE account SET money = money - 500 WHERE NAME = 'tom';

出错了

-- jack账户 + 500元
UPDATE account SET money = money + 500 WHERE NAME = 'jack';

/*
	MySql事务操作
		手动提交事务
			1.开启事务 start transaction; 或者 begin;
			2.提交事务 commit;
			3.回滚事务 rollback;
			
		自动提交事务
			MySql默认的提交方式 自动提交事务
			每执行一条DML语句 都是一个单独的事务
*/
-- 手动提交事务演示

-- 成功案例
USE db2;

start transaction;  -- 开启事务

update account set money = money - 500 where name = 'tom';

update account set money = money + 500 where name = 'jack';

commit;	-- 提交事务

-- 失败案例 会进行回滚
start transaction; -- 开启事务

INSERT INTO account VALUES(NULL,'张百万',3000);
INSERT INTO account VALUES(NULL,'有财',3500);


-- 直接关闭窗口 模拟系统崩溃 数据没有发生改变

-- 如果事务中 SQL 语句没有问题，commit 提交事务，会对数据库数据的数据进行改变。 如果事务中 SQL 语句有问题，rollback 回滚事务，会回退到开启事务时的状态。 

-- 自动提交事务 MySQL 默认每一条 DML(增删改)语句都是一个单独的事务

--  登录mysql，查看autocommit状态。
SHOW VARIABLES LIKE 'autocommit';

-- 把 autocommit 改成 off;
SET @@autocommit=off;

-- 执行修改操作

-- 选择数据库
use db2;

-- 修改数据
update account set money = money - 500 where name = 'jack';

-- 手动提交
commit;

/*
	事务的四大特性(面试要问)
		原子性
			每个事务都是一个整体,不可以再拆分,事务中的所有SQL
			要么都执行成功 要么都执行失败
			
		一致性
			事务在执行之前 数据库的状态,与事务执行之后的状态要保持一致
		
		隔离性
			事务与事务之间不应该相互影响,执行时要保证隔离状态
		
		持久性
			一旦事务执行成功,对数据的修改是持久的.	
*/

/*
	MySql的隔离级别
			各个事务之间是隔离,相互独立.但是如果多个事务对数据库中的同一批数据
		进行并发访问的时候,就会引发一些问题,可以通过设置不同的隔离级别来解决
		对应的问题
	
	并发访问的问题
		脏读: 一个事务读取到了另一个事务没有提交的数据.
		不可重复读:	一个事务中 两次读取的数据不一致.
		幻读: 一个事务中,一次查询的结果,无法支撑后续的业务操作.
	
	设置隔离级别
		read uncommitted: 读未提交
			可以防止哪些问题: 无
			
		read committed: 读已提交 (Oracle默认 隔离级别)
			可以防止: 脏读
		
		repeatable read : 可重复读	(MySql默认的隔离级别)
			可以防止: 脏读 ,不可重复读
			
		serializable : 串行化
			可以防止: 脏读 ,不可重复读 ,幻读
			
	注意: 隔离级别 从小到大 安全性是越来越高的,但是效率是越来越低的,
	根据不同的情况选择对应的隔离级别
	
*/


/*
	查看隔离级别
	select @@tx_isolation;
	
	设置隔离级别
	set global transaction isolation level 级别名称;
	read uncommitted 读未提交
	read committed   读已提交
	repeatable read  可重复读
	serializable     串行化

*/
-- 查看隔离级别	MySql默认隔离级别  repeatable read
select @@tx_isolation;

-- 设置隔离级别为 读已提交                        
set global transaction isolation level read committed ;
```



