# 六、事务

## 1、概念

<p>实际开发过程中，往往完成一个完整的业务功能需要执行多条SQL语句。而整体功能中的每一条SQL语句都缺一不可，一旦任何一条SQL语句执行失败，数据就会变得不正确。</p>

<br/>

以银行转换为例：

- A账户转出100元，执行update语句
- B账户转入100元，执行update语句
- A账户所在银行保存交易记录，执行insert语句
- B账户所在银行保存交易记录，执行insert语句
- 总行保存交易记录，执行insert语句

<br/>

<p>以上操作缺失任何一个，整个功能就执行失败了。所以这些SQL语句必须当作一个整体来对待。</p>
<p>要生效一起生效，有任何失败则整体撤销。</p>

## 2、事务ACID特性

### ①Automicity原子性

从功能逻辑的角度来说：事务中的各个操作缺一不可，不可再分。缺少任何一个操作，事务整体在逻辑上就不成立了，就不能满足业务需求了。

<br/>

上面转账是一个例子，这里我们再举一个下单的例子：

- 保存订单数据：t_order表执行insert操作
- 保存订单详情数据：t_order_item表执行insert操作
- 更新库存数据：t_stock表执行update操作
- 更新销量数据：t_sales表执行update操作
- 更新用户积分：t_user_score表执行update操作

<br/>

### ②Consistency一致性

<p>事务执行前，数据处于一致性状态——说白了就是都正确。</p>
<p>事务执行后，数据仍然处于一致性状态。</p>

<br/>

为了做到这一点：

- 提交事务：事务中所有操作全部都成功（要么都做）
- 回滚事务：事务中有任何一个操作失败（要么都撤）

<br/>

### ③Isolation隔离性

事务可以在彼此隔离的状态下并发执行。

<br/>

### ④Durability持久性

事务一旦被提交，它对数据库中数据的改变就是永久性的，接下来的其他操作和数据库故障不应该对其有任何影响。

<br/>

## 3、测试

### 开启事务

<p>MySQL中事务是默认开启的，也就是说，一条SQL语句执行完成后会自动提交。一条SQL语句自己是一个事务整体。</p>
<p>如果我们需要把多条SQL语句设置为一个事务，那显然需要手动控制，也就是把自动提交改成手动提交。</p>

```sql
# 下面语句执行之后，它之后的所有sql，都需要手动提交才能生效，直到恢复自动提交模式。
# 关闭事务的自动提交方式一
set autocommit = false;

# 关闭事务的自动提交方式二
set autocommit = 0;

# 恢复事务为自动提交：把 autocommit 设置为 true 或 1
```

<br/>

```sql
set autocommit = false;  

# 未提交时修改不生效
update t_pig set pig_name="uuu" where pig_id=3;  

commit ;
```

### DDL不支持事务

```sql
# 说明：DDL不支持事务
# DDL：create,drop,alter等创建库、创建表、删除库、删除表、修改库、修改表结构等这些语句不支持事务。
# 换句话只有insert,update,delete语句支持事务。
# TRUNCATE 表名称; 清空整个表的数据，不支持事务。 把表drop掉，新建一张表。

START TRANSACTION;
TRUNCATE t_employee;
ROLLBACK; # 回滚无效
```

## 4、事务隔离级别

**数据库事务的隔离性**：数据库系统必须具有隔离并发运行各个事务的能力, 使它们不会相互影响, 避免各种并发问题。

**一个事务与其他事务隔离的程度称为隔离级别。** 数据库规定了多种事务隔离级别, 不同隔离级别对应不同的干扰程度, 隔离级别越高, 数据一致性就越好, 但并发性越弱。

### 并发问题

| 并发问题   | 现象描述                                                     |
| ---------- | ------------------------------------------------------------ |
| 脏读       | 事务A读取了事务B修改的数据，但是事务B回滚了操作，事务A读取到的数据就是错误的，这样的数据我们称之为脏数据。 |
| 不可重复读 | 事务A在执行期间多次读取同一个数据，但是由于这个数据被其它事务修改了，所以每次读取到的数据不一样。 |
| 幻读       | 事务A执行过程中，表中的记录数量有变化，就像发生了幻觉。      |

### 隔离级别

| 隔离级别 | 英文名称         | 工作机制                                                   | 可以解决的并发问题     |
| -------- | ---------------- | ---------------------------------------------------------- | ---------------------- |
| 读未提交 | read-uncommitted | 当前事务可以读取其它事务尚未提交的修改                     | 无                     |
| 读已提交 | read-committed   | 当前事务只读取其它事务已经提交的修改                       | 脏读                   |
| 可重复读 | repeatable-read  | 当前事务在读取数据过程中多次读取同一个数据可以得到相同结果 | 脏读、不可重复读       |
| 串行化   | serializable     | 当前事务执行过程中，锁定整个表                             | 脏读、不可重复读、幻读 |

- 数据一致性评价：从上到下，数据一致性越来越好
- 性能评价：从上到下，性能越来越差

### 查看事务隔离级别

```sql
# 全局范围  
SELECT @@global.transaction_isolation;  
  
# 当前会话范围  
SELECT @@session.transaction_isolation;
```

### 设置事务隔离级别

```sql
# 全局范围  
SET global TRANSACTION ISOLATION LEVEL read committed;  

# 当前会话范围  
SET SESSION TRANSACTION ISOLATION LEVEL read committed;
```

# 七、选学内容

数据库创建时 所选择的格式

| 排序规则               | 介绍                                                         |
| ---------------------- | ------------------------------------------------------------ |
| utf8mb4_general_ci     | 不区分大小写，以通用排序规则对字符串进行排序                 |
| utf8mb4_bin            | 区分大小写，以二进制方式对字符串进行排序                     |
| utf8mb4_unicode_ci     | 不区分大小写，使用Unicode排序算法对字符串进行排序。 支持多语言排序，包括特殊字符和变音符号。 |
| utf8mb4_unicode_520_ci | 不区分大小写，基于Unicode 5.20版本的排序规则。 支持更广泛的语言和排序方式 |
| utf8_general_ci        | 不区分大小写，以通用排序规则对字符串进行排序。 适用于旧版本的UTF-8字符集 |
| utf8_bin               | 区分大小写，以二进制方式对字符串进行排序。适用于旧版本的UTF-8字符集 |
| latin1_swedish_ci      | 不区分大小写，以瑞典语排序规则对字符串进行排序。 适用于ISO 8859-1字符集 |