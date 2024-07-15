---
title: 《天池龙珠 - SQL训练营》01.SQL基础：初识数据库与SQL-安装与基本介绍等
author: 囧囧JOJO
top: false
cover: false
mathjax: false
tags: [MySQL, SQL, 天池龙珠, 笔记, 入门, 教程]
date: 2022-01-14 09:28:15
img:
coverImg:
password:
summary:

---

# 一、初识数据库

`DB`：数据库（Database）
`DBMS`：（Database Management System）
## 1.1 DBMS的种类
DBMS 主要通过数据的保存格式（数据库的种类）来进行分类，现阶段主要有以下 5 种类型.

- 层次数据库（Hierarchical Database，HDB）
- 关系数据库（Relational Database，RDB）
这种类型的 DBMS 称为关系数据库管理系统（Relational Database Management System，RDBMS）。比较具有代表性的 RDBMS 有如下 5 种。

	- Oracle Database：甲骨文公司的RDBMS
	- SQL Server：微软公司的RDBMS
	- DB2：IBM公司的RDBMS
	-  PostgreSQL：开源的RDBMS
 MySQL：开源的RDBMS
- 面向对象数据库（Object Oriented Database，OODB）
- XML数据库（XML Database，XMLDB）
- 键值存储系统（Key-Value Store，KVS），举例：MongoDB
本课程将向大家介绍使用 SQL 语言的数据库管理系统，也就是关系数据库管理系统（RDBMS）的操作方法。
本课程将向大家介绍使用 SQL 语言的数据库管理系统，也就是关系数据库管理系统（RDBMS）的操作方法。

## 1.2 RDBMS的常见系统结构
使用 RDBMS 时，最常见的系统结构就是客户端 / 服务器类型（C/S类型）这种结构。
## 1.3 数据库安装
目前可以使用云数据库和本地数据库，云数据库只需要在供应商处购买即可使用，这里主要介绍本地如何安装数据库的。
详细的数据库安装教程，请参考[阿里云的官方文档](https://tianchi-media.oss-cn-beijing.aliyuncs.com/dragonball/SQL/other/%E6%9C%AC%E5%9C%B0MySQL%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA%E6%96%B9%E6%B3%95%E4%BB%8B%E7%BB%8D.pdf)。
### 1.3.1 windows 下 MySQL 8.0 的下载安装
#### 1.3.1.1 下载
⾸先以最常⻅的 win10 为例，介绍 MySQL8.0 的下载安装。
MySQL 针对个⼈⽤户和商业⽤户提供了不同的版本
	MySQL 社区版(MySQL Community Edition) 是供个⼈⽤户免费下载的开源数据库，本教程将以MySQL 社区版为例进⾏安装连接和SQL查询的讲解。MySQL 官⽹上的社区版软件的下载地址https://dev.mysql.com/downloads/，选择[MySQL Installer for Windows](https://dev.mysql.com/downloads/windows/)可以下载 windows 操作系统下的最新版 MySQL安装⽂件。如果需要安装历史版本，可以选择最后的[Download Archives](https://downloads.mysql.com/archives/)后选择[MySQL Installer](https://downloads.mysql.com/archives/installer/)，然后在新⻚⾯⾥选择所需历史版本的社区版。
如果想下载本教程所使⽤的 MySQL 8.0.21.0，也可以在百度⽹盘下载
下载链接：[https://pan.baidu.com/s/1SOtMoVqqRXwa2qD0siHcIg](https://pan.baidu.com/s/1SOtMoVqqRXwa2qD0siHcIg)提取码：80lf
备⽤下载链接： [https://pan.baidu.com/s/1zK2vj50DvuAee-EqAcl-0A](https://pan.baidu.com/s/1zK2vj50DvuAee-EqAcl-0A)提取码：80lf
我们接下来以⽂档写作时的最新版 MySQL 8.0.21 为例，进⾏下载安装的介绍。
进⼊到[MySQL Installer for Windows](https://dev.mysql.com/downloads/windows/)⻚⾯后，选择下载下⽅的完整安装程序。
#### 1.3.1.2 安装
找到刚才下载好的msi⽂件，双击开始安装。初学者建议采⽤完全安装模式(Full)进⾏安装：
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16.png)
完全安装模式下，部分模块会依赖其他其他组件(每台电脑上列出的依赖项很可能会有不同)。
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-16458157070781.png)
如果你的电脑之前没有安装过这些组件，则需要额外进⾏安装，此处点击 Execute 按钮即可：
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_17,color_FFFFFF,t_70,g_se,x_16.png)
待下边剩下 3 个按钮且上⽅⼤部分组件为绿⾊时，即可点击 Next：
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-16458157070782.png)
点击 Excute，开始安装服务器软件MySQL Server，连接和查询软件MySQL Workbench及其他相关软
件等内容。
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-16458157070783.png)
稍等⽚刻，安装完成后，点击 Next。
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-16458157070794.png)

下图这⼀步是选是否以集群⽅式安装 MySQL，我们选择默认的第⼀个，然后点击 Next：
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-16458157070795.png)
此处上边的各种相关配置保持默认即可，勾选最下边的"Show Advanced and Logging Options"框，然后点击 Next：
下图是密码强度的设置，第⼀种模式为强密码校验模式，MySQL 8.0 推荐使⽤最新的数据库和客户端，
更换了加密插件，者可能导致第三⽅客户端⼯具⽆法连接数据库。
第⼆种加密⽅式沿袭了 MySQL 5.x 的加密⽅式，对第三⽅⼯具连接不敏感，我们仅为了学习 SQL 查
询，并不需要很⾼的安全性，因此此处请务必选择第⼆种⽅式(⾮常重要)：
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-16458157070796.png)
在这⼀步设置 MySQL 的 root 账户密码，由于上⼀步选择了第⼆个选项，因此这⾥可以设置为较简单容
易记忆的⽽密码，例如"123456"。建议设置⽐较简单的密码，并将密码记录下来以防遗忘，忘记密码是
⼀件麻烦事。
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-16458157070797.png)

此处保持默认即可，如果 windows service name 右侧有⻩⾊警告图标显示，表示名称重复，⼿动更换
⼀个名称即可，然后点击 Next：
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-16458157070798.png)
Logging Options 这⾥使⽤默认设置即可，我们的学习中暂时⽤不到这些设置，直接点击 Next：
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-16458157070799.png)
下图是设置是否⼤⼩写敏感的。这⼀步⾮常重要，由于windows系统是⼤⼩写不敏感的**，请⼤家务必
使⽤第⼀个选项Lower Case**。
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570707910.png)
点击 Execute
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570707911.png)
完成安装后，在下图中点击 Finish 回到安装的主进程：
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570707912.png)
在主进程界⾯点击 Next
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570707913.png)
这⼀步⽆需任何选择，直接点击 Finish
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570707914.png)
进⼊到 Connect To Server 界⾯后，输⼊刚才设置的密码，点击 check 进⾏校验，校验通过后 Status 会
显示连接成功，然后点击 Next
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570708015.png)
点击 Excute 应⽤设置：
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570708016.png)
上述步骤完成后，点击 Finish
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570708017.png)
回到安装主进程后，点击 Next
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570708018.png)
点击 Finish，完成安装。
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570708019.png)
现在，你的电脑上就已经安装了MySQL的服务器软件，⽤于连接服务器进⾏查询的MySQL Workbench，以及其他程序语⾔连接MySQL的驱动，此外还安装了⼏个示例数据库，但本教程将采⽤<SQL基础教程>⼀书中的示例数据库，该数据库的创建和数据导⼊将在本章第三节介绍。
### 1.3.2  MacOS 下 MySQL 8.0 的下载安装
详情参考阿里云的文档
### 1.3.3  Linux 下 MySQL 8.0 的下载安装
详情参考阿里云的文档

# 二、初识 SQL
## 2.1 概念介绍
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570708020.png)

数据库中存储的表结构类似于excel中的行和列，在数据库中，行称为记录，它相当于一条记录，列称为字段，它代表了表中存储的数据项目。

行和列交汇的地方称为单元格，一个单元格中只能输入一条记录。

SQL是为操作数据库而开发的语言。国际标准化组织（ISO）为 SQL 制定了相应的标准，以此为基准的SQL 称为标准 SQL（相关信息请参考专栏——标准 SQL 和特定的 SQL）。

完全基于标准 SQL 的 RDBMS 很少，通常需要根据不同的 RDBMS 来编写特定的 SQL 语句，原则上，本课程介绍的是标准 SQL 的书写方式。

根据对 RDBMS 赋予的指令种类的不同，SQL 语句可以分为以下三类.

- DDL
DDL（Data Definition Language，数据定义语言） 用来创建或者删除存储数据用的数据库以及数据库中的表等对象。DDL 包含以下几种指令。

	- CREATE ： 创建数据库和表等对象

	- DROP ： 删除数据库和表等对象 

	- ALTER ： 修改数据库和表等对象的结构

- DML
DML（Data Manipulation Language，数据操纵语言） 用来查询或者变更表中的记录。DML 包含以下几种指令。

	- SELECT ：查询表中的数据

	- INSERT ：向表中插入新数据

	- UPDATE ：更新表中的数据

	- DELETE ：删除表中的数据

- DCL
	DCL（Data Control Language，数据控制语言） 用来确认或者取消对数据库中的数据进行的变更。除此之外，还可以对 RDBMS 的用户是否有权限操作数据库中的对象（数据库表等）进行设定。DCL 包含以下几种指令。

	- COMMIT ： 确认对数据库中的数据进行的变更

	- ROLLBACK ： 取消对数据库中的数据进行的变更

	- GRANT ： 赋予用户操作权限

	- REVOKE ： 取消用户的操作权限

实际使用的 SQL 语句当中有 90% 属于 DML，本课程会以 DML 为中心进行讲解。

## 2.2 SQL的基本书写规则

- SQL语句要以分号（ ; ）结尾
- SQL 不区分关键字的大小写，但是插入到表中的数据是区分大小写的
- win 系统默认不区分表名及字段名的大小写
- linux / mac 默认严格区分表名及字段名的大小写
- 本教程已统一调整表名及字段名的为小写，以方便初学者学习使用。
- 常数的书写方式是固定的
'abc', 1234, '26 Jan 2010', '10/01/26', '2010-01-26'…
- 单词需要用半角空格或者换行来分隔
SQL 语句的单词之间需使用半角空格或换行符来进行分隔，且不能使用全角空格作为单词的分隔符，否则会发生错误，出现无法预期的结果。

请大家认真查阅《附录1 - SQL 语法规范》，养成规范的书写习惯。

## 2.3 数据库的创建（ CREATE DATABASE 语句）
**语法：**

```sql
CREATE DATABASE < 数据库名称 > ;
```

**创建本课程使用的数据库**

```sql
CREATE DATABASE shop;
```
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_19,color_FFFFFF,t_70,g_se,x_16.png)

## 2.4 表的创建（ CREATE TABLE 语句）
**语法：**

```sql
CREATE TABLE < 表名 >
( < 列名 1> < 数据类型 > < 该列所需约束 > ,
  < 列名 2> < 数据类型 > < 该列所需约束 > ,
  < 列名 3> < 数据类型 > < 该列所需约束 > ,
  < 列名 4> < 数据类型 > < 该列所需约束 > ,
  .
  .
  .
  < 该表的约束 1> , < 该表的约束 2> ,……);
```

**创建本课程用到的商品表**

```sql
CREATE TABLE product(
     product_id CHAR(4) NOT NULL, 
     product_name VARCHAR(100) NOT NULL, 
     product_type VARCHAR(32) NOT NULL, 
     sale_price INTEGER, 
     purchase_price INTEGER, 
     regist_date DATE, 
     PRIMARY KEY(product_id)
 )  ;
```
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570708021.png)

## 2.5 命名规则
- 只能使用半角英文字母、数字、下划线（_）作为数据库、表和列的名称
- 名称必须以半角英文字母开头

商品表和 product 表列名的对应关系
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570708022.png)
2.6 数据类型的指定
数据库创建的表，所有的列都必须指定数据类型，每一列都不能存储与该列数据类型不符的数据。

四种最基本的数据类型

- INTEGER 型
用来指定存储整数的列的数据类型（数字型），不能存储小数。

- CHAR 型
用来存储定长字符串，当列中存储的字符串长度达不到最大长度的时候，使用半角空格进行补足，由于会浪费存储空间，所以一般不使用。

- VARCHAR 型
用来存储可变长度字符串，定长字符串在字符数未达到最大长度时会用半角空格补足，但可变长字符串不同，即使字符数未达到最大长度，也不会用半角空格补足。

- DATE 型
用来指定存储日期（年月日）的列的数据类型（日期型）。

## 2.7 约束的设置
约束是除了数据类型之外，对列中存储的数据进行限制或者追加条件的功能。

`NOT NULL`是非空约束，即该列必须输入数据。

`PRIMARY KEY`是主键约束，代表该列是唯一值，可以通过该列取出特定的行的数据。

## 2.8 表的删除和更新
- 删除表的语法：

```sql
DROP TABLE < 表名 > ;
```
- 删除 product 表
需要特别注意的是，删除的表是无法恢复的，只能重新插入，请执行删除操作时无比要谨慎。

```sql
DROP TABLE product;
```
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570708023.png)

- 添加列的 ALTER TABLE 语句

```sql
ALTER TABLE < 表名 > ADD COLUMN < 列的定义 >;
```
- 添加一列可以存储100位的可变长字符串的 product_name_pinyin 列

```sql
ALTER TABLE product ADD COLUMN product_name_pinyin VARCHAR(100);
```
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570708024.png)

- 删除列的 ALTER TABLE 语句

```sql
ALTER TABLE < 表名 > DROP COLUMN < 列名 >;
```
- 删除 product_name_pinyin 列

```sql
ALTER TABLE product DROP COLUMN product_name_pinyin;
```
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570708025.png)

ALTER TABLE 语句和 DROP TABLE 语句一样，执行之后无法恢复。误添的列可以通过 ALTER TABLE 语句删除，或者将表全部删除之后重新再创建。


【扩展内容】
- 清空表内容

```sql
TRUNCATE TABLE TABLE_NAME;
```
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570708126.png)

优点：相比`drop`/`delete`，`truncate`用来清除数据时，速度最快。

- 数据的更新
基本语法：

```sql
UPDATE <表名>
SET <列名> = <表达式> [, <列名2>=<表达式2>...];  
WHERE <条件>;  -- 可选，非常重要。
ORDER BY 子句;  --可选
LIMIT 子句; --可选
```
**使用 update 时要注意添加 where 条件，否则将会将所有的行按照语句修改**

```sql
-- 修改所有的注册时间
UPDATE product
   SET regist_date = '2009-10-10';  
-- 仅修改部分商品的单价
UPDATE product
   SET sale_price = sale_price * 10
 WHERE product_type = '厨房用具';  
```
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570708127.png)

使用 UPDATE 也可以将列更新为 NULL（该更新俗称为NULL清空）。此时只需要将赋值表达式右边的值直接写为 NULL 即可。

```sql
-- 将商品编号为0008的数据（圆珠笔）的登记日期更新为NULL  
UPDATE product
   SET regist_date = NULL
 WHERE product_id = '0008';  
```
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570708128.png)

和 INSERT 语句一样， UPDATE 语句也可以将 NULL 作为一个值来使用。
**但是，只有未设置 NOT NULL 约束和主键约束的列才可以清空为NULL。**如果将设置了上述约束的列更新为 NULL，就会出错，这点与INSERT 语句相同。
多列更新

UPDATE 语句的 SET 子句支持同时将多个列作为更新对象。

```sql
-- 基础写法，一条UPDATE语句只更新一列
UPDATE product
   SET sale_price = sale_price * 10
 WHERE product_type = '厨房用具';
UPDATE product
   SET purchase_price = purchase_price / 2
 WHERE product_type = '厨房用具';  
```
该写法可以得到正确结果，但是代码较为繁琐。可以采用合并的方法来简化代码。

```sql
-- 合并后的写法
UPDATE product
   SET sale_price = sale_price * 10,
       purchase_price = purchase_price / 2
 WHERE product_type = '厨房用具';  
```
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570708129.png)

需要明确的是，SET 子句中的列不仅可以是两列，还可以是三列或者更多。

## 2.9 向 product 表中插入数据
为了学习·INSERT·语句用法，我们首先创建一个名为·productins·的表，建表语句如下：

```sql
CREATE TABLE productins
(product_id    CHAR(4)      NOT NULL,
product_name   VARCHAR(100) NOT NULL,
product_type   VARCHAR(32)  NOT NULL,
sale_price     INTEGER      DEFAULT 0,
purchase_price INTEGER ,
regist_date    DATE ,
PRIMARY KEY (product_id)); 
```
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570708130.png)

基本语法：

```sql
INSERT INTO <表名> (列1, 列2, 列3, ……) VALUES (值1, 值2, 值3, ……);  
```

对表进行全列 INSERT 时，可以省略表名后的列清单。这时 VALUES子句的值会默认按照从左到右的顺序赋给每一列。

```sql
-- 包含列清单
INSERT INTO productins (product_id, product_name, product_type, 
sale_price, purchase_price, regist_date) VALUES ('0005', '高压锅', '厨房用具', 6800, 5000, '2009-01-15');
-- 省略列清单
INSERT INTO productins 
VALUES ('0005', '高压锅', '厨房用具', 6800, 5000, '2009-01-15');  
```
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570708131.png)

原则上，执行一次 INSERT 语句会插入一行数据。插入多行时，通常需要循环执行相应次数的 INSERT 语句。其实很多 RDBMS 都支持一次插入多行数据

```sql
-- 通常的INSERT
INSERT INTO productins VALUES ('0002', '打孔器', 
'办公用品', 500, 320, '2009-09-11');
INSERT INTO productins VALUES ('0003', '运动T恤', 
'衣服', 4000, 2800, NULL);
INSERT INTO productins VALUES ('0004', '菜刀', 
'厨房用具', 3000, 2800, '2009-09-20');
-- 多行INSERT （ DB2、SQL、SQL Server、 PostgreSQL 和 MySQL多行插入）
INSERT INTO productins VALUES ('0002', '打孔器', 
'办公用品', 500, 320, '2009-09-11'),
('0003', '运动T恤', '衣服', 4000, 2800, NULL),
('0004', '菜刀', '厨房用具', 3000, 2800, '2009-09-20');  
-- Oracle中的多行INSERT
INSERT ALL INTO productins VALUES ('0002', '打孔器', '办公用品', 500, 320, '2009-09-11')
INTO productins VALUES ('0003', '运动T恤', '衣服', 4000, 2800, NULL)
INTO productins VALUES ('0004', '菜刀', '厨房用具', 3000, 2800, '2009-09-20')
SELECT * FROM DUAL;  
-- DUAL是Oracle特有（安装时的必选项）的一种临时表A。因此“SELECT *FROM DUAL” 部分也只是临时性的，并没有实际意义。  
```
INSERT 语句中想给某一列赋予 NULL 值时，可以直接在 VALUES子句的值清单中写入 NULL。想要插入 NULL 的列一定不能设置 NOT NULL 约束。
```sql
INSERT INTO productins (product_id, product_name, product_type, 
sale_price, purchase_price, regist_date) VALUES ('0006', '叉子', 
'厨房用具', 500, NULL, '2009-09-20');  
```
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570708132.png)

还可以向表中插入默认值（初始值）。可以通过在创建表的CREATE TABLE 语句中设置DEFAULT约束来设定默认值。

```sql
CREATE TABLE productins
(product_id CHAR(4) NOT NULL,
（略）
sale_price INTEGER
（略）	DEFAULT 0, -- 销售单价的默认值设定为0;
PRIMARY KEY (product_id));  
```
可以使用INSERT … SELECT 语句从其他表复制数据。

```sql
-- 将商品表中的数据复制到商品复制表中
INSERT INTO productocpy (product_id, product_name, product_type, sale_price, purchase_price, regist_date)
SELECT product_id, product_name, product_type, sale_price, 
purchase_price, regist_date
FROM Product;  
```
- 本课程用表插入数据sql如下：

```sql
- DML ：插入数据
STARTTRANSACTION;
INSERT INTO product VALUES('0001', 'T恤衫', '衣服', 1000, 500, '2009-09-20');
INSERT INTO product VALUES('0002', '打孔器', '办公用品', 500, 320, '2009-09-11');
INSERT INTO product VALUES('0003', '运动T恤', '衣服', 4000, 2800, NULL);
INSERT INTO product VALUES('0004', '菜刀', '厨房用具', 3000, 2800, '2009-09-20');
INSERT INTO product VALUES('0005', '高压锅', '厨房用具', 6800, 5000, '2009-01-15');
INSERT INTO product VALUES('0006', '叉子', '厨房用具', 500, NULL, '2009-09-20');
INSERT INTO product VALUES('0007', '擦菜板', '厨房用具', 880, 790, '2008-04-28');
INSERT INTO product VALUES('0008', '圆珠笔', '办公用品', 100, NULL, '2009-11-11');
COMMIT;
```
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570708133.png)
# 三、练习题
## 3.1 创建表
编写一条 CREATE TABLE 语句，用来创建一个包含表 1-A 中所列各项的表 Addressbook （地址簿），并为 regist_no （注册编号）列设置主键约束

表 Addressbook （地址簿）中的列
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570708234.png)

```sql
CREATE TABLE Addressbook (
	regist_no INTEGER NOT NULL,
	name VARCHAR ( 128 ) NOT NULL,
	address VARCHAR ( 256 ) NOT NULL,
	tel_no CHAR ( 10 ),
	mail_address CHAR ( 20 ),
PRIMARY KEY ( regist_no ) 
);
```
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570708235.png)
## 3.2 插入新列
假设在创建练习1.1中的 Addressbook 表时忘记添加如下一列 postal_code （邮政编码）了，请把此列添加到 Addressbook 表中。

	列名 ： postal_code
	
	数据类型 ：定长字符串类型（长度为 8）
	
	约束 ：不能为 NULL
```sql
ALTER TABLE addressbook ADD COLUMN postal_code CHAR ( 8 ) NOT NULL;
```
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570708236.png)
## 3.3 删除表
编写 SQL 语句来删除 Addressbook 表。
```sql
DROP TABLE addressbook;
```
![在这里插入图片描述](/assets/images/VDztdrSqb/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Zun5ZunSk9KTw==,size_20,color_FFFFFF,t_70,g_se,x_16-164581570708237.png)
## 3.4 恢复删除的表
编写 SQL 语句来恢复删除掉的 Addressbook 表。

**删除后的表⽆法使⽤命令进⾏恢复，只能重新创建**

```sql
CREATE TABLE Addressbook (
	regist_no INTEGER NOT NULL,
	name VARCHAR ( 128 ) NOT NULL,
	address VARCHAR ( 256 ) NOT NULL,
	postal_code CHAR ( 8 ) NOT NULL,
	tel_no CHAR ( 10 ),
	mail_address CHAR ( 20 ),
PRIMARY KEY ( regist_no ) 
);
```
