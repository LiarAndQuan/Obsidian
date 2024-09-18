## 一.数据库相关概念

**DB：数据库（Database）**
即存储数据的“仓库”，其本质是一个文件系统。它保存了一系列有组织的数据。

**DBMS：数据库管理系统（Database Management System）**
是一种操纵和管理数据库的大型软件，用于建立、使用和维护数据库，对数据库进行统一管理和控 制。用户通过数据库管理系统访问数据库中表内的数据。

**SQL：结构化查询语言（Structured Query Language）**: 专门用来与数据库通信的语言

SQL语言功能可以分为以下四类 :
- **DDL（Data Definition Languages 数据定义语言）** 这些语句定义了不同的数据库、表、视图、索引等数据库对象，还可以用来创建、删除、修改数据库和数据表的结构
- **DML（Data Manipulation Language 数据操作语言）** 用于添加、删除、更新和查询数据库记录，并检查数据完整性
- **DQL(Data Query Language 数据查询语言)** 用于查询数据库中表的记录
- **DCL（Data Control Language 数据控制语言）** 用于定义数据库、表、字段、用户的访问权限和安全级别

**四个默认数据库 :** 
- **“information_schema”** 主要保存 MySQL 数据库服务器的系统信息， 比如数据库的名称、数据表的名称、字段名称、存取权限、数据文件所在的文件夹和系统使用的文件夹，等等 
- **“performance_schema”** 可以用来监控 MySQL 的各类性能指标。 
- **“sys”** 主要作用是以一种更容易被理解的方式展示 MySQL 数据 库服务器的各类性能指标，帮助系统管理员和开发人员监控 MySQL 的技术性能。 
- **“mysql”** 数据库保存了 MySQL 数据库服务器运行时需要的系统信息，比如数据文件夹、当前使用的 字符集、约束检查信息，等等

## 二.数据类型

### 1.数值类型

|    类型     | 大小(byte) | 有符号范围                                             | 无符号范围                                                 |
|:-----------:|:----------:|:------------------------------------------------------ |:---------------------------------------------------------- |
|   tinyint   |     1      | (-128,127)                                             | (0,255)                                                    |
|  smallint   |     2      | (-32768，32767)                                        | (0，65535)                                                 |
|  mediumint  |     3      | (-8388608，8388607)                                    | (0，16777215)                                              |
| int/integer |     4      | (-2147483648， 2147483647)                             | (0，4294967295)                                            |
|   bigint    |     8      | (-2^63，2^63-1)                                        | (0，2^64-1)                                                |
|    float    |     4      | (-3.402823466 E+38， 3.402823466351 E+38)              | 0 和 (1.175494351 E38，3.402823466 E+38)                  |
|   double    |     8      | (-1.7976931348623157 E+308， 1.7976931348623157 E+308) | 0 和 (2.2250738585072014 E-308， 1.7976931348623157 E+308) |
|   decimal   |            | 依赖于M(精度)和D(标度) 的值                            | 依赖于M(精度)和D(标度)的 值                                |

### 2.字符串类型

|    类型    |  大小(byte)  |    描述    |
|:----------:|:------------:|:----------:|
|    char    |    0-255     | 定长字符串 |
|  varchar   |   0-65535    | 变长字符串 |
|  tinyblob  |    0-255     | 二进制数据 |
|  tinytext  |    0-255     |   短文本   |
|    blob    |   0-65535    |            |
|    text    |   0-65535    |            |
| mediumblob |  0-16777215  |            |
| mediumtext |  0-16777215  |            |
|  longblob  | 0-4394967295 |            |
|  longtext  | 0-4394967295 |            |

>char 与 varchar 都可以描述字符串，char是定长字符串，指定长度多长，就占用多少个字符，和 字段值的长度无关 。而varchar是变长字符串，指定的长度为最大占用长度 。相对来说，char的性能会更高些

### 3.日期时间类型

|   类型    | 大小(byte) | 范围                                       | 格式                |
|:---------:|:----------:| ------------------------------------------ | ------------------- |
|   date    |     3      | 1000-01-01 至 9999-12-31                   | YYYY-MM-DD          |
|   time    |     3      | -838:59:59 至 838:59:59                    | HH:MM:SS            |
|   year    |     1      | 1901 至 2155                               | YYYY                |
| datetime  |     8      | 1000-01-01 00:00:00 至 9999-12-31 23:59:59 | YYYY-MM-DD HH:MM:SS |
| timestamp |     4      | 1970-01-01 00:00:01 至 2038-01-19 03:14:07 | YYYY-MM-DD HH:MM:SS |


## 三.DDL

### 1.操作数据库

```mysql
#创建数据库
create database if not exists 数据库名 character set 字符集;

#查看当前所有的数据库
show databases;

#查看当前正在使用的数据库,使用的一个 mysql中的全局函数
select database();

#查看指定库下所有的表
show tables from 数据库名;

#查看数据库的创建信息
show create database 数据库名;
desc 数据库名;

#使用或者切换数据库
use 数据库名;

#更改数据库字符集
alter database 数据库名 character set 字符集;

#删除数据库
drop database if exists 数据库名;
```

### 2.操作表

```mysql
#创建空表
create table [if not exists] 表名(
	字段1, 数据类型 [约束条件] [默认值],
	字段2, 数据类型 [约束条件] [默认值],
	字段3, 数据类型 [约束条件] [默认值],
	……
	[表约束条件]
);

#将查询结果作为新表的数据
create table 表名 as select ...;

#查看表数据结构
show create table 表名;
dese 表名;

#追加一个列
alter table 表名 add 字段名 字段类型 [first|after 字段名];

#修改一个列
alter table 表名 modify 字段名 新字段类型 [default] [first|after 字段名];

#重命名一个列
alter table 表名 change 列名 新列名 新数据类型;

#删除一个列
alter table 表名 drop 字段名

#重命名表
alter table 表名 rename to 新表名;
rename table 表名 to 新表名;

#删除表
drop table if exists 数据表1 [,数据表2 ..., 数据表n];

#清空表
truncate table 表名;  #不能回滚
delete from 表名;  #可以回滚
```

## 四.DML

```mysql
#插入数据
insert into 表名(column1 [, column2, ..., columnn])
values
(value1 [, value2, ..., valuen]),
(value1 [, value2, ..., valuen]),
...
(value1 [, value2, ..., valuen]);

#将查询结果插入表中
insert into 目标表名(column1 [, column2, ...,columnn])
select (column1 [, column2, ...,columnn]) [from ... where ...];

#更新数据
update 表名
set column1 = value1 ,column2 = value2, ..., cloumnn = valuen;
[where ...];

#删除数据
delete from 表名
[where ...];
```

## 五.DCL

### 1.用户控制

```mysql
#查询用户,Host代表可以访问的主机,可以使用%通配所有ip
select * from mysql.user;
#创建用户
create user '用户名'@'主机名' identified by '密码';
#修改用户密码
alter user '用户名'@'主机名' identified with mysql_native_password by '新密码';
#删除用户
drop user '用户名'@'主机名'
```

### 2.权限控制

**常用权限 :** 

1. all,all privilege
2. select
3. insert
4. update
5. delete
6. alter
7. drop
8. create

```mysql
#查询权限
show grants for '用户名'@'主机名';
#授予权限
grant 权限列表 on 数据库名.表名 to '用户名'@'主机名';
#撤销权限
revoke 权限列表 on 数据库名.表名 to '用户名'@'主机名';
```

## 六.函数

### 1.字符串函数

>mysql中字符串下标从1开始

1. concat(s1,s2,...sn)
2. lower(str)
3. upper(str)
4. lpad(str,n,pad_char) : 左填充
5. rpad(str,n,pad_char) : 右填充
6. trim(str)
7. substring(srt,start,len)

### 2.数值函数

1. ceil(x)
2. floor(x)
3. mod(x,y)
4. rand() : 返回0-1内的随机数
5. round(x,y) : 对x四舍五入,保留y位

### 3.日期函数

1. curdate()
2. curtime()
3. now()
4. year(date)
5. month(date)
6. day(date)
7. date_add(date,interval_date)
8. datediff(date1,date2)

## 七. select ... from ... where的增强之路

### 1.as

>as可以重命名一个列, 紧跟列名，也可以在列名和别名之间加入关键字AS，别名使用双引号，以便在别名中包含空格或特 殊的字符并区分大小写,AS 可以省略

![[Attachments/Images/Pasted image 20230423160202.png]]

### 2.distinct

>distinct可以去除重复行, 其实是对后面所有列名的组合进行去重, 需要放在所有列名的前面

![[Attachments/Images/Pasted image 20230423143917.png]]

### 3.order by

>使用order by排序, order by在字句的末尾, 可以对多列进行排序
>- asc(ascend): 升序, 默认
>- desc(descend): 降序

![[Attachments/Images/Pasted image 20230423150115.png]]

### 4.limit

>格式: limit \[位置偏移量 , ]  行数
>MySQL 8.0中可以使用“limit 3 offset 4”，意思是获取从第5条记录开始后面的3条记录，和“limit 4,3;”返回的结果相同
>limit字句必须放在整个select语句后面

![[Attachments/Images/Pasted image 20230423150659.png]]

### 5.group by与having和聚合函数

**常用的聚合函数有 :** 

1. avg
2. sum
3. min
4. max
5. count

>可以使用group by字句将表中的数据分为若干组
>可以根据多个字段分组

![[Attachments/Images/Pasted image 20230423162844.png]]

>group by中使用with rollup可以在查询出的分组记录之后增加一条记录, 该记录计算查询出的所有记录的总和
>当使用ROLLUP时，不能同时使用ORDER BY子句进行结果排序，即ROLLUP和ORDER BY是互相排斥的

![[Attachments/Images/Pasted image 20230423163745.png]]

>having过滤分组:
>- 行已经被分组
>- 使用了聚合函数

![[Attachments/Images/Pasted image 20230423164329.png]]

### 6.inner join

>内连接: 合并具有同一列的两个以上的表的行, 结果集中不包含一个表与另一个表不匹配的行
>
>select 字段列表
>from a inner join b
>on a.c = b.c

![[Attachments/Images/Pasted image 20230423153148.png]]

### 7.left outer join , right outer join

>两个表在连接过程中除了返回满足连接条件的行以外还返回左（或右）表中不满足条件的 行 ，这种连接称为左（或右） 外连接。没有匹配的行时, 结果表中相应的列为空(NULL)

>有些员工没有部门

![[Attachments/Images/Pasted image 20230423153927.png]]

>有些部门没有员工

![[Attachments/Images/Pasted image 20230423154004.png]]

### 8.union和union all

>利用UNION关键字，可以给出多条SELECT语句，并将它们的结果组合成单个结果集
>合并时，两个表对应的列数和数据类型必须相同，并且相互对应,  各个SELECT语句之间使用UNION或UNION ALL关键字分隔
>union: 返回两个查询的结果集的并集，去除重复记录
>union all: 返回两个查询的结果集的并集。对于两个结果集的重复部分，不去重

![[Attachments/Images/Pasted image 20230423155422.png]]

其实相当于select * from employees  where email like '%a%' or department_id>90;

![[Attachments/Images/Pasted image 20230423160109.png]]




## 八.子查询

### 1.标量子查询

>子查询返回的结果是单个值（数字、字符串、日期等），最简单的形式，这种子查询称为标量子查询
>例 : 返回job_id与141号员工相同，salary比143号员工多的员工姓名，job_id和工资

![[Attachments/Images/Pasted image 20230423170024.png]]

### 2.列子查询

>返回多行单列结果, 使用多行比较操作符

|  操作符   |                          含义                           |
|:---------:|:-------------------------------------------------------:|
|    in/not in     |                        任意一个                         |
| any /some | 需要和单行比较操作符一起使用,和子查询返回的某一个值比较 |
|    all    | 需要和单行比较操作符一起使用，和子查询返回的所有值比较  |
|           |                                                         |

例: 返回其它job_id中比job_id为‘IT_PROG’部门任一工资低的员工的员工号、姓名、job_id 以及salary

![[Attachments/Images/Pasted image 20230423171336.png]]

### 3.行子查询

>子查询返回的结果是一行（可以是多列），这种子查询称为行子查询。 常用的操作符：= 、<> 、IN 、NOT IN

```mysql
select * from emp where (salary,managerid) = (select salary, managerid from emp where name = '张无忌');
```

### 4.表子查询

>子查询返回的结果是多行多列，这种子查询称为表子查询

```mysql
select * from emp where (job,salary) in ( select job, salary from emp where name = '鹿杖客' or name = '宋远桥' );
```

### 5.相关子查询

>如果子查询的执行依赖于外部查询，通常情况下都是因为子查询中的表用到了外部的表，并进行了条件关联，因此每执行一次外部查询，子查询都要重新计算一次，这样的子查询就称之为关联子查询

例: 查询员工中工资大于本部门平均工资的员工的last_name,salary和其department_id

![[Attachments/Images/Pasted image 20230423172718.png]]

还可以在from中使用子查询

![[Attachments/Images/Pasted image 20230423173534.png]]

>from型的子查询：子查询是作为from的一部分，子查询要用()引起来，并且要给这个子查询取别名， 把它当成一张“临时的虚拟的表”来使用

### 6.exists和not exist

>关联子查询通常也会和exists操作符一起来使用，用来检查在子查询中是否存在满足条件的行
>not exist关键字表示如果不存在某种条件，则返回true，否则返回false

例: 查询公司管理者的employee_id，last_name，job_id，department_id信息

![[Attachments/Images/Pasted image 20230423174845.png]]

例: 查询departments表中，不存在于employees表中的部门的department_id和department_name

![[Attachments/Images/Pasted image 20230423175026.png]]

## 九.约束

### 1.not null

>限定某个字段/某列的值不允许为空

```mysql
#建表时添加非空约束
create table 表名(
	字段名 数据类型 not null
)

#建表后添加非空约束,相当于修改列
alter table 表名 modify 字段名 数据类型 not null;

#删除非空约束,同样相当于修改列
alter table 表名 modify 字段名 数据类型;
```

### 2.unique

>用来限制某个字段/某列的值不能重复
>
>特点:
>- 唯一约束可以是某一个列的值唯一, 也可以多个列组合的值唯一
>- MySQL会给唯一约束的列上默认创建一个唯一索引
>- 如果创建唯一约束时未指定名称，如果是单列，就默认和列名相同；如果是组合列，那么默认和( ) 中排在第一个的列名相同。也可以自定义唯一性约束名

```mysql
#建表时添加唯一约束
create table 表名(
	字段名 数据类型 unique, #单列
	[constraint 约束名] unique (字段名1 [, 字段名2 , ..., 字段名n]);
)

#建表后指定唯一约束
alter table 表名 add unique (字段名1 [, 字段名2 , ..., 字段名n]);
alter table 表名 modify 字段名 字段类型 unique; #相当于修改列

#删除唯一约束
alter table 表名 drop index 唯一约束索引名;
```

### 3.primary key

>用来唯一标识表中的一行记录
>
>特点:
>- 主键约束相当于唯一约束+非空约束的组合，主键约束列不允许重复，也不允许出现空值
>- 一个表最多只能有一个主键约束
>- 主键约束对应着表中的一列或者多列（复合主键)
>- MySQL的主键名总是primary

```mysql
#建表时添加主键
create table 表名(
	字段名 数据类型 primary key,
	[constraint 约束名] primary key (字段名1 [, 字段名2 , ..., 字段名n]);
)

#建表后添加主键
alter table 表名 add primary key (字段名1 [, 字段名2 , ..., 字段名n]);

#删除主键
alter table 表名 drop primary key;
```

### 4.auto_increment

>某个字段的值自增
>特点:
>- 一个表最多只能有一个自增长列
>- 自增长列约束的列必须是键列（主键列，唯一键列）
>- 自增约束的列的数据类型必须是整数类型

```mysql
#建表时添加auto_increment
create table 表名(
	字段名 数据类型 primary key|unique auto_increment,
)

#建表后添加,相当于修改列
alter table 表名 modify 字段名 数据类型 auto_increment;

#删除自增约束,相当于修改列
alter table 表名 modify 字段名 数据类型;
```

### 5.foreign key

>限定某个表的某个字段的引用完整性
>
>特点:
>- 从表的外键列，必须引用/参考主表的主键或唯一约束的列
>- 在创建外键约束时，如果不给外键约束命名，默认名不是列名，而是自动产生一个外键名
>- 当主表的记录被从表参照时，主表的记录将不允许删除，如果要删除数据，需要先删除从表中依赖该记录的数据，然后才可以删除主表的数据
>- 在“从表”中指定外键约束，并且一个表可以建立多个外键约束
>- 当创建外键约束时，系统默认会在所在的列上建立对应的普通索引, 索引名是外键的约束名
>- 删除外键约束后, 必须手动删除对应的索引

```mysql
#建表时添加外键约束
create table 表名(
	字段 数据类型,
	[constraint 外键约束名称] foreign key (从表的某个字段) references 主表名(被参考字段)
)

#建表后添加外键约束
alter table 从表名 add [constraint 约束名] foreign key (从表字段) references 主表名(被参考字段)

#删除外键约束,分为两步
alter table drop foreign key 约束名;
alter table drop index 索引名;
```

约束等级
- cascade : 在父表上update/delete记录时，同步update/delete掉子表的匹配记录
- set null : 在父表上update/delete记录时，将子表上匹配记录的列设为null，但是要注意子 表的外键列不能为not null
- no action : 如果子表中有匹配的记录，则不允许对父表对应候选键进行update/delete操作
- restrict(默认等级) : 同no action， 都是立即检查外键约束
- set default : 父表有变更时，子表将外键列设置成一个默认的值
- 对于外键约束,最好采用 on update cascade on delete restrict

### 6.check

>检查某个字段的值是否符号xx要求，一般指的是值的范围

```mysql
#建表时添加check约束
create table 表名(
	字段名 字段类型 check(条件)
)

#建表后添加check约束,相当于修改表
alter table 表名 add [constraint 约束名] check(约束条件);

#删除check约束
alter table 表名 drop check 约束名;
```

### 7.default

>给某个字段/某列指定默认值，一旦设置默认值，在插入数据时，如果此字段没有显式赋值，则赋值为默 认值

```mysql
#建表时添加default
create table 表名(
	字段名 数据类型 default 默认值
)

#建表后添加default,相当于修改列
alter table 表名 modify 字段名 数据类型 default 默认值

#删除default,相当于修改列
alter table 表名 modify 字段名 数据类型
```

## 十.索引

### 1.主要索引结构

|      索引结构       | 描述                                                                            |
|:-------------------:| ------------------------------------------------------------------------------- |
|     B+Tree     | 最常见的索引类型，大部分引擎都支持 B+ 树索引                                    |
|        Hash         | 底层数据结构是用哈希表实现的, 只有精确匹配索引列的查询才有效, 不 支持范围查询   |
|  R-tree(空间索引)   | 空间索引是MyISAM引擎的一个特殊索引类型，主要用于地理空间数据类 型，通常使用较少 |
| Full-text(全文索引) | 是一种通过建立倒排索引,快速匹配文档的方式。类似于 Lucene,Solr,ES                |

不同的存储引擎对于索引结构的支持情况不同 :

|   索引    |   InnoDB    | MyISAM | Memory |
|:---------:|:-----------:|:------:|:------:|
|  B+tree   |    支持     |  支持  |  支持  |
|   Hash    |   不支持    | 不支持 |  支持  |
|  R-tree   |   不支持    |  支持  | 不支持 |
| Full-text | 5.6之后支持 |  支持  | 不支持 |

>我们平常所说的索引，如果没有特别指明，都是指B+树结构组织的索引

### 2.B+Tree结构

#### 1.BTree

>B树是一种多叉路衡查找树，相对于二叉树，B树每个节点可以有多个分支，即多叉。 以一颗最大度数（max-degree）为5(5阶)的b-tree为例，那这个B树每个节点最多存储4个key，5 个指针

![[Attachments/Images/Pasted image 20231209134752.png]]

特点 :

1. 5阶的B树，每一个节点最多存储4个key，对应5个指针
2. 一旦节点存储的key数量到达5，就会裂变，中间元素向上分裂
3. 在B树中，非叶子节点和叶子节点都会存放数据

#### 2.B+Tree

>我们以一颗最大度数（max-degree）为4（4阶）的b+tree为例，来看一 下其结构示意图：

![[Attachments/Images/Pasted image 20231209135004.png]]

B+Tree 与 B-Tree相比，主要有以下三点区别：

1. 所有的数据都会出现在叶子节点
2. 叶子节点形成一个单向链表
3. 非叶子节点仅仅起到索引数据作用，具体的数据都是在叶子节点存放的

#### 3.Mysql优化后的B+Tree

>MySQL索引数据结构对经典的B+Tree进行了优化。在原B+Tree的基础上，增加一个指向相邻叶子节点 的链表指针，就形成了带有顺序指针的B+Tree，提高区间访问的性能，利于排序

![[Attachments/Images/Pasted image 20231209135202.png]]

### 3.索引分类

在MySQL数据库，将索引的具体类型主要分为以下几类：主键索引、唯一索引、常规索引、全文索引 :

| 分类     | 含义 | 特点 | 关键字 |
| -------- | ---- | ---- | ------ |
| 主键索引 | 针对于表中主键创建的索引 |默认自动创建, 只能 有一个 |PRIMARY  |
| 唯一索引 |避免同一个表中某数据列中的值重复 |可以有多个 |UNIQUE|
| 常规索引 | 快速定位特定数据 |  可以有多个 |        |
| 全文索引         |全文索引查找的是文本中的关键词，而不是比 较索引中的值 |可以有多个  |FULLTEXT  |

而在在InnoDB存储引擎中，根据索引的存储形式，又可以分为以下两种：

| 分类                      | 含义                                                        | 特点                 |
| ------------------------- | ----------------------------------------------------------- | -------------------- |
| 聚集索引(Clustered Index) | 将数据存储与索引放到了一块，索引结构的叶子 节点保存了行数据 | 必须有,而且只 有一个 |
| 二级索引(Secondary Index)    | 将数据与索引分开存储，索引结构的叶子节点关 联的是对应的主键           | 可以存在多个     |

聚集索引选取规则 :

1. 如果存在主键，主键索引就是聚集索引
2. 如果不存在主键，将使用第一个唯一（UNIQUE）索引作为聚集索引
3. 如果表没有主键，或没有合适的唯一索引，则InnoDB会自动生成一个rowid作为隐藏的聚集索引

![[Attachments/Images/Pasted image 20231209140002.png]]

### 4.索引语法

```mysql
#创建索引
create [unique|fulltext] index index_name on table_name(index_col_name,...);
#查看索引
show index from table_name;
#删除索引
drop index index_name on table_name;
```

## 十一.事务

### 1.事务的四大特性

1. 原子性(Atomicity) : 事务是不可分割的最小操作单元，要么全部成功，要么全部失败。
2. 一致性(Consistency) : 事务完成时，必须使所有的数据都保持一致状态。
3. 隔离性(Isolation) : 数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立 环境下运行。
4. 持久性(Durability) : 事务一旦提交或回滚，它对数据库中的数据的改变就是永久的。

### 2.并发事务问题

1. 脏读 : 一个事务读到另一个事务还没有提交的数据
2. 不可重复读 : 一个事务先后读取同一条记录，但两次读取的数据不同，称之为不可重复读
3. 幻读 : 一个事务按照条件查询数据时，没有对应的数据行，但是在插入数据时，又发现这行数据已经存在

### 3.事务隔离级别

为了解决并发事务所引发的问题，在数据库中引入了事务隔离级别。主要有以下几种：

|       隔离级别        | 脏读 | 不可重复读 | 幻读 |
|:---------------------:|:----:|:----------:|:----:|
|   Read uncommitted    |  √   |     √      |  √   |
|    Read committed     |  ×   |     √      |  √   |
| Repeatable Read(默认) |  ×   |     ×      |  √   |
|     Serializable      |  ×   |     ×      |  ×   |

### 4.事务的几个有关命令

```mysql
#查看事务提交方式
select @@autocommit;
#设置提交方式
set @@autocommit = 0;
#提交事务
commit;
#回滚事务
rollback;
#查看事务隔离级别
select @@transaction_isolation
#设置事务隔离级别
set [session | global] transaction isolation level ...
```

## 十二. 视图

>视图是一种虚拟表 ，本身是不具有数据，占用很少的内存空间 , 只保留了查询逻辑

### 1.视图命令

```mysql
#创建视图
create [or replace]
[algorithm = {undefined|merge|temptable}]
view 视图名 [(字段列表)]
as select...
[with [cascaded|local] check option]

#查看视图详细定义信息
show create view 视图名称;

#修改视图,也可以create or replace
alter view 视图名 as select ...;

#删除视图
drop view if exists 视图名1 [, 视图名2, ..., 视图名n];
```

### 2.视图的更新

MySQL支持使用INSERT、UPDATE和DELETE语句对视图中的数据进行插入、更新和删除操作。当视图中的数据发生变化时，数据表中的数据也会发生变化，反之亦然

但是以下情况视图不可更新
- 在定义视图的时候**指定了“algorithm= temptable”**，视图将不支持insert和delete操作
- 视图中**不包含基表中所有被定义为非空又未指定默认值的列**，视图将不支持insert操作
- 在定义视图的select语句中**使用了join联合查询** ，视图将不支持insert和delete操作
- 在定义视图的select语句后的字段列表中**使用了数学表达式或子查询** ，视图将不支持insert, 也不支持update
- 在定义视图的SELECT语句后的字段列表中**使用dinsinct 、 聚合函数 、group by 、 having 、 union等**，视图将不支持insert、update、delete；
- 在定义视图的select语句中**包含了子查询，而子查询中引用了from后面的表**，视图将不支持 insert、update、delete

### 3.local和cascaded

>当使用WITH CHECK OPTION子句创建视图时，MySQL会通过视图检查正在更改的每个行，例如 插 入，更新，删除，以使其符合视图的定义。 MySQL允许基于另一个视图创建视图，它还会检查依赖视 图中的规则以保持一致性。为了确定检查的范围，mysql提供了两个选项： CASCADED 和 LOCAL ，默认值为 CASCADED

1. cascaded

比如，v2视图是基于v1视图的，如果在v2视图创建的时候指定了检查选项为 cascaded，但是v1视图 创建时未指定检查选项。 则在执行检查时，不仅会检查v2，还会级联检查v2的关联视图v1

2. local

比如，v2视图是基于v1视图的，如果在v2视图创建的时候指定了检查选项为 local ，但是v1视图创建时未指定检查选项。 则在执行检查时，知会检查v2，不会检查v2的关联视图v1

## 十三. 存储过程 , 存储函数和游标

### 1.变量

>变量分为**系统变量**以及**用户自定义变量**

#### 1.1系统变量

>系统变量分为全局系统变量（需要添加 global 关键字）以及会话系统变量（需要添加 session 关键字), 如果不写，默认会话级别 , 会话系统变量的初始值是全局系统变量值的复制

```mysql
#查看所有全局|会话变量
show global | session variables;

#查看满足条件的部分系统|会话变量
show global | session variables like '% %';

#查看指定的系统|会话变量的值
select @@global.变量名 | @@session.变量名;
select @@变量名;

#修改系统/会话变量的值,只会临时生效
set @@global.变量名 | @@session.变量名 = value;
set global | session 变量名 = value;
```

>使用set global语句设置的变量值只会临时生效
>mysql8.0新增了set persist命令 , mysql会将该命令的配置保存到数据目录下的mysqld-auto.cnf文件中, 下次启动时会读取该文件 ,实现全局变量的持久化

#### 1.2用户变量

>MySQL 中的用户变量以 一个“@” 开头。根据作用范围不同，又分为会话用户变量和局部变量
>- 会话用户变量：作用域和会话变量一样，只对当前连接会话有效
>- 局部变量：只在 BEGIN 和 END 语句块中有效。局部变量只能在存储过程和函数中使用

```mysql
#定义会话用户变量
set @变量名 = value;
select @变量名 = 表达式 [from ...];
select 表达式 into @变量名 [from ...];

#查看会话用户变量
select @变量名;

#在存储过程/函数中定义局部变量
declare 变量名 数据类型 [default 默认值];

#为局部变量赋值 
set 变量名 = value; 
select 表达式 into 变量名 [FROM ...];

#查看局部变量
select @变量名;
```

### 2.定义条件与处理程序

>定义条件是事先定义程序执行过程中可能遇到的问题
>处理程序定义了在遇到问题时应当采取的处理方式，并且保证存储过程或函数在遇到警告或错误时能继续执行

#### 2.1定义条件

>定义条件就是给MySQL中的错误码命名，这有助于存储的程序代码更清晰。它将一个错误名字和指定的错误条件 关联起来。这个名字可以随后被用在定义处理程序的 declare handler语句中

```mysql
#定义条件
declare 错误名称 condition for 错误码（或错误条件）
```

错误码的说明 : 
- MySQL_error_code 和 sqlstate_value 都可以表示MySQL的错误
	- MySQL_error_code是数值类型错误代码
	- sqlstate_value是长度为5的字符串类型错误代码
- 在ERROR 1418 (HY000)中，1418是MySQL_error_code，'HY000'是sqlstate_value

![[Attachments/Images/Pasted image 20230426233852.png]]

#### 2.2处理程序

>可以为SQL执行过程中发生的某种类型的错误定义特殊的处理程序

```mysql
#定义处理程序
declare 处理方式 handler for 错误类型 处理语句;
```

说明 : 
- 处理方式有三个取值 : 
	- continue : 遇到错误继续执行
	- exit : 遇到错误退出
	- undo : 遇到错误撤销之前的操作
- 错误类型 : 
	- sqlstate '字符串错误码' : 表示长度为5的sqlstate_value类型的错误代码
	- MySQL_error_code ：匹配数值类型错误代码
	- 错误名称 ：表示declare ... condition定义的错误条件名称
	- sqlwarning ：匹配所有以01开头的sqlstate错误代码
	- not found ：匹配所有以02开头的sqlstate错误代码
	- sqlexception ：匹配所有没有被sqlwarning或not found捕获的sqlstate错误代码
- 处理语句 : 如果出现上述条件之一，则采用对应的处理方式，并执行指定的处理语句。语句可以是 像“ SET 变量 = 值 ”这样的简单语句，也可以是使用 BEGIN ... END 编写的复合语句

![[Attachments/Images/Pasted image 20230426233711.png]]

### 3.流程控制

#### 3.1if

```mysql
if 条件1 then 操作1
[elseif 条件2 then 操作2]
...
[else 操作n]
end if;
```

#### 3.2case

```mysql
#用法1: 相当于switch
case 表达式
when value1 then 操作1
when value2 then 操作2
...
else 操作n
end case;

#用法2: 类似多重if
case
when 条件1 then 操作1
when 条件2 then 操作2
...
else 操作n
end case;
```

#### 3.3loop

>loop循环语句用来重复执行某些语句。loop内的语句一直重复执行直到循环被退出（使用leave子句），跳出循环过程 
>相当于while (true)

```mysql
[loop_label :] loop
...
end loop [loop_label];
```

#### 3.4while

>while语句创建一个带条件判断的循环过程。while在执行语句执行时，先对指定的表达式进行判断，如果为真，就执行循环内的语句，否则退出循环

```mysql
[while_label :] while 循环条件 do
...
end while [while_label];
```

#### 3.5repeat

>repeat语句创建一个带条件判断的循环过程 , 相当于do while

```mysql
[repeat_label :] repeat
...
until 条件
end repeat [repeat_label]
```

#### 3.6leave与iterate

>leave相当于break
>iterate相当于continue
>都需要在后面写上label实现跳转

### 4.存储过程

>一组经过预先编译 的 SQL 语句的封装

```mysql
#创建存储过程
delimiter 新的结束标记
create procedure 存储过程名(in|out|inout 参数名 参数类型, ...)
[characteristics ...]
begin
	...
end 新的结束标记
delimiter ;

#调用in模式的参数
call xx('参数');

#调用out模式的参数
set @name;
call xx(@name);
select @name; #查看值

#调用inout模式的参数
set @name = 'quan';
call xx(@name);
select @name

#查看创建信息
show create procedure 存储过程名;

#修改存储过程,只是修改相关特性
alter procedure 存储过程名 [characteristics ...];

#删除存储过程
drop procedure if exists 存储过程名;
```

说明:
- in : 当前参数为输入参数, 如果没有定义参数种类， 默认就是 IN ，表示输入参数
- out : 当前参数为输出参数, 执行完成之后，调用这个存储过程的客户端或者应用程序就可以读取这个参数返回的值了
- inout : 当前参数既可以为输入参数，也可以为输出参数
- characteristics 表示创建存储过程时指定的对存储过程的约束条件，其取值信息如下
	- **language sql** : 说明存储过程执行体是由SQL语句组成的，当前系统支持的语言为SQL
	- **\[not] deterministic** : 指明存储过程执行的结果是否确定 , deterministic表示结果是确定 的。每次执行存储过程时，相同的输入会得到相同的输出。not deterministicC表示结果是不确定 的，相同的输入可能得到不同的输出。如果没有指定任意一个值，默认为not Ddeterministic
	- **{ contains sql | no sql | reads sql data | modifies sql data }** : 指明子程序使用SQL语句的限制
		- contains sql(默认)表示当前存储过程的子程序包含SQL语句，但是并不包含读写数据的SQL语句
		- no sql表示当前存储过程的子程序中不包含任何SQL语句
		- reads sql data表示当前存储过程的子程序中包含读数据的SQL语句
		- modifies sql data表示当前存储过程的子程序中包含写数据的SQL语句
	- **sql security { definer | invoker }** : 执行当前存储过程的权限，即指明哪些用户能够执行当前存储过程
		- definer (默认) 表示只有当前存储过程的创建者或者定义者才能执行当前存储过程
		- invoker 表示拥有当前存储过程的访问权限的用户能够执行当前存储过程
	- **comment 'string'** : 注释信息
- delimiter : 因为MySQL默认的语句结束符号为分号‘;’。为了避免与存储过程中SQL语句结束符相冲突，需要使用 delimiter改变存储过程的结束符
- declare : 用来声明变量，使用的位置在于 BEGIN…END 语句中间，而且需要在其他语句使用之前进行变量的声明
- set : 对变量赋值
- select ... into ... : 将从数据表中查询的结果存放到变量中,也就是为变量赋值

例1 : 返回指定id的salary

![[Attachments/Images/Pasted image 20230425181049.png]]

例2 : 实现累加运算，计算 1+2+…+n 等于多少

![[Attachments/Images/Pasted image 20230425181434.png]]

### 5.存储函数

```mysql
#创建存储函数
delimiter $
create function 函数名(参数名 参数类型) #默认为in参数
returns 返回值类型
[characteristics ...]
begin
	...
	return ...
end $
delimiter ;

#调用存储函数
select 函数名(参数列表);

#查看创建信息
show create function 存储函数名;

#修改存储函数,只是修改相关特性
alter function 存储函数名 [characteristics ...];

#删除存储函数
drop function if exists 存储函数名;

```

例 : 创建存储函数count_by_id()，参数传入dept_id，该函数查询dept_id部门的员工人数，并返回，数据类型为整型

![[Attachments/Images/Pasted image 20230425182657.png]]

### 6.游标

>游标，提供了一种灵活的操作方式，让我们能够对结果集中的每一条记录进行定位，并对指向的记录中的数据进行操作的数据结构。游标让 SQL 这种面向集合的语言有了面向过程开发的能力

```mysql
#第一步,声明游标
declare cursor_name cursor for select_statement;

#打开游标
open cursor_name;

#使用游标,将此行查询结果写入变量中
fetch cursor_name into var_name [, var_name];

#关闭游标
close cursor_name;
```

例 : 创建存储过程“get_count_by_limit_total_salary( )”，声明IN参数 limit_total_salary，DOUBLE类型；声明 OUT参数total_count，INT类型。函数的功能可以实现累加薪资最高的几个员工的薪资值，直到薪资总和 达到limit_total_salary参数的值，返回累加的人数给total_count

![[Attachments/Images/Pasted image 20230427224044.png]]

## 十四. 触发器

>触发器是由事件来触发某个操作
>所谓事件就是指用户的动作或者触发某项行为, 如果定义了触发程序，当数据库执行这些语句时候，就相当于事件发生了，就会自动激发触发器执行相应的操作
>使用别名OLD和NEW来引用触发器中发生变化的记录内容，这与其他的数据库是相似的。现在触发器还 只支持行级触发，不支持语句级触发

```mysql
#创建触发器
create trigger 触发器名称
{before | after} {insert | update | delete} on 表名
for each row
begin
...
end;

#查看触发器
show triggers;
show create trigger;

#删除触发器
drop trigger if exists 触发器名称;
```

例 : 定义触发器“salary_check_trigger”，基于员工表“employees”的INSERT事件，在INSERT之前检查 将要添加的新员工薪资是否大于他领导的薪资，如果大于领导薪资，则报sqlstate_value为'HY000'的错 误，从而使得添加失败

![[Attachments/Images/Pasted image 20230427230705.png]]


