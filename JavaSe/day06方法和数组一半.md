## 方法

### 1. 返回值和返回值类型

> 如果方法声明返回值类型 不是void  那么必须在方法体中使用return返回具体的值 要求值必须和声明类型匹配
>
> return关键字：表示结束方法并且返回内容  所以在return关键字之后不能书写任何代码

#### 1.1 情况1： 一个return返回结果

> 有返回值的方法三种调用方式
>
> 没有返回值的方法 只能单独调用  不能接收 也不能打印

```java
package com.atguigu.test3;

/**
 *  编写方法实现两个数求和计算
 *
 *  有返回值的方法三种调用方式
 *  没有返回值的方法 只能单独调用  不能接收 也不能打印
 *
 *  return关键字：表示结束方法并且返回内容  所以在return关键字之后不能书写任何代码
 */
public class TestAdd {
    public static int add(int a,int b){
        int sum = a + b;
        return sum;
    }

    public static void m1(){
        System.out.println("m1方法执行了");
    }


    public static void main(String[] args) {
        System.out.println("main方法开始执行");

        add(10, 20); // 直接调用方法

        int sum = add(5, 6); // 先接收返回值
        System.out.println("sum = " + sum); // 将返回值打印


        System.out.println(add(2, 3)); // 直接打印方法 先执行add方法 再将方的返回值进行打印

        System.out.println("--------------------------------");

        m1();

//        System.out.println(m1());;



        System.out.println("main执行完毕");


    }
}

```



#### 1.2 情况2：分支结构 多个return

> 在分支结构中返回结果 必须保证每一条分支都有正确的返回值 即可以书写多个return

```java
package com.atguigu.test3;

/**
 *  在分支结构中返回结果 必须保证每一条分支都有正确的返回值 即可以书写多个return
 *  返回值只认类型 务必要将返回的数据 和 方法声明所定义的返回值类型保持一致
 */
public class TestReturn {
    public static String isEven1(int num){
        if(num % 2 == 0){
            return "偶数";
        }else{
            return "奇数";
        }
    }

    public static String isEven3(int num){
        if(num % 2 == 0){
            return "偶数";
        }
        return "奇数";
    }



    public static String isEven2(int num){
        return num % 2 == 0 ? "偶数" : "奇数";
    }


    public static boolean isEven4(int num){
        if(num % 2 == 0 ){
            return true;
        }
        return false;
    }

    public static int isEven5(int num){
        return num % 2;
    }


    public static void main(String[] args) {
        System.out.println(isEven4(4) == true ? "偶数" : "奇数");
        System.out.println(isEven4(4) ? "偶数" : "奇数");
    }




}

```



#### 1.3 情况3：返回值为void 也可以使用return

> 在方法声明返回值为void的情况 也可以使用return  此时return只表示结束方法

```java
package com.atguigu.test3;

/**
 *  如果方法声明返回值类型 不是void  那么必须在方法体中使用return返回具体的值
 *  在方法声明返回值为void的情况 也可以使用return  此时return只表示结束方法
 */
public class TestVoidReturn {
    public static void show(){
        for(int i = 1;i <= 20;i++){
            if(i == 10){
                return; // 结束方法 即 方法将停止执行  注意和break的区别
            }
            System.out.println(i);
        }

        System.out.println("show方法执行完毕");
    }

    public static void main(String[] args) {
        show();
    }

}

```

### 2. 方法重载

> 方法重载：同一个类或者父子类中的方法 名称相同 参数列表(个数、类型、顺序)不同 即可称之为方法重载
>
> 跟返回值类型 访问权限修饰符无关
>
> 在我们调用重载的方法的时候 会根据我们传入的实参匹配到唯一一个与之对应的方法 所以 无需担心
>
> 方法名称都一张 不知道调用哪个方法 这个问题

```java
package com.atguigu.test4;

/**
 *  方法重载：同一个类或者父子类中的方法 名称相同 参数列表(个数、类型、顺序)不同 即可称之为方法重载
 *  跟返回值类型 访问权限修饰符无关
 *
 *  在我们调用重载的方法的时候 会根据我们传入的实参匹配到唯一一个与之对应的方法 所以 无需担心
 *  方法名称都一张 不知道调用哪个方法 这个问题
 */
public class TestMethodOverLoad {
    public static int add(int a,int b){
        System.out.println(a + b);
        return a + b;
    }

    public static double add(String str,int a){
        System.out.println(str + a);
        return 10;
    }

    public static String add(int a,String str){
        System.out.println(str + a);
        return "hello";
    }

    public static void add(int a,int b,int c){
        System.out.println(a + b + c);
    }

    public static void add(int a,int b,int c,int d){
        System.out.println(a + b + c + d);
    }

    public static void add(int a,int b,int c,int d,int e){
        System.out.println(a + b + c + d + e);
    }

    public static void main(String[] args) {
        add(20,30);
        add(20,30,40);
        add(20,30,40,50);
        add(20,30,40,50,60);
    }
}
```



## 数组

### 1. 数组的概念

> 一组**`连续`**的存储空间，存储多个**`相同`**数据类型的值，长度是**`固定`**的。

### 2. 数组的定义

> 先声明、再分配空间：
> 数据类型[] 数组名;
>
> 数组名 = new 数据类型[长度];
> 声明并分配空间：数据类型[] 数组名 = new 数据类型[长度];
>
> 声明并赋值（繁）：
> 数据类型[] 数组名 = new 数据类型[]{value1,value2,value3,...};
>
> 声明并赋值（简）：
> 数据类型[] 数组名 = {value1,value2,value3,...}; 

```java
package com.atguigu.test5;

/**
 *  数组定义三种方式
 */
public class TestArrayDefine {
    public static void main(String[] args) {
        // 方式1 先声明 再分配空间
        int [] arr1;
        arr1 = new int[3];

        // 方式2 声明并且分配空间
        int [] arr2 = new int[4];


        // 方式3 声明并且赋值 繁琐
        int [] arr3 = new int[]{1,22,55,89,56,45,78};


        // 方式4 声明并且赋值 简单
        int [] arr4 = {45,78,12,56,23,89};
    }
}
```

### 3. 数组的使用

> 数组的使用
>
> 元素：数组中的每个数据 称之为元素
>
> 下标、角标、索引、index ： 下标表示数组中元素的编号 自动生成 从0开始 往后依次+1
>
> 数组的访问：获取数组中元素的值 以及 给元素赋值 这些操作 都属于数组的访问
>
> 赋值 数组名[下标] = 值；
>
> 取值 System.out.println(数组名[下标]);
>
> 数组下标越界异常：访问不存在的下标 将会数组下标越界异常  ArrayIndexOutOfBoundsException
>
> 异常会导致程序中断

```java
package com.atguigu.test6;

/**
 *  数组的使用
 *      元素：数组中的每个数据 称之为元素
 *      下标、角标、索引、index ： 下标表示数组中元素的编号 自动生成 从0开始 往后依次+1
 *      数组的访问：获取数组中元素的值 以及 给元素赋值 这些操作 都属于数组的访问
 *          赋值 数组名[下标] = 值；
 *          取值 System.out.println(数组名[下标]);
 *
 *  数组下标越界异常：访问不存在的下标 将会数组下标越界异常  ArrayIndexOutOfBoundsException
 *  异常会导致程序中断
 */
public class TestArrayUse {
    public static void main(String[] args) {
        int [] arr1 = new int[3];
        // 赋值
        arr1[0] = 12;
        arr1[1] = 78;
        arr1[2] = 88;
//        arr1[5566] = 99;

        // 取值
        System.out.println("数组中的第一个元素值为：" + arr1[0]);
        System.out.println("数组中的第二个元素值为：" + arr1[1]);
        System.out.println("数组中的第三个元素值为：" + arr1[2]);
//        System.out.println("数组中的第四个元素值为：" + arr1[3]);



    }
}

```



### 4. 数组的遍历

> 遍历：逐一对数组中的元素进行访问  这个过程称之为遍历 使用 循环来遍历数组

```java
package com.atguigu.test6;

import java.util.Scanner;

/**
 *  遍历：逐一对数组中的元素进行访问  这个过程称之为遍历 使用 循环来遍历数组
 */
public class TestArrayForeach {
    public static void main(String[] args) {
        int [] arr1 = new int[6];

        Scanner input = new Scanner(System.in);

        // 赋值 使用循环遍历赋值
        for(int i = 0;i < 6;i++){
            System.out.println("请输入第" + (i + 1) + "个元素的值");
            arr1[i] =  input.nextInt();
        }

        System.out.println("-----------------------------------------------------");

        // 取值 使用循环遍历取值
        for(int i = 0;i < 6;i++){
            System.out.print(arr1[i] + "\t");
        }
        System.out.println();






    }
}

```



### 5. 数组的属性

> 数组的属性：length 是一个int类型的数据 表示数组的长度
>
> 使用方式 ： 数组名.length

```java
package com.atguigu.test6;

/**
 *  数组的属性：length 是一个int类型的数据 表示数组的长度
 *  使用方式 ： 数组名.length
 */
public class TestArrayField {
    public static void main(String[] args) {
        int [] arr1 = {564,12,5,63};

        System.out.println(arr1.length);

        for(int i = 0;i < arr1.length;i++){
            System.out.print(arr1[i] + "\t");
        }
        System.out.println();

    }
}

```

### 6.数组的默认值

> 数组的默认值
>
> 数组在开辟完空间以后 是有默认值的
>
> 整数  0
>
> 浮点  0.0
>
> 字符  \u0000
>
> 布尔  false
>
> 其他  null
>
> Arrays工具类 位于java.util包 此类提供了有专门用于操作数组的各种方法
>
> Arrays.toString(数组名) ： 将数组中的元素转换为字符串

```java
package com.atguigu.test7;

import java.util.Arrays;

/**
 *  数组的默认值
 *  数组在开辟完空间以后 是有默认值的
 *  整数  0
 *  浮点  0.0
 *  字符  \u0000
 *  布尔  false
 *
 *  其他  null
 *
 *  Arrays工具类 位于java.util包 此类提供了有专门用于操作数组的各种方法
 *  Arrays.toString(数组名) ： 将数组中的元素转换为字符串
 */
public class TestArrayDefaultValue {
    public static void main(String [] args   ) {
        System.out.println("main方法开始执行");

        byte [] arr1 = new byte[3];

        String str = Arrays.toString(arr1);

        System.out.println("str = " + str);




        short []  s1  = new short[5];

        System.out.println(Arrays.toString(s1));

        int [] i1 = new int[7];

        System.out.println(Arrays.toString(i1));

        long [] l1 = new long[3];

        System.out.println(Arrays.toString(l1));

        float [] f1 = new float[2];

        System.out.println(Arrays.toString(f1));

        double [] d1 = new double[3];
        System.out.println(Arrays.toString(d1));

        boolean [] bl1 = new boolean [5];

        System.out.println(Arrays.toString(bl1));

        char [] ch1 = new char[4];
        System.out.println(Arrays.toString(ch1));


        String [] strings = new String[4];

        System.out.println(Arrays.toString(strings));

        System.out.println("程序结束");
    }
}

```

### 7. 数组的扩容



![](C:/Users/89585/Desktop/%E5%B0%9A%E7%A1%85%E8%B0%B7Java%E5%9F%BA%E7%A1%80%E8%AF%BE%E4%BB%B6/%E7%AC%94%E8%AE%B0/day06/day06-%E7%AC%94%E8%AE%B0/%E6%95%B0%E7%BB%84%E7%9A%84%E6%89%A9%E5%AE%B9.png)

> 数组的扩容 ：数组的长度是固定的 一旦定义以后 是无法改变
>
> 扩容的步骤：
>
> 1.创建一个更长的数组
>
> 2.将原数组中的内元素 依次复制到新数组中
>
> 3.将原数组引用(变量)指向新的数组
>
> 数组作为引用数据类型 其数组名(引用)中保存的是数组在内存中的地址
> 所以 将一个数组 赋值 给另外一个数组 赋值的是地址

```java
package com.atguigu.test7;

import java.util.Arrays;

/**
 *  数组的扩容 ：数组的长度是固定的 一旦定义以后 是无法改变
 *  扩容的步骤：
 *  1.创建一个更长的数组
 *  2.将原数组中的内元素 依次复制到新数组中
 *  3.将原数组引用(变量)指向新的数组
 */
public class TestArrayGrow {
    public static void main(String[] args) {
        int [] oldArr = {1,2,3,4,5};

        int [] newArr = new int[oldArr.length * 2];

        for(int i = 0;i < oldArr.length;i++){
            newArr[i] = oldArr[i];
        }
        System.out.println(Arrays.toString(newArr));

        // 数组作为引用数据类型 其数组名(引用)中保存的是数组在内存中的地址
        // 所以 将一个数组 赋值 给另外一个数组 赋值的是地址

        System.out.println("地址赋值之前");
        System.out.println("oldArr地址值：" + oldArr);
        System.out.println("newArr地址值：" +  newArr);
        System.out.println("oldArr长度：" + oldArr.length);
        System.out.println("newArr长度：" +  newArr.length);


        oldArr = newArr;

        System.out.println("地址赋值之后");
        System.out.println("oldArr地址值：" + oldArr);
        System.out.println("newArr地址值：" +  newArr);
        System.out.println("oldArr长度：" + oldArr.length);
        System.out.println("newArr长度：" +  newArr.length);

    }
}


```

