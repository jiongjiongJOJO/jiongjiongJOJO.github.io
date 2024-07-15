---
title: Hive实验一：Hive安装部署及常用命令
author: 囧囧JOJO
top: false
cover: false
mathjax: false
tags: [Hive, Hadoop, 教程, 入门, 笔记, Docker]
date: 2021-12-03 10:53:53
img:
coverImg:
password:
summary:

---

# Hive安装部署及常用命令

## 1 实验准备

### 1.1 启动Hadoop集群

首先按照顺序启动四个Docker容器

```bashbash
docker start master
docker start slave1
docker start slave2
docker start slave3
```

接着使用以下命令，进入到master环境内：

```bash
docker exec -it --privileged master /bin/bash
```

然后在master容器内启动Hadoop服务

```bash
start-dfs.sh
start-yarn.sh
mr-jobhistory-daemon.sh start historyserver
```

### 1.2 资源文件夹

资源文件夹位于`/cgsrc`，该文件夹存放了所有实验中需要用到的安装包。

文件夹中的内容是只读的，需要使用时请挂载到docker容器上。

在《hadoop安装部署实验》中，我们将该资源文件夹挂载到了每个docker容器的`/cgsrc`目录上

![](/assets/images/QqVcDzIex/1638688679820.png)

### 1.3 工作目录

本实验的工作目录为`~/course/hive/hive_op`，**在桌面右键打开新的终端**并使用以下命令创建和初始化工作目录：

```bash
mkdir -p ~/course/hive/hive_op
cd ~/course/hive/hive_op
```

### 1.4 准备数据

在桌面新打开的终端上，在本实验的工作目录`~/course/hive/hive_op`下创建`student.txt`文件,并写入如下数据:

```tex
1001,zhangsan,F,19,XG
1002,wangwu,F,20,XG
1003,liwei,M,19,WL
1004,liulan,F,20,WL
1005,zhaozilin,M,18,GS
1006,huangfei,M,21,XG
1007,wuwei,F,20,GS
1008,yangfang,M,17,XG
1009,zhanwang,F,19,GS
1010,wanggui,F,20,WL
1011,lilei,M,16,JC
1012,liulei,F,20,JC
1013,zhaolin,M,18,WL
1014,huanghu,M,21,JC
1015,weiwei,F,21,WLW
```

使用如下命令进入**master**节点：

```bash
docker exec -it --privileged master bash
```

在**master**节点上，使用如下命令将`student.txt`上传至hdfs的`/hive/test`目录下：

```bash
hadoop fs -mkdir -p /hive/test
hadoop fs -put /course/hive/hive_op/student.txt  /hive/test
```

## 2 安装Hive及配置环境

### 2.1 Hive安装

在桌面右键打开新的终端，执行如下命令进入**master**节点：

```bash
docker exec -it --privileged master /bin/bash
```

从` /cgsrc` 中将 Hive 的安装文件复制到 `/usr/loacl` 目录下

```bash
cp /cgsrc/apache-hive-1.2.1-bin.tar.gz /usr/local/
```

执行如下命令解压：

```bash
cd /usr/local
tar -zxvf apache-hive-1.2.1-bin.tar.gz
rm -f apache-hive-1.2.1-bin.tar.gz
```

将解压得到的`apache-hive-1.2.1-bin` 文件夹重命名为 `hive`

```bash
mv apache-hive-1.2.1-bin/ hive
```

### 2.2 配置环境变量

使用vim编辑器打开 `~/.bashrc` 文件：

```bash
vim ~/.bashrc
```

然后在该文件最末加入下面一行内容：

```bash
export HIVE_HOME=/usr/local/hive
export PATH=$PATH:$HIVE_HOME/bin
```

保存后执行如下命令使配置生效：

```bash
source ~/.bashrc
```

## 3 测试Hive

执行如下命令启动hive：

```bash
hive
```

若得到如下输出则安装成功：

![](/assets/images/QqVcDzIex/1638688772782.png)

至此Hive安装成功。

输入`quit;`可以退出Hive Console

## 4 启动Hive

在**master**节点上输入如下命令启动 Hive服务 ：

```
hive --service hiveserver2 &
```

接着在**master**节点上输入如下命令进入beeline终端并连接Hive:

```
beeline
!connect jdbc:hive2://localhost:10000
```

输入账号/密码：root/root

![](/assets/images/QqVcDzIex/1638688787524.png)

## 5 DDL命令

### 5.1 数据库操作

#### 5.1.1 创建简单数据库

```bash
CREATE DATABASE testdb1;
CREATE DATABASE IF NOT EXISTS testdb2;
```

创建数据库的同时，设置数据库的存储路径:

```bash
CREATE DATABASE testdb3 LOCATION '/user/mydb';
```

创建数据库的同时，给数据库添加注释(描述信息保存在元数据表DBS的DESC项中):

```bash
CREATE DATABASE testdb4 COMMENT 'This is a test database4';
```

![](/assets/images/QqVcDzIex/1638688809149.png)

#### 5.1.2 查看数据库

```bash
SHOW DATABASES;
SHOW DATABASES LIKE "testdb*";
```

执行效果如下：

![](/assets/images/QqVcDzIex/1638688826391.png)

查看当前数据库:

```bash
select current_database();
```

修改（数据库的元数据信息不可更改）:

```bash
alter database testdb4 set dbproperties('date'='2019-10-10');
```

查看库的详细属性信息:

```bash
desc database extended testdb4;
```

![](/assets/images/QqVcDzIex/1638688841410.png)

#### 5.1.3 删除数据库

```bash
drop database testdb1;
```

打开库，并建立数据表，删除数据库:

```bash
use testdb4;
create table mytable(ling string);
drop database testdb4;
```

![](/assets/images/QqVcDzIex/1638688855970.png)

查看出错原因，并执行下列删除操作

```bash
drop database testdb4 cascade;
```

![](/assets/images/QqVcDzIex/1638688865685.png)

### 5.2 数据表操作

#### 5.2.1 建表操作

##### 5.2.1.1 创建内部表

```bash
create table if not exists t1(
id int,
name string,
gender string,
age int,
depart string
);
```

![](/assets/images/QqVcDzIex/1638688897861.png)


##### 5.2.1.2 创建外部表

```bash
create external table if not exists t2(
id int,
name string,
gender string,
age int,
depart string
);
```

![](/assets/images/QqVcDzIex/1638688921067.png)

##### 5.2.1.3 创建分区表

```bash
create table if not exists t3(
id int,
name string,
gender string,
age int
)partitioned by (depart string);
```

![](/assets/images/QqVcDzIex/1638688937529.png)

##### 5.2.1.4 创建桶表

```bash
create table if not exists t4(
id int,
name string,
gender string,
age int,
depart string
)clustered by (id) into 3 buckets;
```

![](/assets/images/QqVcDzIex/1638688952745.png)

#### 5.2.2 修改表

##### 5.2.2.1 重命名表

```bash
alter table t4 rename to tb4;
```

##### 5.2.2.2 增加字段

为t1表增加字段score

```bash
alter table t1 add columns(score float);
```

查看增加字段后的表结构

```bash
desc t1;
```

查看增加字段后表的详细信息

```bash
desc formatted t1;
```

![](/assets/images/QqVcDzIex/1638688971452.png)

![](/assets/images/QqVcDzIex/1638688982229.png)

##### 5.2.2.3 修改字段：字段重命名、修改位置、类型或注释

将字段score的类型由float改为int

```bash
alter table t1 change score score int;
```

将字段score的名字改为grade类型改为double

```bash
alter table t1 change score grade double;
```

将字段grade类型改为int，位置调整到sex字段后

```bash
alter table t1 change grade grade int after gender;
```

注意：即使字段名或字段类型没有改变，也需要完全指定旧的字段名，并给出新的字段名及类型

![](/assets/images/QqVcDzIex/1638688999166.png)

##### 5.2.2.4 删除或替换字段

```bash
alter table t1 replace columns(id string,name string,sex string,age int);
```

移除之前所有字段并重新指定了新的字段

##### 5.2.2.5 修改表的属性

```
alter table t1 set tblproperties('alter_date'='2019-8-27');
```

![](/assets/images/QqVcDzIex/1638689012537.png)

##### 5.2.2.6 修改表分区信息

添加分区

```bash
alter table 分区表名 add if not exists partition(分区字段名=分区字段值);
```

添加分区的同时可以使用location指定路径。 为t3表增加一个depart为'XG'的分区，观察hdfs中目录结构的变化

```bash
alter table t3 add if not exists partition(depart='XG');
```

查看分区

```bash
show partitions t3;
```

重命名分区

```bash
ALTER TABLE t3 PARTITION (depart='XG') RENAME TO PARTITION (depart='WLW');
```

![](/assets/images/QqVcDzIex/1638689028083.png)

#### 5.2.3 复制一个空表

```bash
create table empty like t1;
```

#### 5.2.4 删除表

```bash
drop table if exists t1;
```

#### 5.2.5.删除分区

```
alter table 分区表名 drop if exists partition(分区字段名=分区字段值);
alter table t3 drop partition(depart='WLW');
```

![](/assets/images/QqVcDzIex/1638689042997.png)

## 6 DML命令

数据导入有以下几种方法：

1.向表中装载数据（Load）

```
load data [local] inpath "path" [overwrite] into table tb_name [partition (partcol1=val1,…)];
```

`load data`:表示加载数据 `local`:表示从本地加载数据到 hive 表；否则从 HDFS 加载数据到 hive 表 `inpath`:表示加载数据的路径 `overwrite`:表示覆盖表中已有数据，否则表示追加 `into table`:表示加载到哪张表 `tb_name`:表示具体的表 `partition`:表示上传到指定分区

2.使用`insert`实现数据导入

3.查询语句中创建表并加载数据（As Select）

4.使用`sqoop`实现数据导入

### 6.1 内部表基本操作

#### 6.1.1 创建数据库exercise

```bash
create database if not exists exercise;
```

#### 6.1.2 打开数据库exercise，创建数据表stuInfo

```bash
use exercise;
create table if not exists stuinfo(
id int,
name string,
gender string,
age int,
depart string
)row format delimited fields terminated by ',' lines terminated by '\n' stored as textfile;
```

#### 6.1.3 查看数据表中内容

```bash
select * from stuinfo;
```

![](/assets/images/QqVcDzIex/1638689056669.png)

#### 6.1.4 向stuinfo中导入本地数据

```bash
load data local inpath '/course/hive/hive_op/student.txt' into table stuinfo;
```

#### 6.1.5 查看数据表中内容

```bash
select * from stuinfo;
```

![](/assets/images/QqVcDzIex/1638689068050.png)

#### 6.1.6 创建数据表stuinfo1

```bash
create table stuinfo1 like stuinfo;
```

#### 6.1.7 加载hdfs中数据到表中

```bash
load data inpath '/hive/test/student.txt' into table stuinfo1;
```

观察hdfs的`/hive/test`目录下的文件在加载前后的变化

#### 6.1.8 查询语句中创建表并加载数据（as select）

```bash
create table stuinfo2 as select * from stuinfo1;
```

#### 6.1.9 创建stuinfo3并通过insert插入数据

```bash
create table stuinfo3 like stuinfo;
insert into table stuinfo3 select * from stuinfo1;
select * from stuinfo3;
```

#### 6.1.10 删除stuinfo3表，观察目录结构和文件的变化

```bash
drop table stuinfo3;
```

![](/assets/images/QqVcDzIex/1638689086310.png)

### 6.2 外部表的基本操作

#### 6.2.1 创建外部表

```
create external table ext_stu1 like stuinfo location '/hive/test';
```

#### 6.2.2 查看外部表中信息

```
select * from ext_stu1;
```

![](/assets/images/QqVcDzIex/1638689097818.png)

#### 6.2.3 创建另一个外部表

```bash
create external table ext_stu like stuinfo;
```

#### 6.2.4 导入数据

```bash
load data inpath '/hive/test/student.txt' into table ext_stu;
```

**注意**：执行命名前，先确保hdfs上`/hive/test/student.txt`文件存在，若不存在，先参考实验准备步骤将数据文件上传至hdfs。

#### 6.2.5 查看表中信息和hdfs中文件

```bash
select * from ext_stu;
select * from ext_stu1;
dfs -ls /hive/test;
```

![](/assets/images/QqVcDzIex/1638689108198.png)

#### 6.2.6 删除数据表ext_stu和ext_stu1，并观察目录结构的变化

```bash
drop table ext_stu1;
drop table ext_stu;
```

### 6.3 分区表的基本操作

#### 6.3.1 创建分区表

```bash
create table if not exists stu_ptn(
id int,
name string,
gender string,
age int
)partitioned by (depart string) row format delimited fields terminated by ',' lines terminated by '\n';
```

#### 6.3.2 导入数据，并观察目录结果变化

```bash
load data local inpath '/course/hive/hive_op/student.txt' into table stu_ptn partition(depart='JC');
select * from stu_ptn; 
```

分析结果产生的原因

![](/assets/images/QqVcDzIex/1638689121931.png)

#### 6.3.3 删除分区（观察hfds目录/user/hive/warehouse/stu_ptn结构变化）

```bash
alter table stu_ptn drop partition(depart='JC');
```

#### 6.3.4 通过查询语句向表中插入数据

```bash
insert overwrite table stu_ptn partition(depart='JC') select id,name,gender,age from stuinfo where depart='JC';
select * from stu_ptn;
```

![](/assets/images/QqVcDzIex/1638689132632.png)

#### 6.3.5 多插入模式

```bash
from stuinfo
insert overwrite table stu_ptn partition(depart='GS') select id,name,gender,age where depart='GS'
insert overwrite table stu_ptn partition(depart='WLW') select id,name,gender,age where depart='WLW'
insert overwrite table stu_ptn partition(depart='XG') select id,name,gender,age where depart='XG';
```

![](/assets/images/QqVcDzIex/1638689142123.png)

查看hdfs目录`/user/hive/warehouse/stu_ptn`结构变化 查询表中数据：`select * from stu_ptn;` 查看分区表对应的分区：`show partitions stu_ptn;` 添加分区：`alter table stu_ptn add partition(depart='WL');` 上诉是静态分区。 

#### 6.3.6 动态分区（自动分区模式）

##### 6.3.6.1 创建分区表（创建多个字段的分区表）

```bash
create table stu_ptn1(id int,name string,age int) partitioned by(depart string,gender string);
```

##### 6.3.6.2 设置参数

```bash
set hive.exec.dynamic.partition.mode=nonstrict;
set hive.exec.dynamic.partition=true;
```

##### 6.3.6.3 插入数据（观察目录结构变化）

```bash
insert overwrite table stu_ptn1 partition(depart,gender) select id,name,age,depart,gender from stuinfo;
```

##### 6.3.6.4 说明

查询插入动态分区字段需要放在最后一个字段，否则会将最后一个字段默认为分区字段 多字段分区的时候，目录结构按照分区字段顺序进行划分 分区表使用的时候，分区字段和普通字段一样，只是在建表和添加数据的时候不一样

### 6.4 分桶表的基本操作

#### 6.4.1 创建分桶表

```bash
create table if not exists stu_buk(
id int,
name string,
gender string,
age int,
depart string
)clustered by (age) into 3 buckets;
```

#### 6.4.2 开启分桶功能

```bash
set hive.enforce.bucketing=true;
```

### 6.5 录入数据

```bash
insert into table stu_buk select id,name,gender,age,depart from stuinfo;
或
insert overwrite table stu_buk select * from stuinfo;
insert overwrite table stu_buk select * from stuinfo cluster by age;（没有开启分桶功能，但这是reducetask为3个即设置 mapreduce.job.reduces=3）
```

![](/assets/images/QqVcDzIex/1638689154401.png)

### 6.6 数据导出

#### 6.6.1 将查询的结果导出到本地

##### 6.6.1.1 单重导出

```bash
insert overwrite local directory '/course/hive/hive_op/hive-data/stuinfo' select * from stuinfo;
```

一个或多个文件会被写入到`/course/hive/hive_op/hive-data/stuinfo`目录下，具体个数取决于调用的`reducer`个数

![](/assets/images/QqVcDzIex/1638689172297.png)

##### 6.6.1.2 多重导出

```bash
from stuinfo insert overwrite local directory '/course/hive/hive_op/hive-data/stu-18' select * where age=18
insert overwrite local directory '/course/hive/hive_op/hive-data/stu-19' select * where age=19;
```

注意：数据写入到文件系统时进行文本序列化，且每列用`^A`来分隔，`\n`来换行。

![](/assets/images/QqVcDzIex/1638689181151.png)

#### 6.6.2 将查询的结果格式化导出到本地

```bash
insert overwrite local directory '/course/hive/hive_op/hive-data/stu_ptn_jc'
row format delimited fields terminated by ',' lines terminated by '\n'
select * from stu_ptn where depart='JC';
```

![](/assets/images/QqVcDzIex/1638689189852.png)

#### 6.6.3 将查询的结果导出到 HDFS 上(没有 local)

```bash
insert overwrite directory '/hive/stu_ptn_gs'
row format delimited fields terminated by ',' lines terminated by '\n'
select * from stu_ptn where depart='GS';
```

![](/assets/images/QqVcDzIex/1638689198673.png)

#### 6.6.4 Hadoop 命令导出到本地

在**master**节点上执行如下命令（不是在hive控制台执行该命令，导出前确保本地`/course/hive/hive_op/hive-data/000000_0`文件不存在）

```bash
hadoop fs -get /hive/stu_ptn_jc/000000_0 /course/hive/hive_op/hive-data/;
```

#### 6.6.5 export导出到hdfs

```bash
export table stu_ptn to '/hive/stu_ptn';
```

整个表的数据及元数据都导出到hdfs的`/hive/stu_ptn/`目录下。

![](/assets/images/QqVcDzIex/1638689210114.png)
