## 数组

### 1. 复制数组的方式

> 方式1 普通for循环复制
>
> 方式2 System.arraycopy(原数组，起始位置，新数组，起始位置，元素个数)
>
> 方法3 Arrays.copyOf(原数组，新长度)

```java
package com.atguigu.test1;

import java.util.Arrays;

/**
 * @author WHD
 * @description TODO
 * @date 2023/5/31 9:05
 *  复制数组的三种方式
 *  方式1 普通for循环复制
 *  方式2 System.arraycopy(原数组，起始位置，新数组，起始位置，元素个数)
 *  方法3 Arrays.copyOf(原数组，新长度)
 */
public class TestArrayCopy {
    public static void main(String[] args) {
        // 方式1 普通for循环复制
        int [] arr1 = {1,2,3,4,5};
        int [] arr2 = new int[arr1.length];

        for(int i =0;i < arr1.length;i++){
            arr2[i] = arr1[i]; // 等号右边赋值给等号左边
        }

        System.out.println(Arrays.toString(arr2));

        System.out.println("---------------------------------------------");

        // 方式2 System.arraycopy(原数组，起始位置，新数组，起始位置，元素个数)
        // 最后一个参数 必须大于等于0 并且 小于等于可被粘贴的元素个数 否则将报异常 下标越界异常
        int [] arr3 = {11,22,33,44,55};
        int [] arr4 = new int[arr3.length * 2];

        System.arraycopy(arr3, 1, arr4, 2, 2);

        System.out.println(Arrays.toString(arr4));

        System.out.println("---------------------------------------------");

        int [] arr5 = {1,2,3,4,5};

        int [] arr6 = Arrays.copyOf(arr5,10);

        System.out.println(Arrays.toString(arr6));

    }
}

```

### 2. 课堂练习

> 统计int类型数组中所有元素的总和。
>
>
> 统计int类型数组中所有元素的平均值。

```java
package com.atguigu.test1;

import java.util.Scanner;

/**
 * @author WHD
 * @description TODO
 * @date 2023/5/31 9:20
 *  统计int类型数组中所有元素的总和。
 *
 *  统计int类型数组中所有元素的平均值。
 */
public class TestArraySum {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);

        int [] arr1 = new int[5];
        int sum = 0;
        for(int i = 0;i < arr1.length;i++){
            System.out.println("请输入第" + (i + 1) + "个元素的值");
            arr1[i] = input.nextInt();
            sum += arr1[i];
        }


        System.out.println("arr1数组中元素总和为：" + sum);
        System.out.println("arr1数组中元素平均值为：" + sum / arr1.length);






    }
}

```

> 求一个数组中的最大值，或者最小值。
>
>
> 分析：求最大值思路 先假设一个最大值 依次与其他元素比较 遇到更到的  交换头衔

```java
package com.atguigu.test1;

/**
 * @author WHD
 * @description TODO
 * @date 2023/5/31 9:23
 *  求一个数组中的最大值，或者最小值。
 *
 *  分析：求最大值思路 先假设一个最大值 依次与其他元素比较 遇到更到的  交换头衔
 *
 */
public class TestArrayMaxMinValue {
    public static void main(String[] args) {
        int [] arr1 = {1,5,6,7845,56,123,456,888,999,124};

        int max = arr1[0]; // 假设第一个元素为最大元素
        int min = arr1[0]; // 假设第一个元素为最小值

        for(int i = 1;i < arr1.length;i++){
            if(max < arr1[i]){
                max = arr1[i];
            }

            if(min > arr1[i]){
                min = arr1[i];
            }


        }


        System.out.println("最大的元素为：" + max);
        System.out.println("最小的元素为：" + min);


    }
}

```

### 3. 数组类型的参数和返回值

> 数组类型的参数和返回值
>
> 数组类型的参数  和  返回值 与之前我们学习的其他类型的参数 返回值 使用方式完全相同*
>
> 需求：
>
> 1.编写一个方法 用于统计一个学生5门课的成绩 最后将成绩返回
>
> 2.编写一个方法 用于接收一个学生5门课的成绩 并且打印

```java
package com.atguigu.test1;

import java.util.Arrays;
import java.util.Scanner;

/**
 * @author WHD
 * @description TODO
 * @date 2023/5/31 9:31
 *  数组类型的参数和返回值
 *  数组类型的参数  和  返回值 与之前我们学习的其他类型的参数 返回值 使用方式完全相同
 *
 *  需求：
 *  1.编写一个方法 用于统计一个学生5门课的成绩 最后将成绩返回
 *  2.编写一个方法 用于接收一个学生5门课的成绩 并且打印
 */
public class TestArrayParameterReturned {

    public static double[] getInputScore(){
        double [] score = new double[5];
        Scanner input = new Scanner(System.in);
        for(int i = 0;i < score.length;i++){
            System.out.println("请输入第" + (i + 1) + "门成绩");
            score[i] = input.nextDouble();
        }
        return score;
    }

    public static void printScore(double [] score){
        for(int i = 0;i < score.length;i++){
            System.out.println("第" + (i + 1) + "门成绩为：" + score[i]);
        }
    }

    public static void main(String[] args) {
        double[] score = getInputScore();

      //  System.out.println(Arrays.toString(score));

        printScore(score);
    }
}

```



### 4. 面试题：值传递和引用传递的区别？

> 面试题：值传递和引用传递
>
> 基本数据类型传参属于值传递 (传递是值本身 值的副本 值的拷贝)在方法中对参数的改变 不会影响原变量
>
> 引用数据类型传参属于引用(地址)传递 传递的是地址   在方法对参数的改变 会影响原变量
>
> String类型是特殊的引用数据类型 作为参数传递 不会影响原变量
>
>
> Java官方明确表示 Java中只有值传递 我们传递的引用数据类型 也属于值传递 只不过这个值 是一个地址

```java
package com.atguigu.test2;

import java.util.Arrays;

/**
 * @author WHD
 * @description TODO
 * @date 2023/5/31 10:33
 *  面试题：值传递和引用传递
 *  基本数据类型传参属于值传递 (传递是值本身 值的副本 值的拷贝)在方法中对参数的改变 不会影响原变量
 *  引用数据类型传参属于引用(地址)传递 传递的是地址   在方法对参数的改变 会影响原变量
 *  String类型是特殊的引用数据类型 作为参数传递 不会影响原变量
 *
 *  Java官方明确表示 Java中只有值传递 我们传递的引用数据类型 也属于值传递 只不过这个值 是一个地址
 *
 *
 *
 */
public class TestArrayTransfer {
    public static void m3(String str){
        str += "abc";
    }
    public static void m1(int num){
        num++;
    }
    public static void m2(int [] nums){
        for(int i = 0;i < nums.length;i++){
            nums[i]++;
        }
    }
    public static void main(String[] args) {
        int a = 10;
        m1(a);
        System.out.println(a);

        System.out.println("--------------------------------------------");

        int [] arr = {1,2,3,4};
        m2(arr);
        System.out.println(Arrays.toString(arr));

        System.out.println("--------------------------------------------");

        String b = "hello";
        m3(b);
        System.out.println("b = " + b);

    }
}
```

### 5. 可变长参数

> 可变长参数
>
> 概念：可接收多个同类型实参，个数不限，使用方式与数组相同。
>
> 语法：数据类型... 形参名 
>
> 要求：必须定义在形参列表的最后，且只能有一个。

```java
package com.atguigu.test2;

/**
 * @author WHD
 * @description TODO
 * @date 2023/5/31 10:51
 *  可变长参数
 *  概念：可接收多个同类型实参，个数不限，使用方式与数组相同。
 *  语法：数据类型... 形参名 //必须定义在形参列表的最后，且只能有一个。
 */
public class TestArrayChangeParameter {
    public static void printArray(int nums , int ... args){
        System.out.println("printArray方法开始执行");

        for(int i = 0;i < args.length;i++){
            System.out.print(args[i] + "\t");
        }
        System.out.println();

        System.out.println("printArray方法执行完毕");
    }

    public static void main(String... args) {
        printArray(1,2,3,4,5,89,55,66,99,77,44);
    }




}

```

### 6. 排序算法

#### 6.1 冒泡排序

![](img/冒泡排序1.gif)

> 冒泡排序
>
> 比较的轮数：轮数是长度-1  n - 1
>
> 每一轮比较的次数：最多的一次为长度-1次 后续每一轮递减1
>
>
> 外层循环控制轮数
>
> 内层循环控制每一轮比较的次数
>
>
>
> 笔试题：手写冒泡排序  外层循环 n -1  内层循环 n - 1 - i

```java
package com.atguigu.test3;

import java.util.Arrays;

/**
 * @author WHD
 * @description TODO
 * @date 2023/5/31 11:34
 *  冒泡排序
 *  比较的轮数：轮数是长度-1  n - 1
 *  每一轮比较的次数：最多的一次为长度-1次 后续每一轮递减1
 *
 *   外层循环控制轮数
 *   内层循环控制每一轮比较的次数
 *
 *
 *   笔试题：手写冒泡排序  外层循环 n -1  内层循环 n - 1 - i
 *
 */
public class TestBubble {
    public static void main(String[] args) {
//        int a = 10;
//        int b = 20;
//
//        int temp = a;
//        a = b;
//        b = temp;

        int [] nums = {784,56,125,555,666};

        for(int i = 0;i < nums.length - 1;i++){
            for(int j = 0;j < nums.length - 1 - i;j++){
                if(nums[j] < nums[j + 1]){
                    int temp = nums[j];
                    nums[j] = nums[j + 1];
                    nums[j + 1] = temp;
                }
            }
            System.out.println("第"+( i + 1)+"轮比较完成以后，元素顺序为：" + Arrays.toString(nums));
        }
        System.out.println(Arrays.toString(nums));
    }
}

```

#### 6.2 选择排序

![](img/选择排序2.gif)

> 选择排序：固定位置的元素依次与其他位置的元素 比较大小 遇到需要交换位置的元素
>
> 暂时不交换  使用应该被交换位置的元素 继续 往后比较  等待一轮比较完成以后只交换一次位置
>
>
> 外层循环属于比较的元素A    从第一个元素开始 直到倒数第二个元素  即  i = 0 ; i < n - 1;
>
> 内层循环属于比较的元素B    从第二个元素开始 直到最后一个元素  即 j = i + 1; j < n;
>
> 在比较的过程中 有一个交换比较元素的操作 这个操作我们可以通过控制下标来实现

```java
package com.atguigu.test4;

/**
 * @author WHD
 * @description TODO
 * @date 2023/5/31 14:55
 *  选择排序：固定位置的元素依次与其他位置的元素 比较大小 遇到需要交换位置的元素
 *  暂时不交换  使用应该被交换位置的元素 继续 往后比较  等待一轮比较完成以后只交换一次位置
 *
 *  外层循环属于比较的元素A    从第一个元素开始 直到倒数第二个元素  即  i = 0 ; i < n - 1;
 *  内层循环属于比较的元素B    从第二个元素开始 直到最后一个元素  即 j = i + 1; j < n;
 *  在比较的过程中 有一个交换比较元素的操作 这个操作我们可以通过控制下标来实现
 *
 */
public class TestChoiceSort {
    public static void main(String[] args) {
        int [] nums = {-99,-888,784,56,125,555,666};

        for(int i = 0;i < nums.length - 1;i++){
            int index = i ; // 将每一次循环开始以后 将i的下标做记录
            for(int j = i + 1;j < nums.length;j++){
                if(nums[index] > nums[j]){
                    index = j;
                }
            }
            // 这里交换元素
            // 交换元素的依据是什么呢？ 当index 和 i 不相等的情况下
            if(index != i){
                int temp = nums[index];
                nums[index] = nums[i];
                nums[i] = temp;
            }

        }
    }
}

```



#### 6.3 JDK提供的快速排序

> Arrays.sort(数组) 将数组按照升序排序

```java
package com.atguigu.test5;

import java.util.Arrays;

/**
 * @author WHD
 * @description TODO
 * @date 2023/5/31 16:00
 *  JDK提供的排序方法
 *  Arrays.sort(数组) 将数组按照升序排序

 */
public class TestArrays {
    public static void main(String[] args) {
        int [] nums = {78,412,89,63,89,412,0,74,5};

        Arrays.sort(nums);

        System.out.println(Arrays.toString(nums));

    }
}
```

### 7.二分查找

> 二分查找法： 根据指定的元素 查找此元素在数组中的下标
>
> 前提要求为 使用二分查找 数组必须先完成排序
>
>
> 二分查找比较原理： 将这个数列 一分为二 如果查找的元素 小于中间值 那么从左边查找
>
> 如果大于中间值 从右边查找 如果等于中间值 直接将中间值的下标返回即可 完成
>
> 如果没有找到 返回-1

```java
package com.atguigu.test5;

/**
 * @author WHD
 * @description TODO
 * @date 2023/5/31 15:41
 *  二分查找法： 根据指定的元素 查找此元素在数组中的下标
 *  前提要求为 使用二分查找 数组必须先完成排序
 *
 *  二分查找比较原理： 将这个数列 一分为二 如果查找的元素 小于中间值 那么从左边查找
 *  如果大于中间值 从右边查找 如果等于中间值 直接将中间值的下标返回即可 完成
 *  如果没有找到 返回-1
 *
 */
public class TestBinarySearch {
    public static int binarySearch(int [] array,int num){
        // 以下四个变量 都是下标
        int index = -1 ; // 用于作为最终的返回值
        int begin = 0; // 起始位置
        int end = array.length -1; // 结束位置
        int middle = (begin + end) / 2;  // 中间位置

        while(begin <= end){
            if(num < array[middle]){
                end = middle -1;
            }else if(num > array[middle]){
                begin = middle + 1;
            }else{
                index = middle;
                break;
            }

            middle = (begin + end) / 2;
        }
        return index;
    }

    public static void main(String[] args) {
        int [] arr = {1,2,3,4,5,6,7,8,9};

        int i = binarySearch(arr, 6);

        System.out.println("i = " + i);


    }


}

```



### 8. Arrays工具类

> JDK提供的排序方法
>
> Arrays.sort(数组) 将数组按照升序排序
>
> Arrays.toString(数组) 将数组转换为字符串
>
> Arrays.binarySearch(数组，查找的元素) 使用二分查找返回对应元素的下标
>
> Arrays.fill(数组，元素) 指定元素填充数组
>
> Arrays.copyOf(数组，新长度) 复制数组

```java
package com.atguigu.test5;

import java.util.Arrays;

/**
 * @author WHD
 * @description TODO
 * @date 2023/5/31 16:00
 *  JDK提供的排序方法
 *  Arrays.sort(数组) 将数组按照升序排序
 *  Arrays.toString(数组) 将数组转换为字符串
 *  Arrays.binarySearch(数组，查找的元素) 使用二分查找返回对应元素的下标
 *  Arrays.fill(数组，元素) 指定元素填充数组
 *  Arrays.copyOf(数组，新长度) 复制数组
 */
public class TestArrays {
    public static void main(String[] args) {
        int [] nums = {78,412,89,63,89,412,0,74,5};

        Arrays.sort(nums);

        System.out.println(Arrays.toString(nums));

        System.out.println(Arrays.binarySearch(nums, 5));


        int [] arr = new int [5];
        Arrays.fill(arr, 666);

        System.out.println(Arrays.toString(arr));


    }
}

```



### 9. 二维数组(了解)

> 二维数组：数组中的元素还是数组
>
> 二维数组使用
>
> 声明  : 二维数组在声明的时候 高维度的长度 必须写 低维度的长度  可以分开写

```java
package com.atguigu.test6;

/**
 * @author WHD
 * @description TODO
 * @date 2023/5/31 16:12
 *  二维数组使用
 *  声明  : 二维数组在声明的时候 高维度的长度 必须写 低维度的长度  可以分开写
 *
 */
public class TestArrayUse {
    public static void main(String[] args) {
        // 创建方式1
        int [][] arr1 = new int[3][];

        arr1[0] = new int[5];
        arr1[0][0] = 11;
        arr1[0][1] = 12;
        arr1[0][2] = 13;
        arr1[0][3] = 14;
        arr1[0][4] = 15;

        arr1[1] = new int[3];
        arr1[1][0] = 22;
        arr1[1][1] = 22;
        arr1[1][2] = 22;

        arr1[2] = new int[4];
        arr1[2][0] = 55;
        arr1[2][1] = 55;
        arr1[2][2] = 55;
        arr1[2][3] = 55;



        // 创建方式2
        int [][] arr2;
        arr2 = new int[2][3];

        // 创建方式3
        int [][] arr3 = new int[][]{{1},{2},{3}};

        // 创建方式4
        int [][] arr4 = {{1},{2},{3}};


        System.out.println("---------------------------------------------------------");

        for(int i = 0;i < arr1.length;i++){
            for(int j = 0;j < arr1[i].length;j++){
                System.out.print(arr1[i][j] + "\t");
            }
            System.out.println();
        }
    }
}

```

```java
package com.atguigu.test6;

/**
 * @author WHD
 * @description TODO
 * @date 2023/5/31 16:07
 *  二维数组：数组中的元素还是数组
 */
public class TestArray {
    public static void main(String[] args) {
        int [] nums1 = {1,2,3,4,5};
        int [][] nums2 = {    {1,2,3}    ,        {55,66}     ,     {789,456,123,1245}   };

        System.out.println(nums2[0]);
        System.out.println(nums2[0][0]);
        System.out.println(nums2[0][1]);
        System.out.println(nums2[0][2]);

        System.out.println("----------------------------");

        System.out.println(nums2[1]);
        System.out.println(nums2[1][0]);
        System.out.println(nums2[1][1]);
        System.out.println("----------------------------");

        System.out.println(nums2[2]);
        System.out.println(nums2[2][0]);
        System.out.println(nums2[2][1]);
        System.out.println(nums2[2][2]);
        System.out.println(nums2[2][3]);





    }
}

```





