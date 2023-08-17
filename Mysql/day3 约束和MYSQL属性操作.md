# Day3 约束和MYSQL属性操作

## 1、概述

### 约束的作用

给数据库表中的数据额外添加限制条件，以此来保证『数据完整性』（说白了就是数据正确性）

### 约束的类型

#### 从约束目标来区分

- 键约束：主键约束、外键约束、唯一键约束
- Not NULL约束：非空约束
- Check约束：检查约束
- Default约束：默认值约束

#### 从约束范围来区分

- 表级约束：不仅要看约束字段当前单元格的数据，还要看其他单元格的数据。
  - 键约束
  - 检查约束
- 列级约束：约束字段只看当前单元格的数据即可，和其他单元格无关。
  - 非空约束
  - 默认值约束
    

#### 约束和索引的关系

<p>在MySQL中键约束会自动创建索引，提高查询效率。索引的详细讲解在高级部分。</p>

<p>MySQL高级会给大家讲解索引、存储引擎等，因为高级要给大家分析SQL性能。而基础阶段先不管效率，只要能查出来就行。</p>

约束和索引不同：

- 约束是一个逻辑概念，它不会单独占用物理空间。
- 索引是一个物理概念，它是会占用物理空间。



## 2、非空约束

在数据库表中，限定某个列的值不能为null。

- 细节1：只能针对单独一个列设定非空约束，不能同时针对多个字段设置组合非空。
- 细节2：一张数据库表中可以有多个字段设置非空约束。

### 语法

在创建或修改表时，声明字段时指定not null。

```sql
create table 表名称(
  字段名 数据类型 not null,
  字段名 数据类型 not null,
  字段名 数据类型
);
```

### 测试

非空约束会禁止以下行为：

- insert时，非空字段未提供值且没有默认值
- insert时，非空字段明确设置为null
- update时，非空字段明确设置为null

## 3、唯一键约束

### 效果

- 单列唯一：单个字段值不可重复。
- 组合唯一：多个字段值的组合不可重复。

### 特点

- 一个表可以有很多个唯一键约束
- 每一个唯一键约束字段都会自动创建唯一索引
- 创建唯一索引时也会自动添加唯一约束
- 唯一键约束允许为空，甚至允许约束范围内多个值为空
- 通过删除唯一键约束的索引来删除唯一键约束

### 测试

```sql
create table t_stu(  
    stu_id int auto_increment primary key ,  
    stu_name char(100) unique key , # 单列唯一  
    stu_subject char(100),  
    stu_direct char(100),  
    unique key (stu_subject, stu_direct) # 组合唯一  
);  
# 测试单列唯一  
insert into t_stu(stu_name, stu_subject, stu_direct) VALUES ("tom", "Java", "RD");  
  
# [23000][1062] Duplicate entry 'tom' for key 't_stu.stu_name'  
insert into t_stu(stu_name, stu_subject, stu_direct) VALUES ("tom", "PHP", "QA");  
  
# 测试组合唯一  
# [23000][1062] Duplicate entry 'Java-RD' for key 't_stu.stu_subject'  
insert into t_stu(stu_name, stu_subject, stu_direct) VALUES ("jerry", "Java", "RD");  
  
# 测试唯一字段可以为空，空值可以有多个  
insert into t_stu(stu_subject) values ("MySQL");  
insert into t_stu(stu_subject) values ("Oracle");
```

## 4、主键约束

### 作用

为数据库表中的每一条记录提供唯一标识。

### 特点

- 唯一并且非空
- 一个表最多只能有一个主键约束
- 主键可以由多列组成，这就是联合主键
- 主键列会自动创建索引（能够根据主键查询的，就根据主键查询，效率更高）
- 如果删除主键约束了，主键约束对应的索引就自动删除了，但是非空约束不会自动删除。

> 主键列的唯一并且非空是约束的概念，但是MySQL会给每个表的主键列创建索引，会开辟单独的物理空间来存储每一个主键的目录表（B+Tree结构）。这样设计的意义，可以根据主键快速查询到某一行的记录。

### 主键约束和唯一键约束区别

- 在一张数据库表中：主键约束只能有一个，但是唯一约束可以有多个。
- 主键约束要求字段值不可为空，唯一键约束没有非空要求。

### 语法

```sql
create table 表名称(
	# 在声明字段时指定
	字段名  数据类型  primary key,
    字段名  数据类型,  
    字段名  数据类型  
);
create table 表名称(
	字段名  数据类型,
    字段名  数据类型,  
    字段名  数据类型,
    # 在表级别指定
    primary key(字段名或字段列表)
);
```

## 5、默认值约束

### 效果

在插入记录时，如果没有给字段提供值，那么会自动使用默认值。

### 语法

```sql
create table 表名称(
	字段名  数据类型  primary key,
    字段名  数据类型  【unique key】 【not null】,  
    字段名  数据类型  【not null】 【default 默认值】 
);
```

### 测试

```sql
create table t_flower(  
    flower_id int primary key auto_increment,  
    flower_name char(100),  
    flower_color char(100) default 'red'  
);  
insert into t_flower(flower_name) values ("mother solo");
```

## 6、自增属性

### 效果

给某个字段自动赋值，这个值是一直往上增加，如果没有特殊设定，每次自增1。

### 特点和要求

- 一个表只能有一个自增字段
- 并且自增字段只能是key字段，即定义了主键、唯一键等键约束的字段。一般都是给主键或唯一键加自增。
- 自增字段应该是数值类型，一般都是整数类型。
- 如果插入数据时，自增列指定了 0 和 null，会在当前最大值的基础上自增，
- 如果自增列手动指定了具体值，直接赋值为具体值。

<br/>

### 语法

```sql
create table 表名称(
  字段名  数据类型 primary key auto_increment,
  字段名  数据类型,  
  字段名  数据类型
) auto_increment=自增起点值;
```

<br/>

### 测试

```sql
# 测试：一个表只能有一个自增字段  
# [42000][1075] Incorrect table definition;  
# there can be only one auto column and it must be defined as a key  
create table t_user(  
    user_id int auto_increment,  
    user_age int auto_increment  
);

# 测试：自增字段必须是数值类型  
# [42000][1063] Incorrect column specifier for column 'user_name'  
create table t_user(  
    user_name char(100) auto_increment  
);

create table t_pen(  
    pen_id int primary key auto_increment, # 指定字段值自增生成  
    pen_name char(100)  
) auto_increment=66; # 指定自增值起点  
insert into t_pen(pen_name) values("pen01");  
insert into t_pen(pen_name) values("pen02");  
insert into t_pen(pen_name) values("pen03");  
  
# 如果自增列指定了 0 和 null，会在当前最大值的基础上自增  
insert into t_pen(pen_id, pen_name) values(0, "pen04");  
insert into t_pen(pen_id, pen_name) values(null, "pen05");  
  
# 如果自增列手动指定了具体值，直接赋值为具体值。  
insert into t_pen(pen_id, pen_name) values(144, "pen06");
```

<br/>

## 7、检查约束

### 效果

<p>检查（CHECK） 约束用于限制字段中的值的范围。如果对单个字段定义 CHECK 约束，那么该字段只允许特定范围的值。</p>

<p>如果对一个表定义 CHECK 约束，那么此约束会基于行中其他字段的值在特定的字段中对值进行限制。</p>

<p>在MySQL 8.0.16版本之前， CREATE TABLE语句支持给单个字段定义CHECK约束的语法，但是不起作用。</p>

<p>在MySQL8.0.16版本之后，CREATE TABLE语句既支持给单个字段定义列级CHECK约束的语法，还支持定义表级CHECK约束的语法。</p>

### 语法

```sql
#在建表时，可以指定检查约束
create table 表名称(
  字段名1 数据类型 check(条件),  # 列级检查约束
  字段名2 数据类型,
  字段名3 数据类型,
  check (条件) # 表级检查约束
);
```

### 测试

```sql
create table t_employee(  
    employee_id int auto_increment primary key ,  
    employee_name char(100) check ( employee_name like '%dog%'), # 列级检查约束  
    hire_date date,  
    birthday date,  
    check ( year(hire_date)-year(birthday)>=18 ) # 表级检查约束  
);  
  
# 正确插入  
insert into t_employee(employee_name, hire_date, birthday)  
values ("tom dog peter", "2023-05-11", "1986-10-14");  
  
# 违反列级约束  
# [HY000][3819] Check constraint 't_employee_chk_1' is violated.  
insert into t_employee(employee_name, hire_date, birthday)  
values ("tom peter", "2023-05-11", "1986-10-14");  
  
# 违反表级约束  
# [HY000][3819] Check constraint 't_employee_chk_2' is violated.  
insert into t_employee(employee_name, hire_date, birthday)  
values ("tom dog peter", "2023-05-11", "2019-10-14");
```

## 8、外键约束(了解)

### 效果

<p>限定表中某个字段的引用完整性。</p>

如果t_emp表通过dept_id字段关联到t_dept表，那么外键约束会要求t_emp表中的dept_id字段值在t_dept表中都能够找到。

### 相关概念

- 主表（父表）：被引用的表，被参考的表
- 从表（子表）：引用别人的表，参考别人的表

<br/>

例如：员工表的员工所在部门这个字段的值要参考部门表，

- 部门表是主表
- 员工表是从表。

<br/>

### 特点

- 在“从表”中指定外键约束，并且一个表可以建立多个外键约束
- 创建(create)表时就指定外键约束的话，先创建主表，再创建从表
- 删表时，先删从表（或先删除外键约束），再删除主表。或者先解除关系，再各自删除。
- 从表的外键列，必须引用/参考主表的键列（主键或唯一键），因为被依赖/被参考的值必须是唯一的
- 从表的外键列的数据类型，要与主表被参考/被引用的列的数据类型一致，并且逻辑意义一致。
- 外键列也会自动建立索引（根据外键查询效率很高，很多）
- 外键约束的删除，所以不会自动删除索引，如果要删除对应的索引，必须手动删除

### 语法

```sql
create table 主表名称(
	字段1  数据类型  primary key,
	字段2  数据类型
);

create table 从表名称(
	字段1  数据类型  primary key,
	字段2  数据类型,
	foreign key （从表的某个字段) references 主表名(被参考字段)
);
```

### 测试

```sql
create table t_author(  
    author_id int auto_increment primary key,  
    author_name char(100)  
);  
  
create table t_book(  
    book_id int auto_increment primary key,  
    book_name char(100),  
    author_id int,  
    foreign key (author_id) references t_author(author_id)  
);  
insert into t_author(author_name) values ("tom");  
insert into t_book(book_name, author_id) VALUES ("tom and pig", 1);  
  
# [23000][1452] Cannot add or update a child row: a foreign key constraint fails (`db_hello`.`t_book`, CONSTRAINT `t_book_ibfk_1` FOREIGN KEY (`author_id`) REFERENCES `t_author` (`author_id`))  
insert into t_book(book_name, author_id) VALUES ("tom and pig2", 6);
```

## 9、修改表结构和约束

### 修改表结构

```sql
# 操作1：表重命名  
alter table t_book rename to t_book2;
  
# 操作2：表重命名的另一种写法  
rename table t_book2 to t_book;
  
# 操作3：表中删除字段  
alter table t_flower drop flower_name;
  
# 操作4：表中增加字段  
alter table t_flower add flower_name char(100);
  
# 操作5：表中增加字段，并将字段顺序设置为第一个
alter table t_flower add flower_love char(100) first ;
  
# 操作6：表中增加字段，并将字段顺序设置为指定字段的后面
alter table t_flower add flower_good char(100) after flower_color;
  
# 操作7：修改字段名称，虽然是修改名称，但是新字段名还是要附加字段类型
alter table t_flower change flower_name2 flower_name char(50);
  
# 操作8：修改字段类型等细节，column 关键词可以省略。first、after关键字仍然可以使用  
alter table t_flower modify column flower_name varchar(60);
```

### 修改约束

<br/>

```sql
# 操作1：给表添加主键  
create table t_pig(  
    pig_id int,  
    pig_name char(100),  
    pig_age int,  
    pig_weight int  
);  
alter table t_pig add primary key(pig_id);  
alter table t_pig modify pig_id int auto_increment;  
  
# 操作2：给表删除主键  
alter table t_pig modify pig_id int;  
alter table t_pig drop primary key ;  
  
# 操作3：增加唯一键约束  
alter table t_pig add unique key (pig_name);  
alter table t_pig add unique key (pig_age, pig_weight);  
  
# 操作4：删除唯一键约束，通过删除唯一索引实现  
alter table t_pig drop index pig_age;  
  
# 操作5：查看指定表的索引信息
# 查看索引
show index from t_pig;
# 查看约束
SELECT * FROM information_schema.table_constraints WHERE table_name = 't_pig' and CONSTRAINT_SCHEMA="db_good";  
  
# 操作6：非空约束、默认值约束、自增属性通过修改字段的方式添加或删除  
alter table t_pig modify pig_age int not null default 20;  
  
# 操作7：添加检查约束  
# 无效  
alter table t_pig modify pig_age int check ( pig_age between 0 and 10);  
# 有效  
alter table t_pig add check ( pig_age between 0 and 10);  
  
# 操作8：删除检查约束  
alter table t_pig drop check t_pig_chk_1;  
  
# 操作9：查看表中的约束  
SELECT * FROM information_schema.table_constraints WHERE table_name = 't_pig';  
  
# 操作10：增加外键约束  
create table t_master(  
    master_id int primary key auto_increment,  
    master_name char(100)  
);  
  
create table t_slave(  
    slave_id int primary key auto_increment,  
    slave_name char(100),  
    master_id int  
);  
  
# 添加外键约束  
alter table t_slave add foreign key (master_id) references t_master(master_id);  
  
# 查看约束  
SELECT * FROM information_schema.table_constraints WHERE table_name = 't_slave';  
  
# 删除外键约束  
alter table t_slave drop foreign key t_slave_ibfk_1;  
  
# 查看索引  
show index from t_slave;  
  
# 删除创建外键约束时附带创建的索引  
alter table t_slave drop index master_id;
```

### 