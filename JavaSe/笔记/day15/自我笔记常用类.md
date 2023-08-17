## 常用类

### 1. System类

> System类属于系统信息相关类  此类提供了一些方法用于获取系统相关的信息
>
> **`面试题：final  finally finalize 三者的区别？`**
>
> final关键字用于修饰属性(常量) 方法(不能重写) 类(不能继承)
>
> finally表示异常处理关键字 表示任何情况都执行的代码
>
> finalize 表示当前类对象被垃圾回收器回收时自动执行的方法 属于 (析构函数)
>
> 
>
> arraycopy(Object src, int srcPos, Object dest, int destPos, int length) 复制数组
>
> currentTimeMillis() 获取当前系统时间毫秒数 从1970年1月1号0点0分0秒到目前
>
> exit(int status) 退出JVM虚拟机
>
> gc() 运行垃圾回收器 表示手动调用垃圾回收器 默认情况下 垃圾回收其会自动运行 并且回收垃圾
>
> 手动运行表示立即执行检测 并且判断 针对可以被回收的对象进行回收 但是并不能保证一定会回收某些对象
>
> 
>
> getProperties()  获取系统相关所有的属性
>
> getProperty(String key)  根据属性名获取对应的属性值
>
> nanoTime() 获取当前系统时间纳秒数 从1970年1月1号0点0分0秒到目前

```java
package com.atguigu.test1;

import java.util.Properties;

/**
 *  System类属于系统信息相关类  此类提供了一些方法用于获取系统相关的信息
 *
 *  面试题：final  finally finalize 三者的区别？
 *  final关键字用于修饰属性(常量) 方法(不能重写) 类(不能继承)
 *  finally表示异常处理关键字 表示任何情况都执行的代码
 *  finalize 表示当前类对象被垃圾回收器回收时自动执行的方法 属于 (析构函数)
 *
 *
 *
 *  arraycopy(Object src, int srcPos, Object dest, int destPos, int length) 复制数组
 *  currentTimeMillis() 获取当前系统时间毫秒数 从1970年1月1号0点0分0秒到目前
 *  exit(int status) 退出JVM虚拟机
 *  gc() 运行垃圾回收器 表示手动调用垃圾回收器 默认情况下 垃圾回收其会自动运行 并且回收垃圾
 *  手动运行表示立即执行检测 并且判断 针对可以被回收的对象进行回收 但是并不能保证一定会回收某些对象
 *
 *
 *
 *  getProperties()  获取系统相关所有的属性
 *  getProperty(String key)  根据属性名获取对应的属性值
 *  nanoTime() 获取当前系统时间纳秒数 从1970年1月1号0点0分0秒到目前
 *
 */
public class TestSystem {
    public static void main(String[] args) {
//       System system = new System();

        long millis = System.currentTimeMillis();

        System.out.println("millis = " + millis);

        long nano = System.nanoTime();

        System.out.println("nano = " + nano);

        Student stu = new Student();

        stu = null;

        System.gc();

        System.out.println("stu = " + stu);

        System.out.println("nano = " + nano);


        Properties properties = System.getProperties();

        String property1 = properties.getProperty("user.name");
        System.out.println("property = " + property1);

        String property2 = properties.getProperty("java.version");
        System.out.println("property2 = " + property2);

        String property3 = properties.getProperty("os.version");
        System.out.println("property3 = " + property3);

        String property4 = properties.getProperty("os.name");
        System.out.println("property4 = " + property4);


        properties.list(System.out);



        System.exit(1);

    }
}

```



### 2. Runtime类

> Runtime类被设计为单例模式的 此类对象在内存中只允许存在一份
>
> exec(String command)  执行本地的可执行文件 参数为绝对路径
>
> exit(int status) 退出JVM虚拟机
>
> gc() 于运行垃圾回收器
>
> getRuntime() 获取当前类对象
>
> freeMemory() 获取当前JVM空闲内存  单位是字节
>
> maxMemory()  获取当前JVM最大内存  单位是字节
>
> totalMemory() 获取当前JVM总内存 单位是字节

```java
package com.atguigu.test1;

import java.io.IOException;

/**
 *  Runtime类被设计为单例模式的 此类对象在内存中只允许存在一份
 *
 *  exec(String command)  执行本地的可执行文件 参数为绝对路径
 *  exit(int status) 退出JVM虚拟机
 *  gc() 于运行垃圾回收器
 *  getRuntime() 获取当前类对象
 *
 *  freeMemory() 获取当前JVM空闲内存  单位是字节
 *  maxMemory()  获取当前JVM最大内存  单位是字节
 *  totalMemory() 获取当前JVM总内存 单位是字节
 *
 */
public class TestRuntime {
    public static void main(String[] args) throws IOException {
        Runtime runtime1 = Runtime.getRuntime();
        Runtime runtime2= Runtime.getRuntime();

        System.out.println(runtime1 == runtime2);

        runtime1.exec("D:\\funny\\10秒让整个屏幕开满玫瑰花\\点我.exe");

        System.out.println("空闲内存：" + runtime1.freeMemory() / 1024 / 1024);
        System.out.println("最大内存：" + runtime1.maxMemory() / 1024 / 1024);
        System.out.println("总内存：" + runtime1.totalMemory() / 1024 / 1024);


        runtime1.gc();

        runtime1.exit(1);
    }
}

```



### 3. 日期相关的API

#### 3.1 java.util.Date

> java.util.Date 日期类 提供了常用的日期相关的方法

```java
package com.atguigu.test2;

import java.util.Date;

/**
 *  java.util.Date 日期类 提供了常用的日期相关的方法
 *
 *
 */
public class TestDate {
    public static void main(String[] args) {
        Date date1 = new Date();

        System.out.println(date1);

        System.out.println(date1.getYear() + 1900 +  "年");
        System.out.println(date1.getMonth() + 1 + "月");
        System.out.println(date1.getDate() + "日");
        System.out.println(date1.getDay() + "一周中第几天");
        System.out.println(date1.getHours() + "小时");
        System.out.println(date1.getMinutes() + "分钟");
        System.out.println(date1.getSeconds() + "秒钟");



        Date date2 = new Date(4154784578784L);
        System.out.println("date2 = " + date2);

        Date date3 = new Date(System.currentTimeMillis());
        System.out.println("date3 = " + date3);

        Date date4 = new Date(123, 10, 10, 1, 1, 1);
        System.out.println("date4 = " + date4);



    }

    @Deprecated
    public static void m1(){

    }

}

```

> 日期格式化类 此类用于解析 以及 格式化日期
>
> SimpleDateFormat() 无参构造 表示以默认格式解析个格式化日期 23-6-12 上午10:34
>
> SimpleDateFormat(String pattern) 有参构造 以指定格式解析和格式化日期
>
> parse(String str) 解析：将字符串转换为日期对象  String--->Date
>
> format(Date date) 格式化：将日期对象格式化为我们所想要的字符串  Date ----> String

```java
package com.atguigu.test2;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

/**
 *  日期格式化类 此类用于解析 以及 格式化日期
 *  SimpleDateFormat() 无参构造 表示以默认格式解析个格式化日期 23-6-12 上午10:34
 *  SimpleDateFormat(String pattern) 有参构造 以指定格式解析和格式化日期
 *
 *  parse(String str) 解析：将字符串转换为日期对象  String--->Date
 *  format(Date date) 格式化：将日期对象格式化为我们所想要的字符串  Date ----> String
 */
public class TestSimpleDateFormat {
    public static void main(String[] args) throws ParseException {
        SimpleDateFormat sdf1 = new SimpleDateFormat();

        Date date1 = new Date();

        String formatDate1 = sdf1.format(date1);

        System.out.println("formatDate1 = " + formatDate1);


        Date parseDate1 = sdf1.parse("23-6-12 上午10:32");

        System.out.println("parseDate1 = " + parseDate1);

        SimpleDateFormat sdf2 = new SimpleDateFormat("yyyy年MM月dd日 HH-mm-ss");

        String format = sdf2.format(new Date());

        System.out.println("format = " + format);

        Date parse = sdf2.parse("2021年12月12日 10-11-12");

        System.out.println("parse = " + parse);



    }
}

```



#### 3.2 Calendar类

> 日历类

```java
package com.atguigu.test2;

import java.util.Calendar;

/**
 *  日历类
 */
public class TestCalendar {
    public static void main(String[] args) {
        Calendar instance = Calendar.getInstance();

        System.out.println(instance.get(Calendar.YEAR) + "年");
        System.out.println(instance.get(Calendar.MONTH) + 1 +  "月");
        System.out.println(instance.get(Calendar.DAY_OF_MONTH) + "一个月中第天");
        System.out.println(instance.get(Calendar.DAY_OF_WEEK) + "一个周中第天");
        System.out.println(instance.get(Calendar.HOUR) + "时");
        System.out.println(instance.get(Calendar.MINUTE) + "分");
        System.out.println(instance.get(Calendar.SECOND) + "秒");


    }
}

```



### 4. JDK8相关的日期API

> LocalDate 只能保存年月日的类

```java
package com.atguigu.test2;

import java.time.LocalDate;

/**
 *  LocalDate 只能保存年月日的类
 */
public class TestLocalDate {
    public static void main(String[] args) {
        LocalDate now = LocalDate.now();
        System.out.println(now.getYear());
        System.out.println(now.getMonth());
        System.out.println(now.getDayOfMonth());
        System.out.println(now.getDayOfYear());


        System.out.println("now = " + now);

        LocalDate of = LocalDate.of(2022, 12, 12);
        System.out.println("of = " + of);

    }
}

```

> LocalTime只包含时分秒

```java
package com.atguigu.test2;

import java.time.LocalTime;

/**
 *  LocalTime只包含时分秒
 *
 */
public class TestLocalTime {
    public static void main(String[] args) {
        LocalTime now = LocalTime.now();
        System.out.println(now.getHour());
        System.out.println(now.getMinute());
        System.out.println(now.getSecond());


        System.out.println(now);

        LocalTime of = LocalTime.of(12, 12, 12);
        System.out.println("of = " + of);





    }
}

```

> 包含年月日时分秒

```java
package com.atguigu.test2;

import java.time.LocalDateTime;

/**
 *  包含年月日时分秒
 */
public class TestLocalDateTime {
    public static void main(String[] args) {
        LocalDateTime now = LocalDateTime.now();
        System.out.println("now = " + now);

        LocalDateTime of = LocalDateTime.of(1998, 12, 12, 12, 12, 12);

        System.out.println("of = " + of);

    }
}

```



### 5. BigDecimal和BigInteger

> BigDecimal支持任意精度 任意长度的精确值 浮点数

```java
package com.atguigu.test2;

import java.math.BigDecimal;
import java.math.RoundingMode;

/**
 *  BigDecimal支持任意精度 任意长度的精确值 浮点数
 */
public class TestBigDecimal {
    public static void main(String[] args) {
        BigDecimal bigDecimal1 = new BigDecimal("546873285416526358541035263156103515415610.6523596541555263521063526352632");

        BigDecimal bigDecimal2 = new BigDecimal("59885629856.965596596");


        System.out.println(bigDecimal1.add(bigDecimal2));
        System.out.println(bigDecimal1.subtract(bigDecimal2));
        System.out.println(bigDecimal1.multiply(bigDecimal2));
        System.out.println(bigDecimal1.divide(bigDecimal2, 20, RoundingMode.HALF_UP));


    }
}

```

> BigInteger 任意长度的整数

```java
package com.atguigu.test2;

import java.math.BigInteger;

/**
 * BigInteger 任意长度的整数
 */
public class TestBigInteger {
    public static void main(String[] args) {
        BigInteger bigInteger1 = new BigInteger("79657475465497516574561789626596415748959646545");
        BigInteger bigInteger2 = new BigInteger("79657475465748959646545");

        System.out.println(bigInteger1.add(bigInteger2));
        System.out.println(bigInteger1.subtract(bigInteger2));
        System.out.println(bigInteger1.multiply(bigInteger2));
        System.out.println(bigInteger1.divide(bigInteger2));


    }
}

```



## 集合

### 1. Collection

### 2. List接口

#### 2.1 ArrayList

> 常用方法
>
> ArrayList常用方法  最重要的方法为 CRUD  增删改查
>
> add() 在末尾添加元素
>
> add(int index, E element)  指定位置添加元素
>
> clear() 清空集合
>
> contains(Object o) 判断集合中是否包含某一个元素
>
> get(int index) 根据下标获取元素
>
> indexOf(Object o) 判断集合中第一次出现的位置  未找到返回-1
>
> isEmpty() 判断集合是否为空
>
> iterator() 获取集合迭代器
>
> lastIndexOf(Object o) 判断集合中最后一次出现的位置  未找到返回-1
>
> remove(int index) 根据下标删除元素
>
> remove(Object o) 根据对象删除元素
>
> set(int index, E element) 修改元素
>
> size()  获取集合长度
>
> 
>
> 泛型：用于指定集合中的数据类型

```java
package com.atguigu.test4;

import com.atguigu.test1.Student;

import java.util.ArrayList;

/**
 *  ArrayList常用方法  最重要的方法为 CRUD  增删改查
 *  add() 在末尾添加元素
 *  add(int index, E element)  指定位置添加元素
 *  clear() 清空集合
 *  contains(Object o) 判断集合中是否包含某一个元素
 *  get(int index) 根据下标获取元素
 *  indexOf(Object o) 判断集合中第一次出现的位置  未找到返回-1
 *  isEmpty() 判断集合是否为空
 *  iterator() 获取集合迭代器
 *  lastIndexOf(Object o) 判断集合中最后一次出现的位置  未找到返回-1
 *  remove(int index) 根据下标删除元素
 *  remove(Object o) 根据对象删除元素
 *  set(int index, E element) 修改元素
 *  size()  获取集合长度
 *
 *
 *  泛型：用于指定集合中的数据类型
 */
public class TestArrayListMethod {
    public static void main(String[] args) {
        ArrayList list = new ArrayList();
        list.add(100);
        list.add(10.5);
        list.add(10.5F);
        list.add("abc");
        list.add(true);
        list.add(new Student());
        list.add(new Student[]{  new Student(),  new Student()});


        System.out.println("-------------------------------------------");

        ArrayList<Integer> list1 = new ArrayList<Integer>();
        list1.add(100);
        list1.add(101);
        list1.add(102);
        list1.add(103);
        list1.add(1, 666); // 插入

        System.out.println("集合的长度" +  list1.size());

        System.out.println(list1.get(0));
        System.out.println(list1.get(1));
        System.out.println(list1.get(2));
        System.out.println(list1.get(3));
        System.out.println(list1.get(4));


        Integer remove = list1.remove(0);

        System.out.println("remove = " + remove);

        System.out.println("集合的长度" +  list1.size());


        boolean flag = list1.remove(new Integer(666));

        System.out.println("flag = " + flag);

        System.out.println("集合的长度" +  list1.size());

        System.out.println("修改之前的内容为：" + list1.set(0, 999)); // 将101修改为999


        System.out.println(list1.get(0));

        System.out.println(list1.indexOf(999));

        list1.add(999);

        System.out.println(list1.lastIndexOf(999));

        System.out.println("集合中是否包含999" +  list1.contains(999));

        list1.clear();

        System.out.println("集合是否为空：" +  list1.isEmpty());

        System.out.println("======================================================");

        ArrayList<Student> list3 = new ArrayList<>();

        Student stu1 = new Student("赵四", 20);
        Student stu2 = new Student("赵四", 20);

        list3.add(stu1);
        list3.add(stu2);

        System.out.println(list3.size());

        Student stu3 = new Student("赵四", 20);


        System.out.println(list3.contains(stu3));


    }


}

```

> ArrayList三种遍历方式
>
> 1.普通for循环
>
> 2.迭代器
>
> 3.增强for循环

```java
package com.atguigu.test4;

import java.util.ArrayList;
import java.util.Iterator;

/**
 *  ArrayList三种遍历方式
 *  1.普通for循环
 *  2.迭代器
 *  3.增强for循环
 */
public class TestArrayListForeach {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        list.add("a");
        list.add("b");
        list.add("c");
        list.add("d");
        list.add("e");

        for(int i = 0;i < list.size();i++){
            System.out.println(list.get(i));
        }

        System.out.println("----------------------------------------------");


        Iterator<String> iterator = list.iterator();
        while(iterator.hasNext()){
            System.out.println(iterator.next());
        }

        System.out.println("----------------------------------------------");

        // 增强for循环是JDK1.5新增的内容 底层实现还是迭代器
        for(String str : list){
            System.out.println(str);
        }



    }
}

```

> 集合特点：
>
> ArrayList集合特点 ：有序 有下标 可重复  线程不安全 允许元素为null
>
> 关于ArrayList懒加载思想：当我们调用ArrayList无参构造方法 底层帮我们维护一个长度为0的数组
>
> 当我们第一次添加元素 将数组的长度改为10 注释为初始化长度为10 的数组是错误的
>
> 当我们添加第11个元素的时候 将数组的长度 扩容1.5倍 即 15

#### 2.2 LinkedList

### 3. Set接口

### 4. Map接口

