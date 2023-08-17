# 五、DAO

## 1、三层架构结构梳理

![img.png](C:/Users/89585/Desktop/%E5%B0%9A%E7%A1%85%E8%B0%B7%E5%AD%A6%E4%B9%A0%E6%96%87%E4%BB%B6/MySQL/day05-dao/2023%E5%B9%B47%E6%9C%881%E6%97%A5/%E7%AC%94%E8%AE%B0/images/note02-jdbc/img0112.png)

## 2、DAO接口和实现类

### ①EmployeeDao接口

![img.png](C:/Users/89585/Desktop/%E5%B0%9A%E7%A1%85%E8%B0%B7%E5%AD%A6%E4%B9%A0%E6%96%87%E4%BB%B6/MySQL/day05-dao/2023%E5%B9%B47%E6%9C%881%E6%97%A5/%E7%AC%94%E8%AE%B0/images/note02-jdbc/img0113.png)

<br/>

```java
public interface EmployeeDao {
}
```

### ②EmployeeDaoImpl实现类

![img.png](C:/Users/89585/Desktop/%E5%B0%9A%E7%A1%85%E8%B0%B7%E5%AD%A6%E4%B9%A0%E6%96%87%E4%BB%B6/MySQL/day05-dao/2023%E5%B9%B47%E6%9C%881%E6%97%A5/%E7%AC%94%E8%AE%B0/images/note02-jdbc/img0114.png)

<br/>

```java
public class EmployeeDaoImpl extends BaseDao implements EmployeeDao{
}
```

### ③BaseDao

![img.png](C:/Users/89585/Desktop/%E5%B0%9A%E7%A1%85%E8%B0%B7%E5%AD%A6%E4%B9%A0%E6%96%87%E4%BB%B6/MySQL/day05-dao/2023%E5%B9%B47%E6%9C%881%E6%97%A5/%E7%AC%94%E8%AE%B0/images/note02-jdbc/img0115.png)

<br/>

```java
public class BaseDao<T> {
}
```

## 3、BaseDao手动实现

### ①通用增删改方法

```java
/**
 * 通用增删改方法
 * @param sql 执行增删改操作的 SQL 语句
 * @param params 以可变参数形式传入的 SQL 语句的参数
 * @return 执行 SQL 语句之后，受影响的行数
 */
public int commonUpdate(String sql, Object ... params) {

    // 1、声明变量
    Connection connection = JDBCUtils.getConnection();
    PreparedStatement preparedStatement = null;

    try {
        // 2、创建 PreparedStatement 对象
        preparedStatement = connection.prepareStatement(sql);

        // 3、遍历可变参数数组，给 SQL 设置参数
        for (int i = 0; params != null && i < params.length; i++) {
            // 4、从数组中取出具体参数值
            Object param = params[i];

            // 5、设置参数，参数索引从 1 开始，所以是 i + 1
            preparedStatement.setObject(i + 1, param);
        }

        // 6、执行 SQL 语句
        return preparedStatement.executeUpdate();

    } catch (SQLException e) {

        // ※把编译时异常转换为运行时异常继续抛出，这么做是为了避免掩盖问题
        throw new RuntimeException(e);
    } finally {

        // 7、释放资源
        if (preparedStatement != null) {
            try {
                preparedStatement.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }

        JDBCUtils.releaseConnection(connection);

    }
}
```

### ②通用查询方法：返回 List

```java
/**
 * 查询数据库表，每一条记录封装一个实体类对象，多条记录组成 List 集合
 * @param sql
 * @param clazz 当前要查询的实体类对象的 Class 对象
 * @param params
 * @return 查询结果组成的 List 集合
 */
public List<T> getBeanList(String sql, Class<T> clazz, Object ... params) {

    // 1、声明变量
    Connection connection = JDBCUtils.getConnection();
    PreparedStatement preparedStatement = null;
    ResultSet resultSet = null;

    try {
        // 2、创建 PreparedStatement 对象
        preparedStatement = connection.prepareStatement(sql);

        // 3、给 SQL 语句设置参数
        for (int i = 0; params != null && i < params.length; i++) {
            // 4、从数组中取出具体参数值
            Object param = params[i];

            // 5、设置参数，参数索引从 1 开始，所以是 i + 1
            preparedStatement.setObject(i + 1, param);
        }

        // 6、执行 SQL 语句
        resultSet = preparedStatement.executeQuery();

        // 7、创建集合对象，用来存放每一个从结果集中解析得到的 Java 实体类对象
        List<T> list = new ArrayList<>();

        // 8、解析结果集
        while (resultSet.next()) {

            // 9、创建当前指针指向记录对应的 Java 实体类对象
            T instance = clazz.newInstance();

            // ※提出问题：作为通用的查询，此时无法确定字段名称、数量……
            // ※解决问题的前提：Java 实体类的属性和数据库表字段一一对应
            // ※API 层面的支持：Java 反射技术
            // ※API 层面的支持：ResultSetMetaData 结果集元数据
            // 10、获取结果集元数据对象
            ResultSetMetaData resultSetMetaData = resultSet.getMetaData();

            // 11、通过结果集元数据对象获取结果集中字段的数量
            int columnCount = resultSetMetaData.getColumnCount();

            // 12、遍历当前行指针指向的记录
            for (int i = 1; i <= columnCount; i++) {
                // 13、根据循环变量从当前行获取对应单元格的值
                Object value = resultSet.getObject(i);

                // 14、通过反射的方式把 value 存入实体类对象的对应的属性中
                // 假设 value 是 emp_name 字段的值
                // 那么它对应的 Java 实体类对象的属性 empName
                // ※这里我们可以要求 SQL 语句给每一个字段都设置别名，
                // 别名需要正好就是 Java 实体类对象的属性名
                // [1]从结果集元数据中读取当前索引（i）对应的列的别名
                String columnLabel = resultSetMetaData.getColumnLabel(i);

                // [2]以 columnLabel 作为属性名获取 Java 实体类对应的 Field 对象
                Field field = clazz.getDeclaredField(columnLabel);

                // [3]突破 Field 对象的访问限制
                field.setAccessible(true);

                // [4]调用 Field 对象的 set() 方法设置属性值
                field.set(instance, value);
            }

            // 15、把已经填充好的 Java 实体类对象存入 List<T> 集合
            list.add(instance);
        }

        // 16、把填充好的 List<T> 集合作为返回值返回
        return list;

    } catch (Exception e) {
        throw new RuntimeException(e);
    } finally {

        if (resultSet != null) {
            try {
                resultSet.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }

        if (preparedStatement != null) {
            try {
                preparedStatement.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }

        JDBCUtils.releaseConnection(connection);

    }
}
```

### ③通用查询方法：返回单个对象

```java
/**
 * 查询单个实体类对象
 * @param sql
 * @param clazz 当前要查询的实体类对象的 Class 对象
 * @param params
 * @return 单个实体类对象
 */
public T getSingleBean(String sql, Class<T> clazz, Object ... params) {

    // 1、调用查询集合的方法先根据 SQL 语句查询 List 集合
    List<T> beanList = getBeanList(sql, clazz, params);

    // 2、对 beanList 进行判空操作
    if (beanList == null || beanList.size() == 0) {
        return null;
    }

    // 3、检查 beanList 长度是否大于 1
    if (beanList.size() > 1) {
        throw new RuntimeException("too many results!");
    }

    // 4、取出集合中唯一的元素返回
    return beanList.get(0);
}
```

### ④通用批量增删改方法

```sql
/**
 * 通用批量增删改方法
 * @param sql
 * @param paramArrays 二维数组
 *        二维数组中的每一个数组对应 SQL 语句执行一次
 *        insert into t_employee(emp_name, emp_age, emp_salary) values(?,?,?); ["name01", 21, 1000]
 *        insert into t_employee(emp_name, emp_age, emp_salary) values(?,?,?); ["name02", 22, 2000]
 *        insert into t_employee(emp_name, emp_age, emp_salary) values(?,?,?); ["name03", 23, 3000]
 */
public void batchUpdate(String sql, Object[][] paramArrays) {
    // 1、声明变量
    Connection connection = JDBCUtils.getConnection();
    PreparedStatement preparedStatement = null;

    try {
        // 2、创建 PreparedStatement 对象
        preparedStatement = connection.prepareStatement(sql);

        // 3、遍历二维数组
        for (int i = 0; paramArrays != null && i < paramArrays.length; i++) {
            Object[] paramArray = paramArrays[i];

            // 4、遍历可变参数数组，给 SQL 设置参数
            for (int j = 0; paramArray != null && j < paramArray.length; j++) {
                Object param = paramArray[j];

                preparedStatement.setObject(j + 1, param);
            }

            // 5、把上面设置的参数执行一次积攒
            preparedStatement.addBatch();
        }

        // 6、执行 SQL 语句
        preparedStatement.executeBatch();

    } catch (SQLException e) {

        // ※把编译时异常转换为运行时异常继续抛出，这么做是为了避免掩盖问题
        throw new RuntimeException(e);
    } finally {

        // 7、释放资源
        if (preparedStatement != null) {
            try {
                preparedStatement.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }

        JDBCUtils.releaseConnection(connection);

    }
}
```

## 4、BaseDao借助DBUtils实现

### ①DBUtils简介

DBUtils是Apache软件基金会组织维护的一款JDBC工具集，可以帮助我们极大的简化JDBC代码。

<br/>

![img.png](C:/Users/89585/Desktop/%E5%B0%9A%E7%A1%85%E8%B0%B7%E5%AD%A6%E4%B9%A0%E6%96%87%E4%BB%B6/MySQL/day05-dao/2023%E5%B9%B47%E6%9C%881%E6%97%A5/%E7%AC%94%E8%AE%B0/images/note02-jdbc/img0116.png)

<br/>

### ②导入jar包

commons-dbutils-1.7.jar

<br/>

### ③重构BaseDao

```java
public class BaseDaoDBUtilImpl<T> implements BaseDao<T> {

    private QueryRunner queryRunner = new QueryRunner();

    @Override
    public int update(String sql, Object... params) {

        try {
            // 1、获取数据库连接
            Connection connection = JDBCUtils.getConnection();

            // 2、调用 QueryRunner 的方法执行更新操作
            return queryRunner.update(connection, sql, params);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    @Override
    public List<T> selectByCondition(String sql, Class<T> clazz, Object... param) {

        try {
            // 1、获取数据库连接
            Connection connection = JDBCUtils.getConnection();

            // 2、调用 QueryRunner 的方法执行查询操作
            return queryRunner.query(connection, sql, new BeanListHandler<>(clazz), param);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    @Override
    public T selectSingleEntity(String sql, Class<T> clazz, Object... param) {

        try {
            // 1、获取数据库连接
            Connection connection = JDBCUtils.getConnection();

            // 2、调用 QueryRunner 的方法执行查询操作
            return queryRunner.query(connection, sql, new BeanHandler<>(clazz), param);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }
}
```

<br/>