## 常用类

### 1.枚举类

> 枚举是JDK1.5新增的内容
>
> 枚举是由一组固定的常量组成的数据类型 所以枚举中的值推荐全部大写
>
> 所有的枚举类 都默认继承自java.lang.Enum这个类 所以枚举类不能再继承其他类
>
> 但是可以实现接口
>
> 枚举类不能new对象

```java
package com.atguigu.test1;

/**
 *  枚举是JDK1.5新增的内容
 *  枚举是由一组固定的常量组成的数据类型 所以枚举中的值推荐全部大写
 *  所有的枚举类 都默认继承自java.lang.Enum这个类 所以枚举类不能再继承其他类
 *  但是可以实现接口
 *  枚举类不能new对象
 */
public enum Gender {
    MALE,FEMALE
}

```

```java
package com.atguigu.test1;

public class Student {
    String name;
    String sex;
    Gender gender;

    public static void main(String[] args) {
        Student stu1 = new Student();
        stu1.name = "赵四";
        stu1.sex = "hello world";
        stu1.gender = Gender.MALE;

        Student stu2 = new Student();
        stu2.gender = Gender.FEMALE;
    }

}

```

```java
package com.atguigu.test1;

public class TestWeek {
    public static void main(String[] args) {
        Week day = Week.SAT;
        switch(day){
            case MON:
                System.out.println("周一");
                break;
            case TUE:
                System.out.println("周二");
                break;
            case WED:
                System.out.println("周三");
                break;
            case THUR:
                System.out.println("周四");
                break;
            case FRI:
                System.out.println("周五");
                break;
            case SAT:
                System.out.println("周六");
                break;
            case SUN:
                System.out.println("周日");
                break;

        }

        System.out.println("---------------------------------------");

        Week week = Week.FRI;

        printWeek(week);
    }

    public static void printWeek(Week day){
        switch(day){
            case MON:
                System.out.println("周一");
                break;
            case TUE:
                System.out.println("周二");
                break;
            case WED:
                System.out.println("周三");
                break;
            case THUR:
                System.out.println("周四");
                break;
            case FRI:
                System.out.println("周五");
                break;
            case SAT:
                System.out.println("周六");
                break;
            case SUN:
                System.out.println("周日");
                break;

        }
    }


}

```

```java
package com.atguigu.test1;

public class TestWorker {
    public static void main(String[] args) {
        Worker cto = Worker.getWorkerById("sz6666");
        System.out.println(cto);
        cto.doWork();
    }
}

```

```java
package com.atguigu.test1;

/**
 *  枚举类中可以写普通的属性 普通方法 构造方法 静态方法
 *  枚举类中都会存在一个values() 方法 此方法为JVM编译以后自动帮我们添加的方法
 *  方法的作用为将此枚举类中的具体的值转换为一个数组
 */
public enum Worker implements  Work{
    MANAGER("赵四",'男',25,"sz8888"),
    CEO("大拿",'男',30,"sz9999"),
    CTO("刘能",'男',35,"sz6666")
    ;

    private String name;
    private char sex;
    private int age;
    private String workerId;

    Worker(String name, char sex, int age, String workerId) {
        this.name = name;
        this.sex = sex;
        this.age = age;
        this.workerId = workerId;
    }

    public static Worker getWorkerById(String workerId){
        Worker[] values = Worker.values();
        for(int i = 0;i < values.length;i++){
            if(values[i].workerId.equals(workerId)){
                return values[i];
            }

        }
        return null;
    }

    @Override
    public String toString() {
        return "Worker{" +
                "name='" + name + '\'' +
                ", sex=" + sex +
                ", age=" + age +
                ", workerId='" + workerId + '\'' +
                "} " + super.toString();
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public char getSex() {
        return sex;
    }

    public void setSex(char sex) {
        this.sex = sex;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getWorkerId() {
        return workerId;
    }

    public void setWorkerId(String workerId) {
        this.workerId = workerId;
    }

    @Override
    public void doWork() {
        System.out.println(name + "在工作");
    }
}

```

### 2.包装类

> 包装类构造方法
>
> 每一个包装类都支持传入一个与之对应的基本数据类型 构造当前类实例
>
> 除了Character类之外 其他的包装类还支持传入一个字符串
> 
>
> 使用字符串构造Number子类实例 字符串不能为null 且必须可以解析为正确的数值才可以
>
> 否则将出现 java.lang.NumberFormatException 数字格式化异常

```java
package com.atguigu.test2;

/**
 *  包装类构造方法
 *  每一个包装类都支持传入一个与之对应的基本数据类型 构造当前类实例
 *  除了Character类之外 其他的包装类还支持传入一个字符串
 *
 *  使用字符串构造Number子类实例 字符串不能为null 且必须可以解析为正确的数值才可以
 *  否则将出现 java.lang.NumberFormatException 数字格式化异常
 *
 */
public class TestConstructors {
    public static void main(String[] args) {
        Byte b1 = new Byte("123");
        System.out.println("b1 = " + b1);
        Byte b2 = new Byte((byte)123);
        System.out.println("b2 = " + b2);

        Short s1 = new Short("123");
        Short s2 = new Short((short)123);
        System.out.println("s1 = " + s1);
        System.out.println("s2 = " + s2);

        Integer i1 = new Integer(123);
        System.out.println("i1 = " + i1);
        Integer i2 = new Integer("123");
        System.out.println("i2 = " + i2);

        Long l1 = new Long(124);
        System.out.println("l1 = " + l1);
        Long l2 = new Long("124");
        System.out.println("l2 = " + l2);


        Float f1 = new Float(3.4F);
        System.out.println("f1 = " + f1);
        Float f2 = new Float("3.4F");
        System.out.println("f2 = " + f2);

        Double d1 = new Double(2.5);
        System.out.println("d1 = " + d1);
        Double d2 = new Double("2.5");
        System.out.println("d2 = " + d2);

        Character ch1 = new Character('a');
        System.out.println("ch1 = " + ch1);

        Boolean bl1 = new Boolean(true);
        System.out.println("bl1 = " + bl1);
        // 使用字符串构造Boolean实例  不区分大小写 如果内容为true 则为true 其他的都为false 为null也为false
        Boolean bl2 = new Boolean(null);
        System.out.println("bl2 = " + bl2);



    }
}

```

### 3.自动拆箱和装箱

> 从JDK1.5开始 支持装箱和拆箱
>
> 装箱：将基本数据类型自动转换为包装类对象
>
> 拆箱：将包装类对象自动转换为基本数据类型
> 
>
> ==和equals的区别？
>
> Short Integer Long Character 包装类直接使用=号赋值的情况
> 
>
> 如果取值范围在byte以内 那么使用==比较为true
>
> 因为直接从缓存数组中取出对应的对象 多次取出为同一个对象 所以比较为true
> 
>
> 如果取值范围超出byte ==比较为false
>
> 因为直接new的新的对象 所以地址不同 所以比较为false

```java
package com.atguigu.test3;

/**
 *  从JDK1.5开始 支持装箱和拆箱
 *  装箱：将基本数据类型自动转换为包装类对象
 *  拆箱：将包装类对象自动转换为基本数据类型
 *
 *  ==和equals的区别？
 *
 *  Short Integer Long Character 包装类直接使用=号赋值的情况
 *
 *  如果取值范围在byte以内 那么使用==比较为true
 *  因为直接从缓存数组中取出对应的对象 多次取出为同一个对象 所以比较为true
 *
 *  如果取值范围超出byte ==比较为false
 *  因为直接new的新的对象 所以地址不同 所以比较为false
 *
 */
public class TestInterView {
    public static void main(String[] args) {
        Integer i2 = new Integer(100);

        Integer i3 = Integer.valueOf(100);

        Integer i1 = 200; // 自动装箱 JVM自动帮我们调用valueOf()方法
        int i4 = i1; // 自动拆箱 JVM自动帮我们调用intValue();

        System.out.println("-------------------------------------------------------");


        Integer a = new Integer(100);
        Integer b = new Integer(100);
        System.out.println(a == b);


        Integer c = -129;
        Integer d = -129;
        System.out.println(c == d);

        Character ch1 = 100;
        Character ch2 = 100;
        System.out.println(ch1 == ch2);

        Character ch3 = 128;
        Character ch4 = 128;
        System.out.println( ch3 == ch4);

        System.out.println("-------------------------------------------------------");

        System.out.println(Long.MAX_VALUE);

        System.out.println(Integer.MAX_VALUE);

        System.out.println(Short.MAX_VALUE);

        System.out.println(Byte.MIN_VALUE);

    }
}

```

> *除了**Character**包装类以外 其他的包装类都提供有* *parseXxx**方法 用于将字符串转换为基本数据类型*

```java
package com.atguigu.test3;

/**
 *  除了Character包装类以外 其他的包装类都提供有 parseXxx方法 用于将字符串转换为基本数据类型
 *
 */
public class TestParseXxx {
    public static void main(String[] args) {
        byte b = Byte.parseByte("123");
        System.out.println("b = " + b);

        short i = Short.parseShort("123");
        System.out.println("i = " + i);

        int i1 = Integer.parseInt("123");
        System.out.println("i1 = " + i1);

        long l = Long.parseLong("123");
        System.out.println("l = " + l);

        float v = Float.parseFloat("3.5F");
        System.out.println("v = " + v);

        double v1 = Double.parseDouble("3.5");
        System.out.println("v1 = " + v1);

        boolean abc = Boolean.parseBoolean("abc");
        System.out.println("abc = " + abc);


    }
}

```

> *每个包装类都有**toString**方法 用于将基本数据类型转换为字符串*

> *每一个包装类都提供有**valueOf**方法 用于将基本数据类型转化为包装类对象*

```java
String s2 = Integer.toString(123);
System.out.println("s2 = " + s2);

String s5 = Double.toString(3.5);
System.out.println("s5 = " + s5);

String a = Character.toString('a');
System.out.println("a = " + a);

Integer integer = Integer.valueOf(123);
System.out.println("integer = " + integer);

Double aDouble = Double.valueOf(3.5);
System.out.println("aDouble = " + aDouble);

Character character = Character.valueOf('a');
System.out.println("character = " + character);



```

> *所有的包装类都提供有 用于将 包装类对象 转换为 基本数据类型的方法*

```java
package com.atguigu.test3;

/**
 *  所有的包装类都提供有 用于将 包装类对象 转换为 基本数据类型的方法
 *  xxxValue()
 */
public class TestXxxValue {
    public static void main(String[] args) {
        Byte b1 = new Byte("123");
        byte b = b1.byteValue();
        System.out.println("b = " + b);


        Short s1 = new Short("123");
        short i = s1.shortValue();
        System.out.println("i = " + i);

        Integer i1 = new Integer(123);
        int i2 = i1.intValue();
        System.out.println("i2 = " + i2);

        Long l1 = new Long(123);
        long l = l1.longValue();
        System.out.println("l = " + l);

        Boolean bl1 = new Boolean(true);
        boolean b2 = bl1.booleanValue();
        System.out.println("b2 = " + b2);


        Character ch1 = new Character('a');
        char c = ch1.charValue();
        System.out.println("c = " + c);


        Float f1 = new Float(3.5F);
        float x = f1.floatValue();
        System.out.println(x);

        Double d1 = new Double(2.5);
        double v = d1.doubleValue();
        System.out.println("v = " + v);

    }
}

```

### 4.Math类

> Math类 数学工具类 此类中的方法全部为静态方法 提供的有常用的数学计算的方法   两个静态常量：E   PI
>
> abs() 绝对值
>
> ceil() 向上取整
>
> floor() 向下取整
>
> max() 最大值
>
> min() 最小值
>
> round() 四舍五入
>
> random() 随机数

```java
package com.atguigu.test4;

/**
 *  Math类 数学工具类 此类中的方法全部为静态方法 提供的有常用的数学计算的方法   两个静态常量：E   PI
 *  abs() 绝对值
 *  ceil() 向上取整
 *  floor() 向下取整
 *  max() 最大值
 *  min() 最小值
 *  round() 四舍五入
 *  random() 随机数
 *
 *  练习： 随机生成一个100以内的数 提示用户猜数字
 *  提示猜大了 或者 猜小了 直到猜对 打印恭喜 并且提示猜了几次
 *
 */
public class TestMath {
    public static void main(String[] args) {
        System.out.println(Math.E);

        System.out.println(Math.PI);

        System.out.println(Math.abs(-789));

        System.out.println(Math.ceil(3.1));

        System.out.println(Math.floor(2.9));

        System.out.println(Math.round(5.5));

        System.out.println((int)(Math.random() * 100));

        System.out.println((int)(Math.random() * 17));

    }
}

```

### 5.Random类

> Random类是专门用于生成随机数据的类
>
> nextBoolean()  随机布尔数据
>
> nextDouble() 随机double数据
>
> nextFloat() 随机float数据
>
> nextInt() 随机int数据
>
> nextInt(int bound) 指定范围的int随机数
>
> nextLong() 随机long数据

```java
package com.atguigu.test4;

import java.util.Random;

/**
 *  Random类是专门用于生成随机数据的类
 *
 *  nextBoolean()  随机布尔数据
 *  nextDouble() 随机double数据
 *  nextFloat() 随机float数据
 *  nextInt() 随机int数据
 *  nextInt(int bound) 指定范围的int随机数
 *  nextLong() 随机long数据
 *
 */
public class TestRandom {
    public static void main(String[] args) {
        Random random = new Random();
        System.out.println(random.nextBoolean());
        System.out.println(random.nextDouble());
        System.out.println(random.nextFloat());
        System.out.println(random.nextInt(100));
        System.out.println(random.nextInt());
        System.out.println(random.nextLong());

    }
}

```

### 6.String类

> String类的一些基本方法
>
> length() 获取字符串长度
>
> equals() 比较字符串内容
>
> equalsIgnoreCase() 忽略大小写比较
>
> toLowerCase() 将字符串转换为小写
>
> toUpperCase() 将字符串转换为大写

```java
package com.atguigu.test5;

/**
 *  length() 获取字符串长度
 *  equals() 比较字符串内容
 *  equalsIgnoreCase() 忽略大小写比较
 *  toLowerCase() 将字符串转换为小写
 *  toUpperCase() 将字符串转换为大写
 */
public class TestString1 {
    public static void main(String[] args) {
        String str1 = "abc hello";

        System.out.println(str1.length());

        String str2 = "ABC hello";

        System.out.println(str1.equals(str2));

        System.out.println(str1.equalsIgnoreCase(str2));

        String s1 = str1.toUpperCase();

        System.out.println("s1 = " + s1);

        String s2 = str2.toLowerCase();

        System.out.println("s2 = " + s2);

    }
}
```

> concat() 拼接字符串
>
> indexOf(int num)    根据ASCII 码 或者 Unicode编码查找某个字符第一次出现的位置    返回下标 如未找到 返回-1
>
> indexOf(String str)  根据字符串查找某个字串第一次出现的位置     返回下标 如未找到 返回-1
>
> lastIndexOf(int num) 根据ASCII 码 或者 Unicode编码查找某个字符最后一次出现的位置    返回下标 如未找到 返回-1
>
> lastIndexOf(String str)根据字符串查找某个字串最后一次出现的位置     返回下标 如未找到 返回-1
>
> substring(int beginIndex) 从指定位置开始截取到字符串末尾
>
> substring(int beginIndex，int endIndex) 从指定位置开始截取到指定位置结束 包前不包后
>
> trim() 去除字符串首位的空格

```java
package com.atguigu.test5;

/**
 * concat() 拼接字符串
 * indexOf(int num)    根据ASCII 码 或者 Unicode编码查找某个字符第一次出现的位置    返回下标 如未找到 返回-1
 * indexOf(String str)  根据字符串查找某个字串第一次出现的位置     返回下标 如未找到 返回-1
 * lastIndexOf(int num) 根据ASCII 码 或者 Unicode编码查找某个字符最后一次出现的位置    返回下标 如未找到 返回-1
 * lastIndexOf(String str)根据字符串查找某个字串最后一次出现的位置     返回下标 如未找到 返回-1
 * substring(int beginIndex) 从指定位置开始截取到字符串末尾
 * substring(int beginIndex，int endIndex) 从指定位置开始截取到指定位置结束 包前不包后
 * trim() 去除字符串首位的空格
 */
public class TestString2 {
    public static void main(String[] args) {
        String str1 = "abc";
        String str2 = "hello";

        String concat = str1.concat(str2);
        System.out.println(concat);

        String str3 = "abc 中 hello world";

        System.out.println(str3.indexOf("a"));
        System.out.println(str3.indexOf("abc"));
        System.out.println(str3.indexOf("hello"));
        System.out.println(str3.indexOf("or"));
        System.out.println(str3.indexOf("xyz"));

        System.out.println(str3.indexOf(98));
        System.out.println(str3.indexOf(99));
        System.out.println(str3.indexOf(20013));


        String str4 = "abc 中 hello world abc 中 hello world";

        System.out.println(str4.lastIndexOf("a"));
        System.out.println(str4.lastIndexOf("abc"));
        System.out.println(str4.lastIndexOf("hello"));
        System.out.println(str4.lastIndexOf("or"));
        System.out.println(str4.lastIndexOf("xyz"));

        System.out.println(str4.lastIndexOf(98));
        System.out.println(str4.lastIndexOf(99));
        System.out.println(str4.lastIndexOf(20013));


        System.out.println("----------------------------------------------------");

        String str5 = "abc hello world 666";

        String substring = str5.substring(2);
        System.out.println("substring = " + substring);

        String substring1 = str5.substring(1, 8);// 包前不包后

        System.out.println("substring1 = " + substring1);

        System.out.println("----------------------------------------------------");

        String str6 = "                                 abc hello world   ";
        String trim = str6.trim();
        System.out.println("trim = " + trim);
        

    }
}

```

> *split(String condition)* *根据条件拆分字符串 返回值为字符串数组*

```java
package com.atguigu.test5;

import java.util.Scanner;

/**
 *  split(String condition) 根据条件拆分字符串 返回值为字符串数组
 */
public class TestString3 {
    public static void main(String[] args) {
        String str1 = "abc hello world 666";
        String[] s = str1.split(" ");
        for (int i = 0; i < s.length; i++) {
            System.out.println(s[i]);
        }

        System.out.println("-----------------------------------------------------------");

        Scanner input = new Scanner(System.in);

        System.out.println("请输入一段文字");

        String str = input.next();

        System.out.println("请输入要查找的文字");

        String findStr = input.next();

        String[] split = str.split("");

        for(int i = 0;i < split.length;i++){
            System.out.print(split[i] + "\t");
        }

        System.out.println();

        int count = 0;

        for(int i = 0;i < split.length;i++){
            if(findStr.equals(split[i])){
                count++;
            }
        }

        System.out.println("出现了" + count + "次");

    }
}

```

> charAt(int index) 根据传入的下标返回对应下标位置的字符
>
> contains(CharSequence s)  根据传入的字符串判断是否包含某一段字符串
>
> endsWith(String str)  判断字符串是否以某一个字符串结尾
>
> startsWith(String str)  判断字符串是否以某一个字符串开头
>
> replace(char oldChar,char newChar) 替换所有的指定字符为新字符
>
> replace(CharSequence oldChar,CharSequence newChar) 替换所有的指定字符串为新字符串

```java
package com.atguigu.test5;

/**
 * charAt(int index) 根据传入的下标返回对应下标位置的字符
 * contains(CharSequence s)  根据传入的字符串判断是否包含某一段字符串
 * endsWith(String str)  判断字符串是否以某一个字符串结尾
 * startsWith(String str)  判断字符串是否以某一个字符串开头
 * replace(char oldChar,char newChar) 替换所有的指定字符为新字符
 * replace(CharSequence oldChar,CharSequence newChar) 替换所有的指定字符串为新字符串
 */
public class TestString4 {
    public static void main(String[] args) {
        String str1 = "abc";
        char c1 = str1.charAt(0);
        char c2 = str1.charAt(1);
        char c3 = str1.charAt(2);

        System.out.println(str1.charAt(0));
        System.out.println(str1.charAt(1));
        System.out.println(str1.charAt(2));

        System.out.println("--------------------------------------");

        String str2 = "abc hello world";
        System.out.println(str2.contains("abc"));
        System.out.println(str2.contains("a"));
        System.out.println(str2.contains("helloworld"));

        System.out.println("--------------------------------------");

        String str3 = "abc hello world";

        System.out.println(str3.startsWith("abc"));

        System.out.println(str3.endsWith("d"));

        System.out.println("--------------------------------------");

        String str4 = "";
        System.out.println(str4.isEmpty());

        String str5 = null;
//        System.out.println(str5.isEmpty());

        System.out.println("--------------------------------------");

        String str6 = "abc hello abc world abc 566 abc" ;

        String replace1 = str6.replace('a', 'A');
        System.out.println("replace1 = " + replace1);

        String replace2 = str6.replace("abc", "一二三世界你好");
        System.out.println("replace2 = " + replace2);


    }
}

```

> *使用**replace()**方法将字符串中所有的空格去除*
>
> *replaceAll(String regex,String str)* *根据指定的正则表达式将匹配到的内容替换为指定内容*
>
> *toCharArray()*  *将字符串按照每一个字符转换为**char**数组*

```java
package com.atguigu.test5;

/**
 *  使用replace()方法将字符串中所有的空格去除
 *  replaceAll(String regex,String str) 根据指定的正则表达式将匹配到的内容替换为指定内容
 *  toCharArray()  将字符串按照每一个字符转换为char数组
 */
public class TestString5 {
    public static void main(String[] args) {
        String str1 = "   ab    c hell   o    w    o    l   r d      ";
        String replace = str1.replace(" ", "");
        System.out.println("replace = " + replace);

        System.out.println("----------------------------------------------------");

        String str2 = "   a\tb    c h\te\tl\tl   o \t   w  \t  o \t   l   r d  \t    ";
        String replace1 = str2.replace(" ", "");
        System.out.println("replace1 = " + replace1);


        System.out.println("----------------------------------------------------");
        String s = str2.replaceAll("\\s", "");
        System.out.println("s = " + s);
        System.out.println("----------------------------------------------------");


        String str3 = "1a2b3c4d5e6f";

        String s1 = str3.replaceAll("\\d", "S");
        System.out.println("s1 = " + s1);


    }
}

```

> String类是一个不可变的对象 任何对String对象的改变都会产生新的字符串对象
>
> 所以如果需要频繁的修改一个字符串的内容 不推荐使用String 推荐使用StringBuffer 或者 StringBuilder
> 
>
> 1.为什么String类是不可变对象
>
> 第一：因为String类是使用final修饰的 所以其他类无法继承此类
>
> 第二：String类底层是以char数组维护字符串的 此数组使用final修饰 表示无法指向新的地址
>
> 第三：维护String对象的char数组同时也是使用private修饰的 所以其他类无法访问此数组
> 
>
> 2.有没有什么当可以是字符串内容改变？
>
> 有 使用反射的方式可以获取到char数组 并且改变其内容
>
> 
>
> 为什么比较字符串一定要用equals  而不能使用== 呢？

```java
package com.atguigu.test6;

/**
 *  String类是一个不可变的对象 任何对String对象的改变都会产生新的字符串对象
 *  所以如果需要频繁的修改一个字符串的内容 不推荐使用String 推荐使用StringBuffer 或者 StringBuilder
 *
 *  1.为什么String类是不可变对象
 *      第一：因为String类是使用final修饰的 所以其他类无法继承此类
 *      第二：String类底层是以char数组维护字符串的 此数组使用final修饰 表示无法指向新的地址
 *      第三：维护String对象的char数组同时也是使用private修饰的 所以其他类无法访问此数组
 *
 *  2.有没有什么当可以是字符串内容改变？
 *      有 使用反射的方式可以获取到char数组 并且改变其内容
 *
 *
 *  为什么比较字符串一定要用equals  而不能使用== 呢？
 *
 */
public class TestString1 {
    public static void main(String[] args) {

        System.out.println("hello");

        String str1 = "abc";
        String str2 = "abc";
        String str3 = new String("abc");
        String str4 = new String("abc");

        System.out.println(str1 == str2); // true

        System.out.println(str3 == str4); // false

        System.out.println(str1 == str3); // false

        System.out.println("----------------------------------------------------------------");

        String condition = "电脑";
        condition += "笔记本";
        condition += "外星人";
        condition += "i9 13代";
        condition += "4090 TI";
        condition += "64 GB";
        condition += "1T SSD";
        condition += "244HZ";


        System.out.println(condition);




    }
}

```

> StringBuffer 和 StringBuilder的区别？
>
> StringBuffer线程安全    JDK1.0
>
> StringBuilder线程不安全  JDK1.5
> 
>
> String StringBuffer StringBuilder 三者的区别？
>
> String是不可变对象
>
> 后两者是可变对象
>
> StringBuffer线程安全    JDK1.0
>
> StringBuilder线程不安全  JDK1.5

```java
package com.atguigu.test6;

/**
 *  StringBuffer 和 StringBuilder的区别？
 *  StringBuffer线程安全    JDK1.0
 *  StringBuilder线程不安全  JDK1.5
 *
 *  String StringBuffer StringBuilder 三者的区别？
 *  String是不可变对象
 *  后两者是可变对象
 *  StringBuffer线程安全    JDK1.0
 *  StringBuilder线程不安全  JDK1.5
 *
 */
public class TestStringBufferStringBuilder {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder();
        sb.append(true);
        sb.append("abc");
        sb.append(123);
        sb.append(3.5);
        sb.append(3.5F);
        System.out.println("sb = " + sb);


        sb.delete(0, 3);
        System.out.println("sb = " + sb);

        sb.insert(6, "世界你好");

        System.out.println("sb = " + sb);

        sb.replace(0, 5, "hello world ");

        System.out.println("sb = " + sb);

        sb.reverse();

        System.out.println("sb = " + sb);


    }
}

```

