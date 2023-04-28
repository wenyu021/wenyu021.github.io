---
title: 从0开始学SQL
tags:
  - SQL
abbrlink: 63e7a474
date: 2022-03-15 01:11:33
category: 数据库
toc: true
---
## 第一章
### **初识数据库**   
首先要了解什么是数据库
> 数据库是将大量数据保存起来，通过计算机加工而成的可以进行高效访问的数据集合。该数据集合称为数据库（Database，DB）。  
> 用来管理数据库的计算机系统称为数据库管理系统（Database Management System，DBMS）。

除此之外，还会有`数据库系统（Database System，DBS）`。DB是一种数据集合，DBMS是管理系统，一种计算机软件，DBS就是由DBMS和DB以及其他的的硬件软件组成的一个整体。  <!--more-->
> 思考：
>  * 一本书是不是数据库
>  * 图书馆目录卡是不是数据库
>  * 银行对账单是不是数据库
>  * 电子数据表是不是数据库

****

### **初识SQL**
> `SQL`是一种数据库查询和程序设计语言，用于存取数据以及查询、更新和管理关系数据库系统。  
> 
SQL的特点：  
1. 数据描述、操纵、控制等功能一体化。
2. 高度非过程化。只需要提出需要什么，而不需要提出要怎么获得。
3. 语法简洁易用
#### **创建数据库**
创建一个名为shop的数据库
```sql
CREATE DATABASE shop ;
```
#### **创建表**
创建一个名为product的表
```sql
CREATE TABLE product
(product_id CHAR(4) NOT NULL,
 product_name VARCHAR(100) NOT NULL,
 product_type VARCHAR(32) NOT NULL,
 sale_price INTEGER ,
 purchase_price INTEGER ,
 regist_date DATE ,
 PRIMARY KEY (product_id));
```
SQL中的命名规则规定名称必须以英文字母开头，只能包含英文字母，数字，下划线。
其中`product_id`是列名，`CHAR`是数据类型, `4`是这一列数据的大小, `NOT NULL`是对数据的约束这里是不能为空。
#### 删除表/更新表
* 删除
> `DROP TABLE <表名>； `
* 删除/添加表中的指定列
> ` ALTER TABLE <表名> ADD COLUMN <列的定义>`  
> 其中列的定义与创建表时填写的一样，需要指明**列表名**， **数据类型**等。  
> `ALTER TABLE <表名> DROP COLUMN <列名>;`
* 清空表的内容
> `TRUNCATE TABLE <表名>;`
* 更新表中的数据
> `UPDATE` <表名>  
   `SET` <列名> = <表达式> [, <列名2>=<表达式2>...];    
 `WHERE` <条件>;  -- 可选，非常重要。  
 `ORDER BY `子句;  --可选  
 `LIMIT` 子句; --可选  
>>where 条件指定了具体范围，不指定将会更改所有的行
* 插入数据
> `INSERT INTO <表名> (列1, 列2, 列3, ……) VALUES (值1, 值2, 值3, ……); `   

插入多行时可以直接将多行的值写在 `VALUES` 后面
> `INSERT INTO <表名> (列1, 列2, 列3, ……) VALUES (值1, 值2, 值3, ……), ` 
> `(值1, 值2, 值3, ……), (值1, 值2, 值3, ……); `  
#### 建立索引
三种方法可以建立索引  
1. 创建表的时候建立
   ```sql
   CREATE TABLE mytable(
     ...   
   INDEX [索引名] <列名>  
   );
   ```
2. 使用`CREATE`
    
    >CREATE INDEX 索引名 ON 表名 (列名)；
    

3. 使用`ALERT`
   
   > ALTER TABLE 表名 ADD INDEX 索引名 (列名)；
    

*****

## 练习题
### 1.1
编写一条 CREATE TABLE 语句，用来创建一个包含表 1-A 中所列各项的表 Addressbook （地址簿），并为 regist_no （注册编号）列设置主键约束  
![](../img/ch01.04习题1.png)
```sql
CREATE TABLE "Adressbook" (
	"regist_no"	INTEGER NOT NULL,
	"name"	VARCHAR(128) NOT NULL,
	"address"	VARCHAR(256) NOT NULL,
	"tel_no"	CHAR(10),
	"mail_address"	CHAR(20),
	PRIMARY KEY("regist_no")
);
```
### 1.2
假设在创建练习1.1中的 Addressbook 表时忘记添加如下一列 postal_code （邮政编码）了，请编写 SQL 把此列添加到 Addressbook 表中。

列名 ： postal_code

数据类型 ：定长字符串类型（长度为 8）

约束 ：不能为 NULL
```sql
ALTER TABLE Adressbook ADD COLUMN postal_code CHAR(8) NOT NULL;
```
### 1.3
请补充如下 SQL 语句来删除 Addressbook 表。
```sql
DROP TABLE Addressbook;
```
### 1.4
是否可以编写 SQL 语句来恢复删除掉的 Addressbook 表？  
> 删除的表是无法恢复的，只能重新插入
****
**声明** ：以上学习内容根据DataWhale的[从0到1掌握SQL课程](https://github.com/datawhalechina/wonderful-sql)