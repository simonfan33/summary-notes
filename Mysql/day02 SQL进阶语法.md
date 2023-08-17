## day02 SQL进阶语法

### 1.SQL查询语句的各子句

> 1. select ：查询
> 2. from：从那个表中筛选
> 3. inner | left | right ... join on： 关联多表查询时，去除笛卡尔积
> 4. where：从表中筛选的条件
> 5. group by：分组依据
> 6. having：在分组统计结果中再次筛选(with rollup)
> 7. order by：排序
> 8. limit：分页

必须按照从1到8的顺序去编写各子句

### 2.distinct去重

```mysql
#使用distinct 关键字对查询结构去重，重复的查询结果记录只保留一条
select distinct emp_name, emp_salary, emp_subject from t_emp where emp_id in (54,55);
```

### 3. 关联查询

> 表和表之间，通过字段之间的关联可以设定表之间的关联关系：
> ❤一对一：一个人和他的身份证号之间的关系。
> ❤一对多：客户和订单之间的关系。
> ❤多对多：老师和学生之间的关系。

#### 3.1 概念

> 当A表通过某个字段关联到b表时，我们说A表和B表之间建立了关联关系
>
> 当我们根据表之间的关联关系，在查询中涉及多张表时，就是关联查询。
>
> 比如：员工表通过部门编号关联部门表。

> 关联字段 需要满足一下条件：
>
> - 逻辑意义一样：比如t_emp表中使用的 dept_id关联t_dept表中的id
> - 数据类型一样
>
> 以下的两方面不要求
>
> - 字段名不要求一样
> - 创建外键约束不要求

#### 3.2 关联查询的语法要求

> 联合查询必须写关联条件，关联条件的个数 = n - 1。n是联合查询的表的数量。
>
> - 如果2个表一起联合查询，关联条件数量是1，
> - 如果3个表一起联合查询，关联条件数量是2，
> - 如果4个表一起联合查询，关联条件数量是3，
> - 以此类推。。。。 如果不指定连接条件，就会出现笛卡尔积现象，这是应该避免的。
>   所谓笛卡尔积就A表中每条记录都关联B表中的每条记录，既不符合逻辑，又会导致查询结果数据量暴增。

```mysql
# 笛卡尔积：A 表中每一条记录都和 B 表中的每一条记录匹配，  
# 查询结果数量=A表记录数×B表记录数  
# 危害1：记录全量匹配相乘导致记录数相乘，数据量暴增  
# 危害2：数据之间组合的关系不符合数据本身的逻辑关系  
select t_emp.emp_name, t_dept.dept_id from t_emp,t_dept;
```

> 关联条件可以用on子句编写，也可以写到where中。
> 但是建议用on单独编写，可读性更好。
> 每一个join后面都要加on子句：
>
> - A inner|left|right join B on 条件
> - A inner|left|right join B on 条件 inner|left|right jon C on 条件

#### 3.3 SQL实现 

##### 3.3.1 内连接

> - 语法：A表 inner join B表 on 连接条件
>
> - 执行结果：A表 ∩ B表

#### ![](C:\Users\89585\Desktop\尚硅谷学习文件\MySQL\day02\img\img0064.png)

> **`如果遇到：“Column 'did' in field list is ambiguous”的错误提示，就是说两张表的同名字段必须加别名。`**

```mysql
# 为了简化 SQL 语句，MySQL 允许我们给表设置别名  
select emp_id, emp_name, d.dept_id, dept_name  
from t_emp as e  
         inner join t_dept as d on e.dept_id = d.dept_id;  
  
select emp_id, emp_name, d.dept_id, dept_name  
from t_emp e  
         inner join t_dept d on e.dept_id = d.dept_id;  
  
# 字段也可以设置别名（as 也可以省略）  
# 字段别名加不加引号都行  
select emp_id as "员工编号", emp_name as "员工姓名", d.dept_id "部门编号", dept_name 部门名称  
from t_emp e  
         inner join t_dept d on e.dept_id = d.dept_id;  
  
# 表的别名不能加引号  
# [42000][1064] You have an error in your SQL syntax;  
# check the manual that corresponds to your MySQL server version for the right syntax to use near '"e" inner join t_dept "d" on e.dept_id = d.dept_id' at line 2  
select emp_id, emp_name, d.dept_id, dept_name  
from t_emp "e"  
         inner join t_dept "d" on e.dept_id = d.dept_id;  
select emp_id, emp_name, d.dept_id, dept_name  
from t_emp 'e'  
         inner join t_dept 'd' on e.dept_id = d.dept_id;
```

##### 3.3.2 左外连接

> - 语法：A表 left join B表 on 连接条件
> - 执行结果：
>   - A表全部
>   - A表 - A∩B

![](C:\Users\89585\Desktop\尚硅谷学习文件\MySQL\day02\img\img0065.png)

![](C:\Users\89585\Desktop\尚硅谷学习文件\MySQL\day02\img\img0066.png)

##### 3.3.3 右外连接

> - 语法：A表 right join B表 on 连接条件
> - 执行结果：
>   - B表全部
>   - B表 - A∩B

![](C:\Users\89585\Desktop\尚硅谷学习文件\MySQL\day02\img\img0067.png)

![](C:\Users\89585\Desktop\尚硅谷学习文件\MySQL\day02\img\img0068.png)

##### 3.3.4 全外连接

> - 语法：full outer join ... on，但是MySQL不支持这个关键字，MySQL使用union（合并）结果的方式代替
> - 执行结果：A表 ∪ B表
> - MySQL替代方案：A表查询语句 union B表查询语句

> union的语法细节：
>
> - 参与union的查询语句，输出的字段必须是一样的
> - UNION ALL合并不去重
> - UNION合并且去重

![](C:\Users\89585\Desktop\尚硅谷学习文件\MySQL\day02\img\img0069.png)

```mysql
#查询所有员工和所有部门，包括没有指定部门的员工和没有分配员工的部门。 
SELECT * FROM t_employee LEFT JOIN t_department 
ON t_employee.did = t_department.did

UNION 

SELECT * FROM t_employee RIGHT JOIN t_department 
ON t_employee.did = t_department.did;
```

![](C:\Users\89585\Desktop\尚硅谷学习文件\MySQL\day02\img\img0070.png)

```mysql
#查询那些没有分配部门的员工和没有指定员工的部门，即A表和B表在对方那里找不到对应记录的数据。
SELECT *
FROM t_employee LEFT JOIN t_department
ON t_employee.did = t_department.did
WHERE t_employee.did IS NULL

UNION 

SELECT *
FROM t_employee RIGHT JOIN t_department
ON t_employee.did = t_department.did
WHERE t_employee.did IS NULL;
```

##### 3.3.5 自连接

> 一张表自己和自己关联，物理上来说是同一张表，逻辑上当作两张表来写SQL。

```mysql
#查询每一个员工自己的编号、名字、薪资和他的领导的编号、姓名、薪资。
#把t_employee当成两张表，通过取别名的方式
#t_employee AS emp 把员工表 当成员工表
# t_employee AS mgr 把员工表  当成存储领导信息的领导表
#emp.mid = mgr.eid; 员工表的领导编号就是领导表的员工编号
SELECT emp.eid,emp.ename,emp.salary,  mgr.eid,mgr.ename,mgr.salary
FROM t_employee AS emp INNER JOIN t_employee AS mgr
ON emp.mid = mgr.eid;
```

##### 3.3.6 小结

![](C:\Users\89585\Desktop\尚硅谷学习文件\MySQL\day02\img\img0071.png)

###  4. 分组与聚合查询

#### 4.1 分组的概念

> 根据某个（或某几个）字段的值，把查询结果分组，指定字段的值相同的划分到同一组。例如：根据部门id分组

![](C:\Users\89585\Desktop\尚硅谷学习文件\MySQL\day02\img\img0072.png)

#### 4.2 聚合的概念

> 分组之后，一个组内很可能包含很多条数据，而最终的查询结果中，一个组只生成一条记录。
> 所以问题来了，组内多条记录怎么压缩成一条？
> 两个办法：
>
> - 情况一：某个字段在组内所有记录中的值都是一样的，那就可以直接用。
> - 情况二：使用聚合函数。

#### 4.3 案例一

> 目标：查询各部门员工的平均工资。

```mysql
select avg(emp_salary), dept_id from t_emp group by dept_id;
```

#### 4.4 案例二

> 目标：得到各部门平均工资之后显示合计数值

```mysql
select avg(emp_salary), dept_id from t_emp group by dept_id with rollup ;
```

#### 4.5 案例三

> 目标：先按照部门分组，在部门分组结果内再按照专业分组

```mysql
select avg(emp_salary), emp_subject, dept_id  
from t_emp  
group by dept_id, emp_subject;
```

#### 4.6 案例四

> 目标：查询各部门平均工资，要求显示部门名称。

```mysql
select avg(emp_salary), d.dept_id, dept_name  
from t_emp e  
         left join t_dept d on e.dept_id = d.dept_id  
group by dept_id;
```

#### 4.7 案例五

> 关于分组、聚合操作时涉及的查询条件，原则是：能在分组、聚合操作前执行的查询条件就在操作前执行。因为查询条件把不满足条件的数据过滤掉之后，分组、聚合操作运算量更小，效率更高。
>
> - 分组、聚合操作前执行的查询条件：where子句
> - 分组、聚合操作后执行的查询条件：having子句

```mysql
# 分组、聚合操作前过滤数据：针对所有女员工数据根据部门 id 分组  
select avg(emp_salary), dept_id, emp_gender  
from t_emp  
where emp_gender = 'female'  
group by dept_id;  
  
# 分组、聚合操作后过滤数据：显示平均工资大于4000的聚合结果  
select avg(emp_salary) emp_avg_salary, dept_id  
from t_emp  
group by dept_id  
having emp_avg_salary > 4000;
```

###  5. 子查询

#### 5.1 概念

> 嵌套在另一个SQL语句中的查询。select、update、delete、insert、create等语句都可以嵌套子查询。

#### 5.2 select 嵌套子查询

```mysql
# 需求：计算每个员工的工资和所有员工平均工资的差值
select emp_id,   
       emp_name,   
       emp_salary,   
       emp_salary - (select avg(emp_salary) from t_emp) difference
from t_emp;
```

#### 5.3 where嵌套子查询

> where子句中嵌入子查询肯定是作为查询条件。所以子查询部分需要配合各种表达式。此时需要考虑一个因素：子查询返回的数据是一个值还是多个值？
>
> - 子查询结果：单列单个值——直接使用比较运算符，如“<”、“<=”、“>”、“>=”、“=”、“!=”等与子查询结果进行比较。
> - 子查询结果：单列多个值
>   - 可以使用比较运算符IN或NOT IN进行比较。
>   - 可以使用比较运算符, 如“<”、“<=”、“>”、“>=”、“=”、“!=”等搭配ANY、SOME、ALL等关键字与查询结果进行比较。

```mysql
# 查询工资最高的员工工资和姓名
SELECT ename,salary FROM t_employee WHERE salary = (SELECT MAX(salary) FROM t_employee);

# 在“t_employee”表中查询和“白露”，“谢吉娜”同一部门的员工姓名和电话。
SELECT ename,tel,did
FROM t_employee
WHERE did IN(SELECT did FROM t_employee WHERE ename='白露' || ename='谢吉娜');

SELECT ename,tel,did
FROM t_employee
WHERE did = ANY(SELECT did FROM t_employee WHERE ename='白露' || ename='谢吉娜');

# 在“t_employee”表中查询薪资比“白露”，“李诗雨”，“黄冰茹”三个人的薪资都要高的员工姓名和薪资。
SELECT ename,salary
FROM t_employee
WHERE salary >ALL(SELECT salary FROM t_employee WHERE ename IN('白露','李诗雨','黄冰茹'));
```

#### 5.4 having嵌套子查询

```mysql
# 查询“t_employee”和“t_department”表，按部门统计平均工资，
# 显示部门平均工资比全公司的总平均工资高的部门编号、部门名称、部门平均薪资，
# 并按照部门平均薪资升序排列。
SELECT t_department.did,dname,AVG(salary)
FROM t_employee RIGHT JOIN t_department
ON t_employee.did = t_department.did
GROUP BY t_department.did
HAVING AVG(salary) >(SELECT AVG(salary) FROM t_employee)
ORDER BY AVG(salary);
```

#### 5.5 EXISTS型子查询

> 如果EXISTS关键字后面的参数是一个任意的子查询，系统将对子查询进行运算以判断它是否返回行，如果至少返回一行，那么EXISTS的结果为true，此时外层查询语句将进行查询；如果子查询没有返回任何行，那么EXISTS的结果为false，此时外层查询语句不进行查询。EXISTS和NOT EXISTS的结果只取决于是否返回行，而不取决于这些行的内容，所以这个子查询输入列表通常是无关紧要的。

```mysql
# 查询“t_employee”表中是否存在部门编号为NULL的员工，
# 如果存在，查询“t_department”表的部门编号、部门名称。
SELECT * FROM t_department 
WHERE EXISTS(SELECT * FROM t_employee  WHERE did IS NULL);

# 查询“t_department”表是否存在与“t_employee”表相同部门编号的记录，
# 如果存在，查询这些部门的编号和名称。
SELECT * FROM t_department
WHERE EXISTS(SELECT * FROM t_employee WHERE t_employee.did = t_department.did);

# 查询结果等价于下面的sql
SELECT DISTINCT t_department.*
FROM t_department INNER JOIN t_employee
ON t_department.did = t_employee.did;
```

#### 5.6 from嵌套子查询

> 当子查询结果是多列的结果时，通常将子查询放到FROM后面，然后采用给子查询结果取别名的方式，把子查询结果当成一张“动态生成的临时表”使用。

```mysql
# 在“t_employee”表中，查询每个部门的平均薪资，
# 然后与“t_department”表联合查询
# 所有部门的部门编号、部门名称、部门平均薪资。

SELECT did,AVG(salary) FROM t_employee GROUP BY did;

+------+-------------+
| did  | AVG(salary) |
+------+-------------+
|    1 |  11479.3125 |
|    2 |       13978 |
|    3 |    37858.25 |
|    4 |       12332 |
|    5 |       11725 |
+------+-------------+
5 ROWS IN SET (0.00 sec)

# 用上面的查询结果，当成一张临时表，与t_department部门表做联合查询
# 要给这样的子查询取别名的方式来当临时表用，该临时表必须起别名。
# 而且别忘了：表的别名不能加引号（字段别名可以）

# 错误示范：from后面的t_department和temp表都没有salary字段，
# SELECT t_department.did ,dname,AVG(salary)出现AVG(salary)是错误的
SELECT t_department.did ,dname, AVG(salary)
FROM t_department LEFT JOIN (SELECT did,AVG(salary) FROM t_employee GROUP BY did) temp
ON t_department.did = temp.did;

# 正确写法是：
SELECT t_department.did,dname,pingjun
FROM t_department LEFT JOIN (SELECT did,AVG(salary) AS pingjun FROM t_employee GROUP BY did) temp
ON t_department.did = temp.did;
```

#### 5.7 update嵌套子查询

```mysql
# [1]测试部员工涨薪1.5倍。
UPDATE t_employee
SET salary = salary * 1.5
WHERE did = (SELECT did FROM t_department WHERE dname = '测试部');

# [2]修改“t_employee”表中did为NULL的员工信息，
# 将他们的did值修改为“测试部”的部门编号。
# 子查询select did from t_department where dname = '测试部'
# 这种子查询必须是单个值，否则无法赋值
UPDATE t_employee 
SET did = (SELECT did FROM t_department WHERE dname = '测试部')
WHERE did IS NULL;

# [3]修改“t_employee”表中“李冰冰”的薪资值等于“孙红梅”的薪资值。
# 这里使用子查询先在“t_employee”表中查询出“孙红梅”的薪资。
# select salary from t_employee where ename = '孙红梅';

# 错误示范：You can't specify target table 't_employee' for update in FROM clause'
UPDATE t_employee
SET salary = (SELECT salary FROM t_employee WHERE ename = '孙红梅')
WHERE ename = '李冰冰';

# 当update的表和子查询的表是同一个表时，需要将子查询的结果用临时表的方式表示
# 也就是再套一层子查询，使得update和最外层的子查询不是同一张表
UPDATE t_employee
SET salary = (SELECT salary FROM(SELECT salary FROM t_employee WHERE ename = '孙红梅')temp)
WHERE ename = '李冰冰';

# [3]修改“t_employee”表“李冰冰”的薪资与她所在部门的平均薪资一样。
# 子查询第一层，查询李冰冰的部门编号 
# select did from t_employee where ename = '李冰冰';

# 子查询第二层，查询李冰冰所在部门的平均薪资
# select avg(salary) from t_employee where did = (select did from t_employee where ename = '李冰冰');

# 子查询第三层，把第二层的子查询结果当成临时表再查一下结果
# 目的使得和外层的update不是同一张表
SELECT pingjun 
FROM (
	SELECT AVG(salary) pingjun 
	FROM t_employee 
	WHERE did = (SELECT did FROM t_employee WHERE ename = '李冰冰') temp
);

# update更新
UPDATE t_employee
SET salary = 
(SELECT pingjun FROM 
  (SELECT AVG(salary) pingjun FROM t_employee WHERE did = 
    (SELECT did FROM t_employee WHERE ename = '李冰冰') ) temp)
WHERE ename = '李冰冰';
```

#### 5.8 delete嵌套子查询

```mysql
# [1]从“t_employee”表中删除“测试部”的员工记录。
DELETE FROM t_employee 
WHERE did = (SELECT did FROM t_department WHERE dname = '测试部');

# [2]从“t_employee”表中删除和“李冰冰”同一个部门的员工记录。
# 子查询 “李冰冰”的部门编号
# select did from t_employee where ename = '李冰冰';

# You can't specify target table 't_employee' for update in FROM clause'
# 删除和子查询是同一张表
DELETE FROM t_employee WHERE did = (SELECT did FROM t_employee WHERE ename = '李冰冰');

DELETE FROM t_employee WHERE did = (SELECT did FROM (SELECT did FROM t_employee WHERE ename = '李冰冰')temp);
```

#### 5.9 使用子查询复制表

```mysql
# 仅仅是复制表结构，可以用create语句
CREATE TABLE department LIKE t_department;

# 使用INSERT语句+子查询，复制数据，此时INSERT不用写values
INSERT INTO department (SELECT * FROM t_department WHERE did<=3);

# 同时复制表结构+数据
# 如果select后面是部分字段，复制的新表就只有这一部分字段
CREATE TABLE d_department AS (SELECT * FROM t_department);
```

###  6. 排序

```mysql
# 需求：查询员工姓名和工资，按照工资排序（默认是升序）  
select emp_name, emp_salary from t_employee order by emp_salary;  
select emp_name, emp_salary from t_employee order by emp_salary asc;  
  
# 需求：降序排序  
select emp_name, emp_salary from t_employee order by emp_salary desc;  
  
# 需求：先根据部门 id 排序，在部门 id 重复的范围内，再根据工资排序  
select emp_name, dept_id, emp_salary from t_employee order by dept_id asc, emp_salary desc;
```

###  7. 分页

![](C:\Users\89585\Desktop\尚硅谷学习文件\MySQL\day02\img\img0073.png)

> 页面上每次只显示数据的一部分，而不是全部数据。

> 分页显示数据的好处：
>
> - 查询速度比加载全部数据要快。
> - 只加载一部分数据，节约资源：内存、网络传输时间……
> - 有利于改善用户体验：一次性看到非常多的数据（几百、上千），用户体验会不太好

> 页面上用户操作的方式：
>
> - 点击页码按钮
> - 点击上一页、下一页按钮
> - 在文本框输入指定页的页码 上面三种其实都是提供了一个参数：目标页面的页码（pageNo）
>
> 
> 除了页码，还有一个隐含的参数：每页显示数据的数量（pageSize）
>
> 我们后端程序的任务就是根据前端提供的 pageNo、pageSize 把页面显示分页所需的数据查询出来。

#### 7.1 Mysql分页

> MySQL 分页需要用到 limit 子句。limit 子句需要传入两个参数：
>
> - 参数1：index
> - 参数2：pageSize
>
> 使用 limit 子句的公式：(pageNo-1)*pageSize,pageSize

```mysql
# limit 子句本身的含义是：从你指定的索引位置开始，加载指定数量的记录  
select emp_id, emp_name, emp_age from t_employee limit 0, 5;  
select emp_id, emp_name, emp_age from t_employee limit 5, 5;  
select emp_id, emp_name, emp_age from t_employee limit 10, 5;  
  
# 第一页 pageNo:1，index:0      0*5     (pageNo-1)*pageSize  
# 第二页 pageNo:2，index:5      1*5  
# 第三页 pageNo:3，index:10     2*5
```



### 8、系统函数

> 函数：代表一个独立的可复用的功能。
>
> 和Java中的方法有所不同，不同点在于：MySQL中的函数必须有返回值，参数可以有可以没有。
>
> MySQL函数分类：
>
> \- 系统预定义函数：MySQL数据库管理软件给我们提供好的函数，直接用就可以，任何数据库都可以用公共的函数。 - 聚合函数：或者又称为分组函数，多行函数，表示会对表中的多行记录一起做一个“运算”，得到一个结果。 - 单行函数：表示会对表中的每一行记录分别计算，有n行得到还是n行结果 - 用户自定义函数：由开发人员自己定义的，通过CREATE FUNCTION语句定义，是属于某个数据库的对象。

#### 8.1 单行函数举例

##### concat字符串拼接

```mysql
# 拼接字符串  
select concat("Hello", " ", "tom!");  
select emp_id, concat(emp_name, " ", emp_salary) from t_employee where emp_id=6;  
  
# 替换字符串  
select replace(emp_name, "a", "@") from t_employee;
```



##### case ... when

```mysql
SELECT eid,ename,salary,
CASE WHEN salary>20000 THEN '羡慕级别'
     WHEN salary>15000 THEN '努力级别'
     WHEN salary>10000 THEN '平均级别'
     ELSE '保底级别'
END AS "等级"
FROM t_employee;  
```

##### round()

```mysql
# ROUND(x,y) 返回参数x的四舍五入的有y位的小数的值  
select avg(emp_salary) from t_employee;  
select round(avg(emp_salary), 2) from t_employee;  
select round(avg(emp_salary), 1) from t_employee;
```



#### 8.2 聚合函数举例

> - avg() 多个值计算平均数
> - min() 多个值取最小值
> - max() 多个值取最大值
> - sum() 多个值求和
> - count() 多个值计数
> - group_concat() 分组后，组内多个值拼接成一个字符串

```mysql
# group_concat()分组后，组内多个值拼接成一个字符串  
select dept_id 部门id, group_concat(emp_name separator ",") 部门成员列表  
from t_employee  
group by dept_id;
```

















