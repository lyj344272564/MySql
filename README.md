# MySql

---

## Day01

### 1. DDL操作数据库

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

### 2. DDL_操作表

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

### 3. DML_操作表中的数据（增删改）

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

### 4. DQL_操作表中的数据（查）

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

### 1. DDL_表单操作

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

### 2. 约束

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

### 3. MySql事务

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

## Day03

### 1. 多表的设计

```mysql
-- 创建部门表
-- 一方,主表
CREATE TABLE department(
	 id INT PRIMARY KEY AUTO_INCREMENT,   
	 dep_name VARCHAR(30),	
	 dep_location VARCHAR(30)
);

-- 创建员工表
-- 多方 ,从表
CREATE TABLE employee(
	eid INT PRIMARY KEY AUTO_INCREMENT, 
	ename VARCHAR(20),
	age INT,
	dept_id INT
);

-- 添加2个部门 
INSERT INTO department VALUES(NULL, '研发部','广州'),(NULL, '销售部', '深圳'); 
SELECT * FROM department; 

-- 添加员工,dep_id表示员工所在的部门 
INSERT INTO employee (ename, age, dept_id) VALUES ('张百万', 20, 1); 
INSERT INTO employee (ename, age, dept_id) VALUES ('赵四', 21, 1); 
INSERT INTO employee (ename, age, dept_id) VALUES ('广坤', 20, 1); 
INSERT INTO employee (ename, age, dept_id) VALUES ('小斌', 20, 2); 
INSERT INTO employee (ename, age, dept_id) VALUES ('艳秋', 22, 2); 
INSERT INTO employee (ename, age, dept_id) VALUES ('大玲子', 18, 2); 

SELECT * FROM employee;

-- 插入一条 不存在部门的数据
INSERT INTO employee (ename,age,dept_id) VALUES('无名',35,3);


/*
	外键约束
		作用: 外键约束可以让两张表之间产生有一个对应的关,从而保证了主从表引用的完整性
	
	外键
		外键指的是在从表中与主表的主键对应的字段
		
	主表和从表
		主表 主键id所在的表 ,一的一方
		从表 外键字段所在的表,多的一方
	
	添加外键约束的语法格式
		1.创建表的时候添加外键
		create table 表名(
			字段...
			[constraint] [外键约束名] foreign key(外键字段名) references 主表(主键字段)
		);
	
*/

-- 创建员工表 添加外键
CREATE TABLE employee(
	eid INT PRIMARY KEY AUTO_INCREMENT,
	ename VARCHAR(20),
	age INT,
	dept_id INT, -- 外键字段 指向了主表的主键
	-- 添加外键约束
	CONSTRAINT emp_dept_fk FOREIGN KEY(dept_id) REFERENCES department(id)
);

-- 正常添加数据 (从表外键 对应主表主键)
INSERT INTO employee (ename, age, dept_id) VALUES ('张百万', 20, 1); 
INSERT INTO employee (ename, age, dept_id) VALUES ('赵四', 21, 1); 
INSERT INTO employee (ename, age, dept_id) VALUES ('广坤', 20, 1); 
INSERT INTO employee (ename, age, dept_id) VALUES ('小斌', 20, 2); 
INSERT INTO employee (ename, age, dept_id) VALUES ('艳秋', 22, 2); 
INSERT INTO employee (ename, age, dept_id) VALUES ('大玲子', 18, 2); 

-- 插入一条错误的数据
-- 添加外键约束之后 就会产生一个强制的外键约束检查 保证数据的完整性和一致性
INSERT INTO employee (ename, age, dept_id) VALUES ('错误', 18, 3); 


/*
	删除外键约束
	语法格式
		alter table 从表 drop foreign key 外键约束的名称

*/

-- 删除 employee表中 外键
ALTER TABLE employee DROP FOREIGN KEY emp_dept_fk;

-- 创建表之后添加外键
-- 语法格式 alter table 从表 add CONSTRAINT emp_dept_fk FOREIGN KEY(dept_id) REFERENCES department(id)

-- 简写 不写外键约束名 自动生成的外键约束 employee_ibfk_1
ALTER TABLE employee ADD FOREIGN KEY(dept_id) REFERENCES department(id)

/*
	外键约束的注意事项
		1. 从表的外键类型必须与主表的主键类型一致
		2. 添加数据时,应该先添加主表的数据
		3. 删除数据的时候 要先删除从表中的数据
*/

-- 添加一个新的部门
INSERT INTO department(dep_name,dep_location) VALUES('市场部','北京');

-- 添加一个属于市场部的员工
INSERT INTO employee(ename,age,dept_id) VALUES('老胡',24,3);

/*
	级联删除
		指的是在删除主表的数据的同时,可以删除与之相关的从表中的数据
	
	级联删除
		on delete cascade
*/
-- 重新创建添加级联操作
CREATE TABLE employee(
	eid INT PRIMARY KEY AUTO_INCREMENT,
	ename VARCHAR(20),
	age INT,
	dept_id INT,
	CONSTRAINT emp_dept_fk FOREIGN KEY(dept_id) REFERENCES department(id)
	-- 添加级联删除
	ON DELETE CASCADE
);

-- 添加数据
INSERT INTO employee (ename, age, dept_id) VALUES ('张百万', 20, 1); 
INSERT INTO employee (ename, age, dept_id) VALUES ('赵四', 21, 1); 
INSERT INTO employee (ename, age, dept_id) VALUES ('广坤', 20, 1); 
INSERT INTO employee (ename, age, dept_id) VALUES ('小斌', 20, 2); 
INSERT INTO employee (ename, age, dept_id) VALUES ('艳秋', 22, 2); 
INSERT INTO employee (ename, age, dept_id) VALUES ('大玲子', 18, 2); 

-- 删除部门编号为 2 的数据
DELETE FROM department WHERE id = 2;

/*
	表与表之间的三种关系
		一对多关系(1:n 常见): 班级和学生 部门和员工
		多对多关系(n:n 常见): 学生与课程 演员和角色
		一对一关系(1:1 了解): 身份证 和 人
*/

-- 一对多关系 省表与市表

-- 创建省表 主表 一的一方
CREATE TABLE province(
	id INT PRIMARY KEY AUTO_INCREMENT,
	NAME VARCHAR(20),
	description VARCHAR(20)
);

-- 创建市表 从表 中 外键字段指向 主表的主键
CREATE TABLE city(
	cid INT PRIMARY KEY AUTO_INCREMENT,
	NAME VARCHAR(20),
	description VARCHAR(20),
	
	-- 创建外键 添加外键约束
	pid INT,
	FOREIGN KEY(pid) REFERENCES province(id)
);


-- 多对多关系 演员与角色

-- 演员表
CREATE TABLE actor(
	id INT PRIMARY KEY AUTO_INCREMENT,
	NAME VARCHAR(20)
);

-- 角色表
CREATE TABLE role(
	id INT PRIMARY KEY AUTO_INCREMENT,
	NAME VARCHAR(20)
);

-- 创建中间表
CREATE TABLE actor_role(
	-- 中间表的主键
	id INT PRIMARY KEY AUTO_INCREMENT,
	
	-- aid 字段 指向 actor表的主键
	aid INT,
	
	-- rid 指向 role表的主键
	rid INT
);

-- 添加外键约束

-- aid字段添加外键约束
ALTER TABLE actor_role ADD FOREIGN KEY(aid) REFERENCES actor(id);

-- rid字段添加外键约束
ALTER TABLE actor_role ADD FOREIGN KEY(rid) REFERENCES role(id);
```

### 2. 多表查询

```mysql
/*
	多表查询的语法
		select 字列表段 from 表名列表;

*/

CREATE DATABASE db3_2 CHARACTER SET utf8;

#分类表 (一方 主表)
CREATE TABLE category (
  cid VARCHAR(32) PRIMARY KEY ,
  cname VARCHAR(50)
);

#商品表 (多方 从表)
CREATE TABLE products(
  pid VARCHAR(32) PRIMARY KEY ,
  pname VARCHAR(50),
  price INT,
  flag VARCHAR(2),		#是否上架标记为：1表示上架、0表示下架
  category_id VARCHAR(32),
  -- 添加外键约束
  FOREIGN KEY (category_id) REFERENCES category (cid)
);

#分类数据
INSERT INTO category(cid,cname) VALUES('c001','家电');
INSERT INTO category(cid,cname) VALUES('c002','鞋服');
INSERT INTO category(cid,cname) VALUES('c003','化妆品');
INSERT INTO category(cid,cname) VALUES('c004','汽车');


#商品数据
INSERT INTO products(pid, pname,price,flag,category_id) VALUES('p001','小米电视机',5000,'1','c001');
INSERT INTO products(pid, pname,price,flag,category_id) VALUES('p002','格力空调',3000,'1','c001');
INSERT INTO products(pid, pname,price,flag,category_id) VALUES('p003','美的冰箱',4500,'1','c001');

INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p004','篮球鞋',800,'1','c002');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p005','运动裤',200,'1','c002');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p006','T恤',300,'1','c002');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p007','冲锋衣',2000,'1','c002');

INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p008','神仙水',800,'1','c003');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p009','大宝',200,'1','c003');


/*
	笛卡尔积
	
*/

-- 多表查询 交叉连接查询 的查询结果会产生 笛卡尔积 是不能够使用的.
SELECT * FROM products ,category;

/*
	1.内连接查询
	2.外连接查询
*/

/*
	内连接查询
		特点 通过指定的条件 去匹配俩张表中的内容, 匹配不上的就不显示
		
		隐式内连接
			语法格式: select 字段名... from 左表,右表 where 连接条件
			
		显式内连接
			语法格式: select 字段名... from 左表 [inner] join 右表 on 连接条件
			inner 可以省略
	
*/

-- 1.查询所有商品信息和对应的分类信息
-- 隐式内连接
SELECT * FROM products , category WHERE category_id = cid;

-- 2.查询商品表的商品名称 和 价格,以及商品的分类信息
-- 多表查询中 可以使用给表起别名的方式 简化查询
SELECT  
	p.`pname`,
	p.`price`,
	c.`cname`
FROM products p,category c WHERE p.`category_id` = c.`cid`;

--  查询 格力空调是属于哪一分类下的商品 
SELECT 
	p.`pname`,
	c.`cname`
FROM products p, category c WHERE p.`category_id` = c.`cid` AND p.`pid` = 'p002';

-- 1.查询所有商品信息和对应的分类信息
-- 显式内连接
SELECT 
* 
FROM products p 
INNER JOIN category c ON p.`category_id` = c.`cid`;

-- 2.查询鞋服分类下,价格大于500的商品名称和价格
/*
	查询之前要确定几件事情
		1.查询几张表 products &  category
		2.表的连接条件 p.`category_id` = c.`cid`; 从表.外键 = 主表.主键
		3.查询所用到的字段  商品名称  价格
		4.查询的条件 分类 = 鞋服,  价格 > 500
*/

SELECT 
	p.`pname`,
	p.`price`
FROM products p 
INNER JOIN category c ON p.`category_id` = c.`cid`
WHERE p.`price` > 500 AND c.`cname` = '鞋服';

/*
	外连接查询
		左外连接
			语法格式 关键字 left [outer] join 
				select 字段名 from 左表 left join 右表 on 连接条件
			左外连接的特点
				以左表为基准 匹配右表中的数据 如果能匹配上就显示
				如果匹配不上, 左表中的数据正常显示,右表数据显示为null
		
		右外连接
			语法格式 关键字 right [outer]  join
				select 字段名 from 左表 right join 右表 on 条件
			右外连接的特点
				以右表为基准 匹配左表中的数据 如果能够匹配上 就显示
				如果匹配不到 右表中的数据就正常显示 左表显示null
		
*/

-- 左外连接查询
SELECT 
* 
FROM category c 
LEFT JOIN products p ON c.`cid` = p.`category_id`;

--  查询每个分类下的商品个数
/*
	1.查询的表
	2.查询的条件 分组 统计
	3.查询的字段 分类 分类下商品个数信息
	4.表的连接条件
*/

SELECT 
	c.`cname`,
	COUNT(p.`pid`)
FROM
-- 表连接
category c  LEFT JOIN products p ON c.`cid` = p.`category_id`
-- 分组
GROUP BY c.`cname`;


-- 右外连接查询
SELECT * FROM products p RIGHT JOIN category c ON p.`category_id` = c.`cid`;
```

### 3. 子查询

```mysql
/*
	子查询 subQuery
		一条select语句的结果,作为另外一条select语句的一部分.
	
	子查询的特点
		子查询必须要放在 小括号中
		子查询作为父查询的条件使用(更多的时候)
*/

-- 查询价格最高的商品信息
-- 1.查询出最高的价格
SELECT MAX(price) FROM products ; -- 5000

-- 2.根据最高价格 查出商品信息
SELECT * FROM products WHERE price = 5000;

-- 使用一条SQL完成 子查询方式
SELECT * FROM products WHERE price = (SELECT MAX(price) FROM products );

/*
	子查询分类
		where型子查询: 将子查询的结果 作为父查询的 比较条件使用.
		from型子查询: 将子查询的查询结果作为一张表使用
		exists 型子查询: 查询结果是单列多行的情况,可以将子查询的结果作为父查询的 in函数中的条件使用
*/

-- 子查询作为查询条件

-- 1. 查询化妆品分类下的 商品名称 商品价格
-- 查询出化妆品分类的 id
SELECT cid FROM category WHERE cname = '化妆品'; -- c003

-- 2.根据化妆品id 查询对应商品信息
SELECT 
	p.`pname`,
	p.`price`
FROM products p
WHERE p.`category_id` = (SELECT cid FROM category WHERE cname = '化妆品');

-- 查询小于平均价格的商品信息
-- 1.求出平均价格
SELECT AVG(price) FROM products; -- 1866

-- 2.获取小于平均价格的商品信息
SELECT 
* 
FROM products
WHERE price < (SELECT AVG(price) FROM products);


-- from型子查询方式 

-- 查询商品中,价格大于500的商品信息,包括 商品名称 商品价格 商品所属分类名称
SELECT * FROM category;

SELECT  
	p.`pname`,
	p.`price`,
	c.cname
FROM products p 
-- 注意 子查询的结果作为一张表时,要起一个别名 否则无法访问表中的字段
INNER JOIN (SELECT * FROM category) c ON p.`category_id` = c.cid 
WHERE p.`price` > 500;

/*
	子查询的结果是单列多行, 作为父查询的 in 函数中的条件使用
	语法格式
		select 字段名 from 表名 where 字段 in(子查询);
*/
-- 查询价格小于两千的商品,来自于哪些分类(名称)

-- 1.查询小于两千的商品的 分类id
SELECT DISTINCT category_id FROM products WHERE price < 2000;

-- 2.根据分类的id 查询 分类的信息
SELECT * FROM category 
WHERE cid IN 
(SELECT DISTINCT category_id FROM products WHERE price < 2000);


-- 查询家电类 与 鞋服类下面的全部商品信息
-- 1.首先要获取 家电类和鞋服类 分类id
SELECT cid FROM category WHERE cname IN('家电','鞋服');

-- 2.根据 分类id 查找商品信息
SELECT 
	* 
FROM products WHERE category_id IN
(SELECT cid FROM category WHERE cname IN('家电','鞋服'));

-- 子查询的总结
-- 1.子查询如果是一个字段(单列) ,那么就在where后面做条件
-- 2.如果是多个字段(多列) 就当做一张表使用 (要起别名)

/*
	数据库三范式
		三范式指的就是数据库设计的一个规则.
		作用 就是为了创建 冗余较小 结构合理的数据库
		范式 就是设计数据库的要求(规范)
	
	第一范式(1NF)  满足最低要求的范式
	第二范式(2NF)	在满足第一范式的基础之上 进一步满足更多的规范
	第三范式(3NF)	以此类推
*/

/**
 * 
 * 1NF: 列具有原子性，设计列要做到列不可拆分
 * 
 * 2NF: 一张表只能描述一件事情
 * 
 * 3NF: 消除传递依赖，表中的信息如果能够推导出来，就不要设计一个字段来单独记录(空间最省原则)
 * 
 * */
 
/*
	反三范式
		指的是通过增加冗余或者重复数据 来提高数据库的读性能
		浪费存储空间 节省查询时间(以空间换时间)
	
	冗余字段
		某一个字段 属于一张表,但是 他又在多张表中都有出现
*/
```



