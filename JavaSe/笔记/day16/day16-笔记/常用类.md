## 常用类

### 1. System类

> System类属于系统信息相关类  此类提供了一些方法用于获取系统相关的信息
>
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
 * @author WHD
 * @description TODO
 * @date 2023/6/12 9:06
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
>
> exec(String command)  执行本地的可执行文件 参数为绝对路径
>
> exit(int status) 退出JVM虚拟机
>
> gc() 于运行垃圾回收器
>
> getRuntime() 获取当前类对象
>
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
 * @author WHD
 * @description TODO
 * @date 2023/6/12 9:39
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
 * @author WHD
 * @description TODO
 * @date 2023/6/12 10:17
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
 * @author WHD
 * @description TODO
 * @date 2023/6/12 10:27
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
 * @author WHD
 * @description TODO
 * @date 2023/6/12 10:41
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
 * @author WHD
 * @description TODO
 * @date 2023/6/12 10:46
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
 * @author WHD
 * @description TODO
 * @date 2023/6/12 10:49
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
 * @author WHD
 * @description TODO
 * @date 2023/6/12 10:51
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
 * @author WHD
 * @description TODO
 * @date 2023/6/12 10:55
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
 * @author WHD
 * @description TODO
 * @date 2023/6/12 10:58
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
 * @author WHD
 * @description TODO
 * @date 2023/6/12 14:29
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
 * @author WHD
 * @description TODO
 * @date 2023/6/12 14:50
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
>
> 关于ArrayList懒加载思想：当我们调用ArrayList无参构造方法 底层帮我们维护一个长度为0的数组
>
> 当我们第一次添加元素 将数组的长度改为10 注释为初始化长度为10 的数组是错误的
>
> 当我们添加第11个元素的时候 将数组的长度 扩容1.5倍 即 15
>
> 增删改查效率：
>
> 修改和查询效率高，因为有下标可以通过一次查询就获取到所需要操作的元素 ，修改也需要通过下标查询，所以效率高
>
> 添加如果需要扩容的情况 效率低  
>
> 删除如果需要移动元素的情况 效率低 

#### 2.2 LinkedList

> 常用方法
>
> LinkedList常用方法
>
> add(Object obj) 添加在链表的末尾
>
> add(int index,Object obj) 指定位置添加
>
> addFirst(Object obj) 添加在链表的头部
>
> addLast(Object obj) 添加在链表的尾部
>
>
> remove() 删除第一个元素
>
> remove(int index) 指定位置删除元素
>
> removeFirst() 删除第一个元素
>
> removeLast() 删除最后一个元素
>
>
> get(int index) 指定下标获取元素
>
> getFirst() 获取第一个元素
>
> getLast() 获取最后一个元素
>
>
> set(int index,Object obj) 修改元素
>
>
> clear() 清空集合
>
> isEmpty() 判断集合是否为空
>
> size() 获取元素个数

```java
package com.atguigu.test1;

import java.util.LinkedList;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/13 9:14
 *  LinkedList常用方法
 *  add(Object obj) 添加在链表的末尾
 *  add(int index,Object obj) 指定位置添加
 *  addFirst(Object obj) 添加在链表的头部
 *  addLast(Object obj) 添加在链表的尾部
 *
 *  remove() 删除第一个元素
 *  remove(int index) 指定位置删除元素
 *  removeFirst() 删除第一个元素
 *  removeLast() 删除最后一个元素
 *
 *  get(int index) 指定下标获取元素
 *  getFirst() 获取第一个元素
 *  getLast() 获取最后一个元素
 *
 *  set(int index,Object obj) 修改元素
 *
 *  clear() 清空集合
 *  isEmpty() 判断集合是否为空
 *  size() 获取元素个数
 *
 */
public class TestLinkedListMethod {
    public static void main(String[] args) {
        LinkedList list = new LinkedList();
        list.add("a");
        list.add(123);
        list.add(123.8);
        list.add(123.8F);
        list.add(false);

        System.out.println("----------------------------------------------");

        LinkedList<String> list1 = new LinkedList<>();
        list1.add("abc");
        list1.add("hello");
        list1.add("world");
        list1.add("666");
        list1.add("世界你好");
        list1.add("哈哈哈");

        System.out.println(list1.size());

        list1.add(1, "999");

        list1.addFirst("123456"); // 0

        list1.addLast("嘿嘿嘿");


        System.out.println(list1.get(0));
        System.out.println(list1.getFirst());
        System.out.println(list1.getLast());
        System.out.println(list1.get(1));


        System.out.println(list1.remove(1));
        System.out.println(list1.removeFirst());

        System.out.println(list1.removeLast());

        System.out.println(list1.set(0, "中华人民共和国"));

        System.out.println(list1.getFirst());

        list1.clear();

        System.out.println(list1.isEmpty());


    }
}

```

> LinkedList三种遍历方式
>
> 不推荐使用普通for循环遍历LinkedList  因为效率低

```java
package com.atguigu.test1;

import java.util.Iterator;
import java.util.LinkedList;
import java.util.List;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/13 9:26
 *  LinkedList三种遍历方式
 */
public class TestLinkedListForeach {
    public static void main(String[] args) {
        List<Integer> list = new LinkedList<>();

        list.add(1);
        list.add(2);
        list.add(3);
        list.add(4);
        list.add(5);


        // 方式1 普通for循环遍历
        for(int i = 0;i < list.size();i++){
            System.out.println(list.get(i));
        }

        System.out.println("------------------------------------------------------");

        // 方式2 迭代器方式
        Iterator<Integer> iterator = list.iterator();
        while(iterator.hasNext()){
            System.out.println(iterator.next());
        }

        System.out.println("------------------------------------------------------");


        for(Integer i : list){
            System.out.println("i = " + i);
        }









    }
}

```

> LinkedList集合特点  有序 有下标(注意 因为底层不是数组 所以不同于数组的下标) 可重复 允许null元素 线程不安全
>
> 没有初始长度 没有上限大小 不需要扩容
>
> 底层实现：双向链表   每一个元素是一个Node对象 包含三部分 分别为 上一个元素的引用 下一个元素的引用和当前对象本身
>
>
> 如果根据下标查询 不能直接一次性查询到我们所需要的元素 必须先找到相邻的元素 以此类推 所以根据下标查询慢
>
> 修改效率低 因为修改需要先查询
>
>
> 删除 和 添加 效率高 因为 删除不需要移动元素 添加不需要扩容
>
>
> LinkedList集合get(int index)方法优化：如果查找的下标小于元素个数中间值 从前往后查找
>
> 否则 从后往前查找

#### 2.3 Vector

> Vector 和 ArrayList区别？
>
> Vector是线程安全的 初始数组长度为10  扩容为2倍
>
> ArrayList线程不安全 初始数组长度为0 扩容为1.5倍

```java
package com.atguigu.test2;

import java.util.Vector;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/13 10:31
 *  Vector 和 ArrayList区别？
 *      Vector是线程安全的 初始数组长度为10  扩容为2倍
 *      ArrayList线程不安全 初始数组长度为0 扩容为1.5倍
 *
 *
 */
public class TestVector {
    public static void main(String[] args) {
        Vector<String> vector = new Vector<>();
        vector.add("a");
        vector.add("abc");
        vector.add("hello");
        vector.add("中文");
        vector.add("555");

        String remove = vector.remove(0);

        System.out.println("remove = " + remove);

        System.out.println(vector.size());

        String set = vector.set(0, "666");

        System.out.println("set = " + set);

        System.out.println(vector.get(0));

        // 遍历方式 三种 和 ArrayList相同

        vector.clear();

        vector.isEmpty();
    }
}
```

### 3. Set接口

#### 3.1 HashSet

> HashSet 底层是有HashMap实例维护支持的 使用HashMap中的键来存储HashSet中的元素
>
> 是一个无序  没有下标 允许元素为null 不能重复 线程不安全的 集合
>
> HashSet没有下标 也没有顺序 所有没有查询的方法 只能通过遍历获取所有的数据
>
>
> `面试题：HashSet(HashMap中的键)去除重复的依据？`
>
> 两个对象equals比较为true  并且hashCode相同 则认为是重复元素

```java
package com.atguigu.test4;

import com.atguigu.test3.Student;

import java.util.HashSet;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/13 16:02
 *  HashSet 底层是有HashMap实例维护支持的 使用HashMap中的键来存储HashSet中的元素
 *  是一个无序  没有下标 允许元素为null 不能重复 线程不安全的 集合
 *  HashSet没有下标 也没有顺序 所有没有查询的方法 只能通过遍历获取所有的数据
 *
 *  HashSet(HashMap中的键)去除重复的依据？
 *  两个对象equals比较为true  并且hashCode相同 则认为是重复元素
 *
 */
public class TestHashSet {
    public static void main(String[] args) {
        HashSet<String> set= new HashSet<>();
        set.add("a");
        set.add("b");
        set.add("c");
        set.add("d");
        set.add("e");

        set.remove("a");

        System.out.println(set.size());

        System.out.println(set.contains("b"));

        set.clear();

        System.out.println(set.isEmpty());

        System.out.println("---------------------------------------------");

        HashSet<Student> stuSet = new HashSet<Student>();

        Student stu1 = new Student("大拿", 18);
        Student stu2 = new Student("大拿", 18);

        System.out.println(stuSet.add(stu1));
        System.out.println(stuSet.add(stu2));

        System.out.println(stuSet.size());



    }
}

```



#### 3.2 TreeSet

> TreeSet底层实现为TreeMap  一个有序的Set集合 顺序为根据元素 比较的顺序 自定义类型比较重写Comparable接口中的
>
> compareTo方法 制定比较规则
>
> TreeSet没有下标 也没有顺序 所有没有查询的方法 只能通过遍历获取所有的数据

```java
package com.atguigu.test4;

import java.util.TreeSet;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/13 16:10
 *  TreeSet底层实现为TreeMap  一个有序的Set集合 顺序为根据元素 比较的顺序 自定义类型比较重写Comparable接口中的
 *  compareTo方法 制定比较规则
 *  TreeSet没有下标 也没有顺序 所有没有查询的方法 只能通过遍历获取所有的数据
 */
public class TestTreeSet {
    public static void main(String[] args) {
        TreeSet<String> set = new TreeSet<>();

        set.add("a");
        set.add("b");
        set.add("c");
        set.add("ahello");
        set.add("aworld");

        for(String str : set){
            System.out.println("str = " + str);
        }


    }
}

```



#### 3.3 LinkedHashSet

> LinkedHashSet 一个有序的Set集合 顺序为插入顺序   底层实现为LinkedHashMap

```java
package com.atguigu.test4;

import java.util.LinkedHashSet;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/13 16:15
 *  LinkedHashSet 一个有序的Set集合 顺序为插入顺序   底层实现为LinkedHashMap
 *
 */
public class TestLinkedHashSet {
    public static void main(String[] args) {
        LinkedHashSet<String> set = new LinkedHashSet<>();
        set.add("a");
        set.add("b");
        set.add("c");
        set.add("d");
        set.add("e");

        for (String s : set) {
            System.out.println("s = " + s);
        }


    }
}

```



### 4. Map接口

#### 4.1 HashMap

> 常用方法
>
> HashMap常用方法
>
> put(Object key,Object value) 添加元素
>
> remove(Object key) 根据键删除元素
>
> replace(Object key,Object value) 修改元素
>
> get(Object key ) 获取元素
>
> containsKey(Object key) 是否包含某一个键
>
> containsValue(Object value)  是否包含某一个值
>
> isEmpty() 判断集合是否为空
>
>
> clear() 清空集合
>
> size() 元素个数
>
> entrySet() 获取集合键值对组合
>
> keySet() 获取集合中所有的键
>
> values() 获取集合中所有的值

```java
package com.atguigu.test2;

import java.util.HashMap;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/13 10:43
 *  HashMap常用方法
 *  put(Object key,Object value) 添加元素
 *  remove(Object key) 根据键删除元素
 *  replace(Object key,Object value) 修改元素
 *  get(Object key ) 获取元素
 *  containsKey(Object key) 是否包含某一个键
 *  containsValue(Object value)  是否包含某一个值
 *  isEmpty() 判断集合是否为空
 *
 *  clear() 清空集合
 *  size() 元素个数
 *  entrySet() 获取集合键值对组合
 *  keySet() 获取集合中所有的键
 *  values() 获取集合中所有的值
 *
 */
public class TestHashMapMethod {
    public static void main(String[] args) {
        HashMap<String,String> map = new HashMap();
        map.put("CN", "中国");
        map.put("RU", "俄罗斯");
        map.put("US", "美国");
        map.put("JP", "小日本");
        map.put("KR", "小棒棒");

        map.put("CN", "中华人民共和国");

        System.out.println(map.size());

        System.out.println(map.get("CN"));

        System.out.println("删除元素值为：" +  map.remove("JP"));

        System.out.println(map.size());

        System.out.println(map.replace("KR", "小棒子"));

        System.out.println(map.get("KR"));

        System.out.println(map.containsKey("JP"));

        System.out.println(map.containsKey("CN"));

        System.out.println(map.containsValue("中华人民共和国"));

        map.clear();

        System.out.println(map.isEmpty());
    }
}

```



> 遍历方式
>
>  HashMap六种遍历方式
>
> 1.获取所有的键 根据键获取值
>
> 2.获取所有的值
>
> 3.获取所有的键值对的组合
>
> 4.获取所有的键的迭代器
>
> 5.获取所有的值 的迭代器
>
> 6.获取所有的键值对的组合  的迭代器

```java
package com.atguigu.test2;

import java.util.*;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/13 10:51
 *  HashMap六种遍历方式
 *  1.获取所有的键 根据键获取值
 *  2.获取所有的值
 *  3.获取所有的键值对的组合
 *  4.获取所有的键的迭代器
 *  5.获取所有的值 的迭代器
 *  6.获取所有的键值对的组合  的迭代器
 *
 */
public class TestHashMapForeach {
    public static void main(String[] args) {
        HashMap<String,String> map = new HashMap();
        map.put("CN", "中国");
        map.put("RU", "俄罗斯");
        map.put("US", "美国");
        map.put("JP", "小日本");
        map.put("KR", "小棒棒");

        // 方式1
        Set<String> keys = map.keySet();
        for(String key : keys){
            System.out.println(key + "====" + map.get(key));
        }

        System.out.println("----------------------------------------------");

        // 方式2
        Collection<String> values = map.values();
        for(String value : values){
            System.out.println("value = " + value);
        }

        System.out.println("----------------------------------------------");

        // 方式3
        Set<Map.Entry<String, String>> entries = map.entrySet();

        for(Map.Entry<String,String> entry : entries){
            System.out.println(entry.getKey() + "====" + entry.getValue());
        }

        System.out.println("----------------------------------------------");


        // 方式4
        Iterator<String> it1 = map.keySet().iterator();
        while(it1.hasNext()){
            String next = it1.next();
            System.out.println( next + "====" + map.get(next));
        }
        System.out.println("----------------------------------------------");

        // 方式5
        Iterator<String> it2 = map.values().iterator();
        while(it2.hasNext()){
            System.out.println(it2.next());
        }

        System.out.println("----------------------------------------------");


        // 方式6
        Iterator<Map.Entry<String, String>> it3 = map.entrySet().iterator();
        while(it3.hasNext()){
            Map.Entry<String, String> next = it3.next();
            System.out.println(next.getKey() + "====" + next.getValue());
        }
    }
}

```

> 特点：无序 没有下标 线程不安全 允许键和值为null 键不能重复 值可以重复
> *
>
> 回顾我们之前所学习集合的数据结构：
>
> ArrayList 数组    因为空间连续      所以查询 修改 快   添加(扩容) 删除(移动元素) 慢
>
> LinkedList 双向链表 因为空间不连续   所以添加 删除 快   查询 修改 慢
>
>
> HashMap数据结构：
>
> JDK1.7 数组 + 单向链表
>
> JDK1.8 数组 + 单向链表 + 红黑树
>
>
> 存储元素过程：HashMap一个元素包含四部分 分别为 key值 value值 根据key计算的hash值 和 指向下一个元素的引用
>
> 当我们添加元素 先维护一个长度为16的数组 根据key计算的hash值对数组长度-1 进行&运算 得到的值作为下标存放在
>
> 数组对应的位置
>
> 如果此位置为null 则直接存放
>
> 如果此位置不为null  则判断key和hash值是否完全相同
>
> ​	如果完全相同 则直接覆盖
>
> ​	如果不同
>
> ​	判断是树节点 还是链表节点 分别以不同的方式添加
>
>
> HashMap扩容 当数组的使用率达到75% 就扩容2倍 是指数组的长度*2
>
> 关于转换红黑树 当链表的个数大于8 并且数组的长度大于64 单向链表转换为红黑树
>
> 当链表的长度小于6 红黑树转换为单向链表
>
> 虽然JDK8中加入了红黑树 但是数组 + 单向链表是常态 转换为红黑树的概率小于千万分之一

> 1.每个节点只能是红色或者黑色。
>
> 2.根节点必须是黑色。
>
>  3.红色的节点，它的叶节点只能是黑色。
>
> 4.从任一节点到其每个叶子的所有路径都包含相同数目的黑色节点。
>
> 由以上四个特性我们可以看出一些红黑树的特点：
>
> ​        从根基节点到最远叶节点的路径（最远路径）肯定不会大于根基节点到最近节点的路径（最短路径）的两倍长。这是因为性质3保证了没有相连的红色节点，性质4保证了从这个节点出发的不管是哪一条路径必须有相同数目的黑色节点，这也保证了有一条路径不能比其他任意一条路径的两倍还要长。

#### 4.2 Hashtable

>  Hashtable和HashMap区别？
>
> HashMap线程不安全 Hashtable线程安全
>
> HashMap允许键或者值为null  Hashtable 不允许键或者值为null
>
> HashMap初始数组长度为16 扩容为2倍  扩容方法为resize()
>
> Hashtable初始数组长度为11 扩容为2 倍+ 1 扩容方法为rehash();

```java
package com.atguigu.test3;

import java.util.Hashtable;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/13 15:24
 *  Hashtable和HashMap区别？
 *  HashMap线程不安全 Hashtable线程安全
 *  HashMap允许键或者值为null  Hashtable 不允许键或者值为null
 *  HashMap初始数组长度为16 扩容为2倍  扩容方法为resize()
 *  Hashtable初始数组长度为11 扩容为2 倍+ 1 扩容方法为rehash();
 */
public class TestHashtable {
    public static void main(String[] args) {
        Hashtable<Integer,String> table = new Hashtable<>();
        table.put(1, "a");
        table.put(2, "b");
        table.put(3, "c");
        table.put(4, "d");
        table.put(5, "e");

        System.out.println(table.remove(5));

        System.out.println(table.replace(4, "hello"));

        System.out.println(table.get(4));

        // 遍历方式和HashMap相同


    }
}

```

#### 4.3 Properties

> Properties类属于JDK提供的只让我们存储字符串内容的一个Map集合  继承自Hashtable
>
> 不推荐使用put或者putAll方法 应该使用setProperty() 方法

```java
package com.atguigu.test3;

import java.util.Properties;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/13 15:30
 *  Properties类属于JDK提供的只让我们存储字符串内容的一个Map集合  继承自Hashtable
 *  不推荐使用put或者putAll方法 应该使用setProperty() 方法
 */
public class TestProperties {
    public static void main(String[] args) {
        Properties properties = System.getProperties();
//        properties.put(true, 'a');
        properties.list(System.out);

        Properties properties1 = new Properties();
        properties1.setProperty("a1", "abc");
        properties1.setProperty("a2", "abc");
        properties1.setProperty("a3", "abc");
        properties1.setProperty("a4", "abc");
        properties1.setProperty("a5", "abc");

        properties1.list(System.out);

    }
}

```

#### 4.4TreeMap

>  TreeMap 一个基于树结构的Map集合 有序的Map集合 顺序为根据键比较的顺序
>
> 使用自定义的数据类型来作为TreeMap的键 必须实现Comparable接口 重写 compareTo方法 

```java
package com.atguigu.test3;

import java.util.Map;
import java.util.Set;
import java.util.TreeMap;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/13 15:40
 *  TreeMap 一个基于树结构的Map集合 有序的Map集合 顺序为根据键比较的顺序
 */
public class TestTreeMap {
    public static void main(String[] args) {
        TreeMap<Integer,String> map = new TreeMap<>();
        map.put(123, "abc1");
        map.put(156, "abc2");
        map.put(12, "abc3");
        map.put(78, "abc4");
        map.put(10, "abc5");

        Set<Map.Entry<Integer, String>> entries = map.entrySet();

        for (Map.Entry<Integer, String> entry : entries) {
            // System.out.println(entry.getKey() + "====" + entry.getValue());
            System.out.println(entry);
        }


    }
}

```

```java
package com.atguigu.test3;

import java.util.Iterator;
import java.util.Map;
import java.util.TreeMap;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/13 15:43
 */
public class Student implements  Comparable<Student>{
    private String name;
    private int age;

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public Student() {
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public static void main(String[] args) {
        Student stu1 = new Student("大拿", 18);
        Student stu2 = new Student("赵四", 19);

        TreeMap<Student,String> map = new TreeMap<>();

        map.put(stu1, "abc");
        map.put(stu2, "hello");

        Iterator<Map.Entry<Student, String>> iterator = map.entrySet().iterator();
        while(iterator.hasNext()){
            System.out.println(iterator.next());
        }


        System.out.println(map.size());

    }


    @Override
    public int compareTo(Student stu) {
//        if(this.getAge() > stu.getAge()){
//            return 1;
//        }else if(this.getAge() < stu.getAge()){
//            return -1;
//        }
//        return 0;
//        return this.getAge() > stu.getAge() ? 1 : (this.getAge() == stu.getAge() ? 0 : -1);
        return stu.getAge() -this.getAge();
    }
}

```

