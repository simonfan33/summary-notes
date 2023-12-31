##  循环结构

### 1. 打印三角形

> 使用多重循环打印三角形
>
> 左半部分为到三角形
>
> 右半部分为一个直角三角形  每一行元素的个数为  行号 * 2  - 1

```java
package com.atguigu.test1;

/**
 *  使用多重循环打印三角形
 *  左半部分为到三角形
 *  右半部分为一个直角三角形  每一行元素的个数为  行号 * 2  - 1
 */
public class TestTriangle {
    public static void main(String[] args) {
        for(int i = 1;i <= 5;i++){
            // 左半部分
            for(int j = 5;j >= i;j--){
                System.out.print("$");
            }
            // 右半部分
            for(int j =1;j <= i * 2 -1   ;j++){
                System.out.print("*");
            }
            // 换行
            System.out.println();
        }
    }
}

```

### 2. 课堂练习

#### 2.1 银行菜单

> 打印银行菜单。（要求使用：switch、do while）

```java
package com.atguigu.test1;

import java.util.Scanner;

/**
 *  打印银行菜单。（要求使用：switch、do while）
 */
public class TestBankMenu {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        int choice;
        do {
            System.out.println("********************欢迎使用钱真多银行菜单系********************");

            System.out.println("1.开户   2.存款   3.取款   4.转账   5.修改密码  6.查询余额  0.退出");

            choice = input.nextInt();

            switch(choice){
                case 1:
                    System.out.println("执行开户功能");
                    break;
                case 2:
                    System.out.println("执行存款功能");
                    break;
                case 3:
                    System.out.println("执行取款功能");
                    break;
                case 4:
                    System.out.println("执行转账功能");
                    break;
                case 5:
                    System.out.println("执行修改密码功能");
                    break;
                case 6:
                    System.out.println("执行查询余额功能");
                    break;
                case 0:
                    System.out.println("谢谢使用，再见~");
                    break;
                default:
                    System.out.println("输入有误，请重新输入");
                    break;
            }
        } while (choice != 0);


        System.out.println("程序结束");


    }
}

```

#### 2.2 打印数列

> 使用循环输出 100、95、90、85.......5

```java
package com.atguigu.test1;

/**
 *  使用循环输出 100、95、90、85.......5
 */
public class TestPrintNumber {
    public static void main(String[] args) {
        for(int i = 100;i > 0;i-=5){
            System.out.println(i);
        }

        System.out.println("-----------------------------");

        for(int i = 100;i > 0;i--){
            if(i % 5 == 0){
                System.out.println("i = " + i);
            }
        }

    }
}

```

### 3. break关键字补充

> break关键字在多重循环中的使用
>
>
> break默认情况：只会对离break关键字最近的结构产生作用
>
> 也可以在指定的结构之前 加上标记 然后再break关键字之后指定对应的标记 跳出指定的结构

```java
package com.atguigu.test1;

/**
 *  break关键字在多重循环中的使用
 *
 *  break默认情况：只会对离break关键字最近的结构产生作用
 *  也可以在指定的结构之前 加上标记 然后再break关键字之后指定对应的标记 跳出指定的结构
 *
 *
 */
public class TestBreak {
    public static void main(String[] args) {
        for(int i = 1;i <= 5;i++){
            for(int j = 1;j <= 6;j++){
                if(j == 3){
                    break;
                }
                System.out.print(j + "\t");
            }
            System.out.println();
        }

        System.out.println("------------------------------------------------");
        sb : for(int i = 1;i <= 5;i++){
            for(int j = 1;j <= 6;j++){
                if(j == 3){
                    break sb;
                }
                System.out.print(j + "\t");
            }
            System.out.println();
        }

        System.out.println("------------------------------------------------");

    }
}

```

## 方法

### 1. 方法的概念

> 一系列代码指令的集合，用于解决特定的问题，可以反复使用。

### 2. 方法的格式

```java
public static void 方法名(){
    
}
```

### 3.方法的定义位置

> 与main方法平级

### 4. 方法的调用

> 在需要调用方法的位置直接书写方法名即可。

```java
package com.atguigu.test3;

/**
 *  使用方法打印符号
 */
public class TestPrintSign1 {

    public static void main(String[] args) {
        System.out.println("床前明月光");
        printSign();
        System.out.println("疑是地上霜");
        printSign();
        System.out.println("举头望明月");
        printSign();
        System.out.println("低头思故乡");
        printSign();
    }

    public static void printSign(){
       for(int i = 1;i <= 10;i++){
           System.out.print("-");
       }
       System.out.println();
    }
}
```

### 5.方法的参数

> 参数：调用方法时传递的数据称之为参数
>
>
>
> 形参：形式参数 方法定义的时候书写的参数 称之为形参
>
> 形参规定了参数的**`个数、类型、顺序`** 表示实参必须遵循这个规定
>
>
> 实参：方法调用的时候传入的参数 必须符合形参的约定

#### 5.1 单个参数

> 使用参数实现 可以让用户灵活的指定横线的个数

```java
package com.atguigu.test4;

/**
 *  使用参数实现 可以让用户灵活的指定横线的个数
 */
public class TestPrintSign {
    public static void main(String[] args) {
        System.out.println("床前明月光");
        int a = 10;
        printSign(a);

        System.out.println("疑是地上霜");
        printSign(5);

        System.out.println("举头望明月");
        int numa = 3;
        int numb = 4;
        int sum = numa + numb;
        printSign(sum);

        System.out.println("低头思故乡");
        int count = 3;
        printSign(count);
    }
    /**
     *  参数：调用方法时传递的数据称之为参数
     *  形参：形式参数 方法定义的时候书写的参数 称之为形参
     *  形参规定了参数的个数、类型、顺序 表示实参必须遵循这个规定
     *
     *  实参：方法调用的时候传入的参数 必须符合形参的约定
     * @param count
     */
    public static void printSign(int count){
        for(int i = 1;i <= count;i++){
            System.out.print("-");
        }
        System.out.println();
    }

}
```

#### 5.2 多个参数

> 使用多个参数实现 让用户灵活的指定 符号的 个数 以及符号的类型

```java
package com.atguigu.test5;

/**
 *  使用多个参数实现 让用户灵活的指定 符号的 个数 以及符号的类型
 */
public class TestPrintSign {
    public static void main(String[] args) {
        System.out.println("床前明月光");
        printSign(5, "#");


        System.out.println("疑是地上霜");
        printSign(10,"%");

        System.out.println("举头望明月");
        int count = 10;
        String sign = "&";
        printSign(count, sign);

        System.out.println("低头思故乡");
        printSign(6, "*");
    }

    public static void printSign(int count,String sign){
        for(int i = 1;i <= count;i++){
            System.out.print(sign);
        }
        System.out.println();
    }

}
```

