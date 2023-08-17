## MySQL基础介绍 和 基础语法

### 1. Mysql基础介绍

> MySQL 称为 **`DBMS`** 
>
> MySQL 属于一个 data base management System
>
> 是一个 **`数据库管理系统`** 类似一个软件 用于管理数据库
>
> 其他类似的数据库管理系统 还有 SQLserver ，Oracle 等

> **`数据库 DB (DataBase)`**
>
> 集合存储和管理数据的地方

> **`数据持久化`** 
>
> 数据存储在内存中，虽然效率更高  但是**`程序一旦奔溃或断电 数据就会丢失`**
>
> 将数据 写入可掉电的式的设备中 通常按一定格式写到文件中，才能永久保存
>
> **那么为什么不使用I/O流的方式来做持久化呢？**
>
> 通过I/O流操作硬盘上的文件读写效率太低。其实数据库表本身就是硬盘上的文件，但由于数据库表中的数据都是结构化的，所以通过数据库来操作效率高很多。

> MySQL的社区版 开源完全免费 
>
> **`MySQL 是关系型数据库`**
>
> 丰富的接口 

> MongoDB、Redis、Elasticsearch等是非关系型数据库管理系统。

> No-SQL 非关系数据库
>
> 非关系型数据库分为以下几类：
>
> - 键值存储 Redis
> - 文档数据库 MongoDB、Apache CouchDB
> - 宽列存储 Cassandra，Hbase，Scylla
> - 图形存储 Neo4j 

> 数据表的关系 
>
> - 一对一  
>
>   该关系中第一个表中的一个行只可以与第二个表中的一个行相关，且第二个表中的一个行也只可以与第一个表中的一个行相关
>
>   
>
> - 一对多  
>
>   第一个表中的一个行可以与第二个表中的一个或多个行相关，但第二个表中的一个行只可以与第一个表中的一个行相关。
>
>   
>
> - 多对多 
>
>   该关系中第一个表中的一个行可以与第二个表中的一个或多个行相关。第二个表中的一个行也可以与第一个表中的一个或多个行相关。通常两个表的多对多关系会借助第三张表，转换为两个一对多的关系。

### 2.SQL 

> SQL是 结构化查询语言, （Strutrue Query Language）, 专门用于操作/访问数据库的通过语言

#### 2.1 数据库基本操作

> 查看所有数据库

```mysql
show databases；
```

> 创建数据库 

```mysql
create database 数据库名字

create database db_Student
```

> 删除数据库

```mysql
drop database 数据库名字

drop database db_Student
```

> 使用自己的数据库

```mysql
use 数据库名字
use db_Student
```



#### 2.2 数据表的基本操作

> 显示数据表 名字

```mysql
show tables from 数据库名字
show tables from db_Student 

如果在之间 已经使用了 use 数据库
show tables 
```

> 创建数据库

```mysql
create table 数据库名.表名( 如果前面已经使用了数据 use db_Student 就不需要书写 数据库名.
	字段名 数据类型
    字段名 数据类型
);
```

```mysql
create table t_Stu(
	Stu_id int auto_increment PRIMRAY KEY
    Stu_name char(10)
);
```

> 查看定义好的表中的数据表结构

```mysql
desc 数据库名.表名;

desc t_Stu;
```

> 添加数据

```mysql
insert into 数据库名.表名 values(值列表);
insert into t_Stu values (1,"张三");
```

> 查看一个表的数据

```mysql
select * from 数据库名.表名  #表示查看所有的列
select * from t_Stu;
```

> 删除一个表

```mysql
drop table 数据库名.表名;
drop table t_Stu;
```



#### 2.3 SQL分类

> DDL 语句：数据定义语句(Data Define language)，例如：创建（create），修改（alter），删除（drop）等
>
> DML语句：数据操作语句，例如：增（insert)，删（delete），改（update），查（select）
>
> DQL语句：因为查询语句使用的非常的频繁，所以很多人把查询语句单拎出来一类
>
> DCL语句：数据控制语句，例如：grant，commit，rollback等
>
> 其他语句：USE语句，SHOW语句，SET语句等。这类的官方文档中一般称为命令。



> 1. SQL语法规范 sql语句 不区分大小写
>
> 数据库的表中的数据是否区分大小写。这个的话要看**`表格的字段的数据类型、编码方式以及校对规则`**。
>
> ci（大小写不敏感），cs（大小写敏感），_bin（二元，即比较是基于字符编码的值而与language无关，区分大小写）
>
> sql中的**`关键字`**，比如：create,insert等，不区分大小写。但是大家**`习惯上把关键字都“大写”`**。
>
> 2. 命名时：尽量使用26个英文字母大小写，数字0-9，下划线，不要使用其他符号，不要使用纯数字来命名
> 3. 建议不要使用mysql的关键字等来作为表名、字段名、数据库名等，
> 4. 建议不要使用mysql的关键字等来作为表名、字段名、数据库名等，



#### 2.4 SQL脚本中如何加注释

> - 单行注释：#注释内容（mysql特有的）
> - 单行注释：--空格注释内容    其中--后面的空格必须有
> - 多行注释：/* 注释内容 */

#### 2.5 SQL数据类型

> 整数 INT， BIGINT，BIT等
>
> 小数  DOUBLE FLOAT
>
> 字符串 char，varchar  char为固定长度 不管输入多少个数的字符串 都储存定义的字符串个数
>
> varchar  动态的储存
>
> Enum和Set类型
>
> 日期时间类型 DATE YEAR TIME DATETIME

#### 2.6 主键

> 非空：在数据库表中主键字段非常重要，它是区分每一条记录的根本依据，所以绝对不能为空。
>
> 唯一：为了能够区分每一条记录，所以主键值在数据库表中必须是唯一的。

##### 2.6.1 主键的业务规则

> - 不可修改：作为每一条记录的唯一标识，一旦修改就可能导致这条记录找不到。但是这个要求是对我们编写代码的要求，MySQL没有这个限制
> - 不要使用业务字段作为主键: 在我们的项目中所有描述现实世界的都是业务字段。例如：学生姓名、年龄、性别、籍贯、身高、体重……
> - 为什么不能使用业务字段作为主键呢？<br/>
>   即使使用唯一性的业务字段作为主键也不建议，因为业务字段可能为空，也可能删除导致表没有主键。

##### 2.6.2 设置主键的语法

> 使用**`PRIMARY KEY`** 作为创建主键的语法

```mysql
create table t_Stu (
	id int PRIMARY KEY,
    Stu_name char(10)
);
```

##### 2.6.3 主键自增

> - 为了保证主键值的唯一性（不重复），所以在数据库表层面为每一条新增的记录生成主键是最好的办法。
> - 如果由程序员在Java程序中生成主键值，那么并发场景下A线程计算出来是5，B线程也计算出来是5，就重复了
> - 具体来说，在创建表时可以通过**`auto_increment`**关键词设置某个字段的值由MySQL生成，程序员不必指定。<br/>

```mysql
create table t_Stu (
	id int auto_increment PRIMARY KEY,
    Stu_name char(10)
);
```

##### 2.6.4 查看表结构

```mysql
desc 表名；
desc t_Stu
```



#### 2.7 基础增删改查操作

> 先创建库和表做准备工作：

```mysql
create database db_hr;
use db_hr;
create table t_emp(
	id int auto_increment primary key,
    emp_name char(100),
    emp_age int,
    emp_salary double
);
```

##### 2.7.1 insert 语句

![](C:\Users\89585\Desktop\尚硅谷学习文件\MySQL\day01\img\insert.png)

```mysql
#插入一条数据，主键因为是自己增加的，所以不必指定数据了
insert into t_emp(emp_name, emp_age, emp_salary) VALUES("tom",25,5000.00);

#插入多条数据 
insert into t_emp(emp_name, emp_age, emp_salary) 
			VALUES("jerry", 35,      6000.00),
				  ("bob",   24,      3400.00),
				  ("harry", 86,      6000.00);
#同时一次性 插入多条数据				  
```

##### 2.7.2 delete语句

```mysql
#删除数据库表中的所有数据，但是这样做太危险， 几乎不可能发生在实际项目中
delete from t_emp;

#根据查询条件指定要删除的记录，通常是根据主键来删除.where关键词后面通过表达式来构建查询条件
delete from t_emp where id=2;

#根据主键的id 去删除指定的数据
```

##### 2.7.3 update语句

> 在指定数据库表中修改指定的字段

```mysql
# 把t_emp 表中的 emp_salary 字段设置为新值，但是这会影响到所有记录
update t_emp set emp_salary = 66666.66;
#这条语句 会把整个数据表中的 emp_salary 数据更改为 66666.66

# 使用 where 子句设置查询条件，有针对性的修改
update t_emp set emp_salary = 6666.66 where id=3;

#涉及多个字段，用逗号分开
update t_emp set emp_salary = 400, emp_name = "happy100" where id=14;


```

##### 2.7.4 select语句

> 在指定数据库中查询指定语句

```mysql
#查询 t_emp 表中的全部记录，每条记录都显示全部字段
select * from t_emp;

#查询 t_emp 表中的全部记录，每条记录都只显示指定字段
select id,emp_name,emp_salary from t_emp;

#查询 t_emp 表中符合查询条件的记录，每条记录只显示指定字段
select id,emp_name,emp_salary from t_emp where id=3;
```

> 实际开发中 不要采用select * 的语法，因为这样不知道具体查询了那些字段.时间一长，别说别人，自己都不记得.

#### 2.8 运算符与表达式

##### 1.算术运算符

| 符号 | 说明                                          |
| ---- | --------------------------------------------- |
| +    | 在MySQL中，加号就是求和，没有字符串拼接的功能 |
| -    | 做减法                                        |
| *    | 做乘法                                        |
| /    | 做除法                                        |
| div  | 做除法，但只保留商的整数部分                  |
| %    | 取模                                          |
| mod  | 取模                                          |

> 注意：MySQL中没有+=这样的写法

##### 2.比较运算符

| 符号 | 说明       |
| ---- | ---------- |
| >    | 大于       |
| <    | 小于       |
| >=   | 大于或等于 |
| <=   | 小于或等于 |
| =    | 等于       |
| !=   | 不等于     |
| <>   | 不等于     |

> - **注意**：=是做是否相等的判断，不是赋值
> - **注意**：不能写 xxx = null，此时要写 xxx is null
> - **注意**：!=或&lt;&gt;也不能用于对null值进行判断，而是要写成 xxx is not null

##### 3.区间或集合范围比较运算符

> - 查询在区间范围的记录：between x and y
> - 查询不在区间范围的记录：not between x and y
> - 查询在集合范围的记录：in (x,y,z)
> - 查询不在集合范围的记录：not in (x,y,z)

##### 4.模糊匹配比较运算符

| 符号 | 说明                               |
| ---- | ---------------------------------- |
| %    | 表示这里可以匹配任意数量的任意字符 |
| _    | 每一个下划线匹配一个任意字符       |

> 这里需要使用到 like 关键字

```mysql
select stu_name from t_Stu where stu_name like "%a";

select stu_name from t_Stu where stu_name like "__a";
```

##### 5.逻辑运算符

| 符号 | 说明     |
| ---- | -------- |
| &&   | 逻辑与   |
| and  | 逻辑与   |
| \|\| | 逻辑或   |
| or   | 逻辑或   |
| !    | 逻辑非   |
| xor  | 逻辑异或 |

##### 6.关于null值判断

> null值的判断

```mysql
xxx is null;
xxx is not null;
xxx <=> null;
```

> null值的计算
>
> 调用 **`ifnull()`** 函数，在某条记录中某个字段值为 null时，使用替代值来计算

```mysql
ifnull(xxx,替代值);
```



































