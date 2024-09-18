## 八.视图

>视图是一种虚拟表 ，本身是不具有数据，占用很少的内存空间

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

MySQL支持使用INSERT、UPDATE和DELETE语句对视图中的数据进行插入、更新和删除操作。当视图中的数据发生变化时，数据表中的数据也会发生变化，反之亦然

但是以下情况视图不可更新
- 在定义视图的时候**指定了“algorithm= temptable”**，视图将不支持insert和delete操作
- 视图中**不包含基表中所有被定义为非空又未指定默认值的列**，视图将不支持insert操作
- 在定义视图的select语句中**使用了join联合查询** ，视图将不支持insert和delete操作
- 在定义视图的select语句后的字段列表中**使用了数学表达式或子查询** ，视图将不支持insert, 也不支持update
- 在定义视图的SELECT语句后的字段列表中**使用dinsinct 、 聚合函数 、group by 、 having 、 union等**，视图将不支持insert、update、delete；
- 在定义视图的select语句中**包含了子查询，而子查询中引用了from后面的表**，视图将不支持 insert、update、delete

## 九.存储过程和存储函数

### 1.变量

>变量分为**系统变量**以及**用户自定义变量**

#### 1.1系统变量

>系统变量分为全局系统变量（需要添加 global 关键字）以及会话系统变量（需要添加 session 关键字), 如果不写，默认会话级别
>每一个MySQL客户机成功连接MySQL服务器后，都会产生与之对应的会话 , 会话期间，MySQL服务实例会在MySQL服务器内存中生成与该会话对应的会话系统变量，这些会话系统变量的初始值是全局系统变量值的复制

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

![](https://quan-blog-1321822882.cos.ap-shanghai.myqcloud.com/2023/11/18/1700321465719.jpg)


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

![](https://quan-blog-1321822882.cos.ap-shanghai.myqcloud.com/2023/11/18/1700321475173.jpg)


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

![](https://quan-blog-1321822882.cos.ap-shanghai.myqcloud.com/2023/11/18/1700321489683.jpg)


例2 : 实现累加运算，计算 1+2+…+n 等于多少

![](https://quan-blog-1321822882.cos.ap-shanghai.myqcloud.com/2023/11/18/1700321499816.jpg)


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

![](https://quan-blog-1321822882.cos.ap-shanghai.myqcloud.com/2023/11/18/1700321511024.jpg)


## 十.游标

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

![](https://quan-blog-1321822882.cos.ap-shanghai.myqcloud.com/2023/11/18/1700321521678.jpg)


## 十一.触发器

>触发器是由事件来触发某个操作
>所谓事件就是指用户的动作或者触发某项行为, 如果定义了触发程序，当数据库执行这些语句时候，就相当于事件发生了，就会自动激发触发器执行相应的操作

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

![](https://quan-blog-1321822882.cos.ap-shanghai.myqcloud.com/2023/11/18/1700321534790.jpg)


上面触发器声明过程中的NEW关键字代表INSERT添加语句的新记录


## 十.事务

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
