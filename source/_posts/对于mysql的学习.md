---
title: 对于mysql的学习
date: 2022-03-13 08:52:21
tags: mysql 学习 数据库
categories: mysql
---

<!--more-->

mysql学习告一段落，我对mysql基础部分做了总结

**目录**

[1.基本命令操作](#1.%E5%9F%BA%E6%9C%AC%E5%91%BD%E4%BB%A4%E6%93%8D%E4%BD%9C)

[2.mysql的四种语言](#2.mysql%E7%9A%84%E5%9B%9B%E7%A7%8D%E8%AF%AD%E8%A8%80)

[3.数据库的字段属性\(一部分\)](<#3.数据库的字段属性(一部分)>)

[4.如何创建一个数据库表](#4.%E5%A6%82%E4%BD%95%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AA%E6%95%B0%E6%8D%AE%E5%BA%93%E8%A1%A8)

[5.修改和删除数据表字段](#5.%E4%BF%AE%E6%94%B9%E5%92%8C%E5%88%A0%E9%99%A4%E6%95%B0%E6%8D%AE%E8%A1%A8%E5%AD%97%E6%AE%B5)

[6.外键](#6.%E5%A4%96%E9%94%AE)

[7.DML语言](#7.DML%E8%AF%AD%E8%A8%80)

[8.DQL查询数据（重点）](#8.DQL%E6%9F%A5%E8%AF%A2%E6%95%B0%E6%8D%AE%EF%BC%88%E9%87%8D%E7%82%B9%EF%BC%89)

[9.去重操作和其他用法](#9.%E5%8E%BB%E9%87%8D%E6%93%8D%E4%BD%9C%E5%92%8C%E5%85%B6%E4%BB%96%E7%94%A8%E6%B3%95)

[10.模糊查询](#10.%E6%A8%A1%E7%B3%8A%E6%9F%A5%E8%AF%A2)

[11.联表查询](#11.%E8%81%94%E8%A1%A8%E6%9F%A5%E8%AF%A2)

[12.自连接](#12.%E8%87%AA%E8%BF%9E%E6%8E%A5)

[13\. 分页和排序](<#13. 分页和排序>)

[ 14.子查询和嵌套查询](#%C2%A014.%E5%AD%90%E6%9F%A5%E8%AF%A2%E5%92%8C%E5%B5%8C%E5%A5%97%E6%9F%A5%E8%AF%A2)

[ 15.常用函数](#%C2%A015.%E5%B8%B8%E7%94%A8%E5%87%BD%E6%95%B0)

[16.聚合函数和分组过滤](#16.%E8%81%9A%E5%90%88%E5%87%BD%E6%95%B0%E5%92%8C%E5%88%86%E7%BB%84%E8%BF%87%E6%BB%A4)

[ 17.操作复习（要记住，）](#%C2%A017.%E6%93%8D%E4%BD%9C%E5%A4%8D%E4%B9%A0%EF%BC%88%E8%A6%81%E8%AE%B0%E4%BD%8F%EF%BC%8C%EF%BC%89)

[18.ACID](#18.ACID)

[ 19.事务](#%C2%A019.%E4%BA%8B%E5%8A%A1)

[ 20.数据库三大范式](#%C2%A020.%E6%95%B0%E6%8D%AE%E5%BA%93%E4%B8%89%E5%A4%A7%E8%8C%83%E5%BC%8F)

[ 21.第一个JDBC程序](#%C2%A021.%E7%AC%AC%E4%B8%80%E4%B8%AAJDBC%E7%A8%8B%E5%BA%8F)

[ 22.列的数据类型](#%C2%A022.%E5%88%97%E7%9A%84%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B)

[23.索引](#23.%E7%B4%A2%E5%BC%95)

[ 24.数据库用户管理](#%C2%A024.%E6%95%B0%E6%8D%AE%E5%BA%93%E7%94%A8%E6%88%B7%E7%AE%A1%E7%90%86)

[25.参考视频：【狂神说Java】MySQL最新教程通俗易懂\_哔哩哔哩\_bilibili](#25.%E5%8F%82%E8%80%83%E8%A7%86%E9%A2%91%EF%BC%9A%E3%80%90%E7%8B%82%E7%A5%9E%E8%AF%B4Java%E3%80%91MySQL%E6%9C%80%E6%96%B0%E6%95%99%E7%A8%8B%E9%80%9A%E4%BF%97%E6%98%93%E6%87%82_%E5%93%94%E5%93%A9%E5%93%94%E5%93%A9_bilibili)

---

### **1.基本命令操作**

- 都使用net start mysql   启动数据库服务

- mysql \-uroot \-p123456    连接数据库

- flush privileges；刷新权限

- 所有的语句都使用 ；结尾

- show databases；查看所有的数据库

- mysql>use school； 切换数据库

- database changed

- show tables； 查看数据库中的所有表

- describe student；显示数据库所有表的信息

- create database westons；创建一个数据库

- exit；退出连接

- \-- 单行注释

- /\*    \*/ 多行注释

### 2.mysql的四种语言

DDL 定义语言

DML 操作语言

DQL 查询语言

DCL 控制语言

### 3.数据库的字段属性\(一部分\)

unsigned 无符号的整数  不能声明为负数

zerofill     零填充

自增         自动在上一次记录的基础上加一

非空         必须赋值

默认    设置默认的值

### 4.如何创建一个数据库表

```sql
CREATE TABLE IF NOT EXISTS `表名`(
    `id` int(4) not null auto_increment comment '学号',
    `name`  varchar(30) not null  default '匿名' comment '姓名',
    `birthday` datetime default null comment '出生日期',
PRIMARY KEY (`id`)
)ENGINE =INNODB DEFAULT CHARSET=utf8
```

### 5.修改和删除数据表字段

```sql
修改表名
ALTER TABLE teacher RENAME AS teacher1
增加表的字段
ALTER TABLE teacher1 ADD age INT(11)
修改表的字段
ALTER TABLE teacher1 MODIFY age VARCHAR(11)   -- 修改约束
ALTER TABLE teacher1 CHANGE age age1 INT(1) -- 字段重命名


删除表的字段
ALTER TABLE teacher1 DROP age1

删除
--删除表（如果表存在再删除）
DROP TABLE IF EXISTS teacher1
```

### 6.外键

```sql
给一个表添加一个外键

ALTER TABLE `student`
ADD CONSTRAINT `FK_gradeid` FOREIGN KEY(` gradeid `)  REFERENCES ` grade `( ` gradeid`);

最佳实践

数据库就是单纯的表，只用来存数据，只有行（数据）和列（字段）

我们相使用多张表的数据，想使用外键 （程序去实现）
```

### 7.DML语言

数据库的意义：存储数据，数据管理

- insert

- update

- delete

---

\--插如语句（添加）

insert into 表名 （\[字段1，字段2，字段3\]）values（'值1','值2','值3' ...）

一般插如语句，我们一定要数据和字段一一对应 例：

INSERT INTO \`student\`\(\` name \`, \`pwd \`,\`sex\`\)

VALUES \('李四','aaaaaa',\`男\`）， \('李三,'aa11aa',\`男\`）

字段和字段之间使用英文逗号

字段可以省略，但插入的值要和字段一一对应

修改语句

update 表名  set colnum\_ name = value where  条件

colnum\_ name是数据库的列，带上\`   \`

条件：筛选的条件，如果没有指定，则会修改所有的列

value 可以是具体的值也可以是一个变量

删除  delete

delete from 表名 \[where 条件\]

delete from student 清空全部数据

delete from \`student\`  WHERE id =1    

删除指定数据

TRUNCATE 命令

完全清除一个数据库表，表的结构和索引约束不会变

TRUNCATE \` student\`

delete 和  truncate 的区别

相同的：都能删除数据，都不会删除表结构

不同的：

- 删除侯 值增量可能不一样

### 8.DQL查询数据（重点）

- 所有的查询操作都用它：SELECT

- 简单的查询，复杂的查询都可以做

- 数据库中核心的语言，最重要的语句

- 使用频率最高的语句

指定查询字段

SELECT \* FROM student   \-->查询全部的学生

SELECT \* FROM result     \--.>查询全部结果

查询指定字段

SELECT \`studentno\`,\`studentname\` FROM student

 \--别名    ， 给结果起一个名字   AS

SELECT \`studentno\` AS  学号，\`studentname\` AS  学生姓名 FROM student  AS   stu

\---函数    CONCAT\(a,b）

SELECT CONCAT\('姓名',StudentName  AS  新名字   FROM  student

### 9.去重操作和其他用法

去重 distinct

select distinct 'studentno'  from  result

查询有哪些同学参加了考试

发现重复数据，去重

\--查询系统版本

select version（）

\--用来计算

select 100\*3-1 as 计算结果

### 10.模糊查询

![](https://img-blog.csdnimg.cn/c3e948f00407441cb041a399dd0f5e29.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 上，下方的图片是我从狂神的视频里截的   狂神讲的mysql还是不错的

### 11.联表查询

![](https://img-blog.csdnimg.cn/2271174b98964a54836195de4f0c0933.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 

### 12.自连接

自己的表和自己的表连接

核心：一张表拆为两张表

![](https://img-blog.csdnimg.cn/dc9d988f77f1497297a3270c54ca0e97.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 ![](https://img-blog.csdnimg.cn/d9e1b599a4ef42ccb60ef11694740261.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 

![](https://img-blog.csdnimg.cn/6505d6e5279a4286ab37819b301abd51.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

### 13\. 分页和排序

![](https://img-blog.csdnimg.cn/92d225bfa65042939571e490a3522656.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

###  14.子查询和嵌套查询

![](https://img-blog.csdnimg.cn/ca3c64fae89c461fb45415da7c67eebc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

###  15.常用函数

ABS 绝对值

CEILING 向上取整

FLOOR   向下取整

RAND     随机数

SIGN       判断一个数的符号

CHAR\_LENGTH   字符串长度

CONCAT  拼接字符串

INSERT      替换

LOWER  小写字母

UPPER   大写字母

### 16.聚合函数和分组过滤

![](https://img-blog.csdnimg.cn/e2416459b66e4d959595b3479ad07b52.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 ![](https://img-blog.csdnimg.cn/e0cb7b62e4a147d1b02b9bee5b9cc42c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

###  17.操作复习（要记住，）

```sql
CREATE DATABASE StudentScore  -- 创建一个数据库
-- 创建表
-- 表名（字段名，数据类型，是否设为主键，非空（完整性约束）
CREATE TABLE Student(sNo CHAR(9) NOT NULL PRIMARY KEY,sName CHAR(12) NOT NULL ,sex CHAR(2),age INT,dept CHAR(50));
CREATE TABLE Course(cNo CHAR(9) NOT NULL PRIMARY KEY, cName CHAR(30) NOT NULL,credit INT);
CREATE TABLE Score(sNo CHAR(9) NOT NULL,cNo CHAR(6) NOT NULL,grade FLOAT,PRIMARY KEY(sNo,cNo));
DROP TABLE Student -- 删除表
-- 修改表的结构
-- alter table <表名>[alter column<字段名><数据类型>]   修改表中已有字段的定义
-- [add column<字段名><数据类型><字段级完整性约束>]     增加新字段以及相应的完整性约束条件
-- [drop column<字段名>] |  [drop constraint<完整性约束>]
-- 删除该子句中给出的字段    删除指定的完整性约束条件


-- 利用sql语句添加一个字符型的电话字段phone，长度为11个字符
ALTER TABLE Student ADD COLUMN phone CHAR(11);
-- 插入数据
INSERT INTO Student(sNo,sName,sex,age,dept) VALUES('201201009','王毅','男',18,'外语');
-- values子句：指定添加数据的具体值。当指定字段名时，values子句中的值的排序顺序必须和字段名的排列顺序一致；若不指定字段，则values子句中的值的排列
-- 顺序必须与创建表字段时的排序顺序一致。
-- 修改数据
UPDATE Student SET dept = '金融' WHERE sNo = '201201009';
-- <表名>:要修改的值
-- set子句;要修改的字段及其修改后的值
-- where子句：指定待修改的记录应当满足的条件
-- 删除数据
-- DELETE FROM<表名>[WHERE <条件>]、
DELETE FROM Student WHERE sNO='201201009';

-- 数据查询
-- ALL：指定在结果集中显示所有记录，包括重复行。ALL是默认设置
-- DISTINCT:指定在结果集中显示所有记录，不包括重复行（用于去重）
-- TOP n [PERCENT]:指定从结果集中输出前n行，如果指定了PERCENT，表示结果集中输出前百分之n行
-- *  ：指定返回查询表的所有字段
-- FROM:用于指定查询的表或视图
-- WHERE：用于设置查询条件
-- GROUP BY子句：指明按照《字段名表>中的值进行分组，该字段的值相同的记录为一组，如果带HAVIN子句，则需要满足其后的条件，才予以输出
-- HAVING 用来指定每一个分组内应该满足的条件，即对每个分组内的记录进行筛选，常和GROUP BY连用


-- HAVING 和 WHERE 的区别
-- where子句是对整个表中的数据筛选出满足条件的记录；而HAVING子句是对Group by分组查询后产生的组设置的条件，所以是筛选出满足条件的组。
-- ORDER BY 将查询结果按指定的次序表达式的值升序或降序排列。ASC 升序  DESC 降序  ，此语句需放在sql命令的最后

-- 1.简单查询
SELECT sNo AS 学号,sName AS 姓名 FROM Student; -- 学号和姓名为别名

-- 2.条件查询
-- 常用的运算符及功能
-- =,>,<,>=,<=,!=,<>     比较大小
-- BETWEEN AND,NOT BETWEEN AND   确定范围
-- IN, NOT IN      确定集合
-- LIKE, NOT LIKE   字符匹配
-- IS NULL,IS NOT NULL   判断空值
-- AND , OR , NOT    逻辑运算
SELECT * FROM Student WHERE dept ='计算机';

-- 3.多重条件查询 当查询需要指定一个以上的查询条件时，这种条件称为多重条件或复合条件
SELECT * FROM Student WHERE dept ='计算机' AND sex = '男';

-- 4.模糊查询
-- 当查询条件不知道完全精确的值时，还可以使用like或not like进行模糊查询，模糊查询也称部分匹配查询
--   % 代表0个或多个字符
--   _代表一个字符
--   [] 表示在某一范围内的字符
--   [^]表示不在某一范围内的字符

SELECT * FROM Student WHERE sName LIKE '李%';  -- 查找所有姓李的同学


-- 5.常用的的统计函数及统计汇总查询
-- AVG<字段名> 字段名所在列的平均值
-- SUM<字段名> 字段名所在列的总和
-- MAX<字段名> 字段名所在列的最大值
-- MIN<字段名> 字段名所在列的最小值
-- COUNT (*)  统计表中记录的个数
-- 除了COUNT(*)外，其他函数在计算过程中均忽略NULL值、
SELECT AVG(grade) AS 平均成绩 FROM Score;

-- 6. ORDER BY
-- 在成绩表score中查询课程表号cNo为c001的学生的学号sNo和成绩grade，并按成绩降序排列
SELECT sNo,grade FROM score WHERE cNo = 'c001' ORDER BY grade DESC


```

### 18.ACID

![](https://img-blog.csdnimg.cn/8d137f8980ba46bea4f288043d1350ad.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

###  19.事务

![](https://img-blog.csdnimg.cn/2230dd9750a24a718cc2f5fe920b3532.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 ![](https://img-blog.csdnimg.cn/13df14dd428b44b79d155abaa33a43c5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

###  20.数据库三大范式

![](https://img-blog.csdnimg.cn/5e8d01d5cc4c4b6bbc3339dd237bc4d7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

###  21.第一个JDBC程序

```java

package com.gukeyang;

import java.sql.*;

//我的第一个jdbc程序
public class lesson01 {
public static void main(String[] args) throws ClassNotFoundException, SQLException {
//加载驱动
Class.forName("com.mysql.jdbc.Driver");

//用户信息和url
String url = "jdbc:mysql://localhost/jdbcstudy?useUnicode=true&characterEncoding=utf8&useSSL=true";
String username="root";
String password="123456";


//连接成功，数据库对象
Connection connection = DriverManager.getConnection(url,username,password);


//执行sql的对象
Statement statement=connection.createStatement();


//执行sql的对象去执行sql，可能存在结果，查看返回结果
String sql = "SELECT * FROM user";
ResultSet resultSet = statement.executeQuery(sql);

while(resultSet.next()){
System.out.println("id="+resultSet.getObject("id"));
System.out.println("name="+resultSet.getObject("NAME"));
System.out.println("pwd="+resultSet.getObject("PASSWORD"));
System.out.println("email="+resultSet.getObject("email"));
System.out.println("birthday="+resultSet.getObject("birthday"));
}

//释放连接
resultSet.close();
statement.close();
connection.close();
}



}
```

![](https://img-blog.csdnimg.cn/7adedeb512fd47618db8708eb0cc534d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

###  22.列的数据类型

数值

- tinyint       一个字节

- smallint     两个字节

- mediumint 三个字节

- int             四个字节

- bigint        八个字节

- float          四个字节

- double      八个字节

- decimal 字符串形式的浮点数

字符串

- varchar 可变字符串

- tinytext 微型文本

- test  文本串 

时间日期

- date   日期  YYYY-MM-DD

- time   时间  HH ：mm ：ss

- datetime

- timestamp 时间戳

- year 年份

null

### 23.索引

![](https://img-blog.csdnimg.cn/d3c537ac869d42a8972157337b5e2f23.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 ![](https://img-blog.csdnimg.cn/b27e431aa1b94e388dd2d3bebe1c2bee.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 ![](https://img-blog.csdnimg.cn/018ec9ca95da4d34a58de7f86ea46469.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 ![](https://img-blog.csdnimg.cn/0c3293f13dd24eceaaaf7f264bad5acf.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

###  24.数据库用户管理

![](https://img-blog.csdnimg.cn/e484a190b46b4a8c93b211b055c82083.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAbTBfMjI3,size_20,color_FFFFFF,t_70,g_se,x_16)

 

```sql
操作数据库->操作数据库的表->操作表中的数据

mysql的关键字不区分大小写
	* 
创建数据库   CREATE DATABASE IF NOT EXITS westons
	* 
删除数据库   DROP DATABASE IF EXITS westos
	* 
使用数据库   USE school
	* 
查看数据库   SHOW DATABASE    --->查看所有的数据库

```

### 25.参考视频：[【狂神说Java】MySQL最新教程通俗易懂\_哔哩哔哩\_bilibili](https://www.bilibili.com/video/BV1NJ411J79W?spm_id_from=333.1007.top_right_bar_window_default_collection.content.click "【狂神说Java】MySQL最新教程通俗易懂_哔哩哔哩_bilibili")