## IO

### 1. 字节流

#### 1.1 读(InputStream)

* FileInputStream

> InputStream
>
> FileInputStream
>
>
> FileInputStream(File file) 读取传入的File对象
>
> FileInputStream(String fileName) 根据传入字符串读取文件
>
>
>
> available() 获取当前字节读取流可读字节数
>
> read() 每次读取一个字节 返回值为读取的内容 读取到文件末尾 返回值为-1
>
> read(byte [] data) 每次读取一个字节数组 返回值为读取字节的个数  读取到文件末尾 返回值为-1

```java
package com.atguigu.test3;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/14 11:13
 *  InputStream
 *  FileInputStream
 *
 *  FileInputStream(File file) 读取传入的File对象
 *  FileInputStream(String fileName) 根据传入字符串读取文件
 *
 *
 *  available() 获取当前字节读取流可读字节数
 *  read() 每次读取一个字节 返回值为读取的内容 读取到文件末尾 返回值为-1
 *  read(byte [] data) 每次读取一个字节数组 返回值为读取字节的个数  读取到文件末尾 返回值为-1
 */
public class TestFileInputStream1 {
    public static void main(String[] args) throws IOException {

        FileInputStream fileInputStream = new FileInputStream("A.txt");

        int readData = -1;

        while((readData = fileInputStream.read()) != -1){
            System.out.println((char)readData);
        }

        // 关闭资源
        fileInputStream.close();



    }
}

```

> read(byte [] data) 每次读取一个字节数组 返回值为读取字节的个数  读取到文件末尾 返回值为-1
>
>
> 中文占几个字节：
>
> GBK 占2个字节
>
> UTF-8 占3个字节

```java
package com.atguigu.test3;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/14 11:20
 *  read(byte [] data) 每次读取一个字节数组 返回值为读取字节的个数  读取到文件末尾 返回值为-1
 *
 *  中文占几个字节：
 *  GBK 占2个字节
 *  UTF-8 占3个字节
 *
 */
public class TestFileInputStream2 {
    public static void main(String[] args) {
        try {
            FileInputStream fileInputStream = new FileInputStream("A.txt");

            System.out.println(fileInputStream.available() + "字节");

            byte [] data = new byte[fileInputStream.available()];

            int readCount = -1; // 读取字节的个数

            while((readCount = fileInputStream.read(data)) != -1){
                System.out.println(new String(data, 0, readCount));
            }

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```



#### 1.2 写(OutputStream)

* FileOutputStream

> OutputStream
>
> FileOutputStream
>
>
>
> FileOutputStream (File file)
>
> FileOutputStream (File file,boolean append)
>
> FileOutputStream (String file)
>
> FileOutputStream (String file,boolean append)
>
>
>
> void write(int c)
>
>
> void close()

```java
package com.atguigu.test3;

import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/14 11:33
 * OutputStream
 * FileOutputStream
 * <p>
 * FileOutputStream (File file)
 * FileOutputStream (File file,boolean append)
 * FileOutputStream (String file)
 * FileOutputStream (String file,boolean append)
 * <p>
 * void write(int c)
 *
 * void close()
 */
public class TestFileOutputStream1 {
    public static void main(String[] args) {
        FileOutputStream fileOutputStream = null;
        try {
            fileOutputStream = new FileOutputStream("B.txt", true);

            fileOutputStream.write(100);
            fileOutputStream.write(101);
            fileOutputStream.write(102);

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            // 关闭资源
            try {
                if (fileOutputStream != null) {
                    fileOutputStream.close();
                }
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }
}

```

> void write(byte[] buf)

```java
package com.atguigu.test3;

import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/14 11:39
 *  void write(byte[] buf)
 */
public class TestFileOutputStream2 {
    public static void main(String[] args) throws IOException {
        FileOutputStream fileOutputStream = new FileOutputStream("C.txt",true);

        String str = "中国 世界你好好 hello world 666";

        byte[] data = str.getBytes();

        fileOutputStream.write(data);

        fileOutputStream.close();

    }
}

```



### 2. 字符流

#### 2.1 读(Reader)

##### 2.1.1 InputStreamReader

> Reader
>
> InputStreamReader
>
>
> InputStreamReader(InputStream inputStream)
>
> InputStreamReader(InputStream inputStream,String charSetName)
>
>
> read() 每次读取一个字符 返回值为读取的内容  如果读取到末尾 返回值为-1
>
>
> close() 关闭资源

```java
package com.atguigu.test4;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.InputStreamReader;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/14 14:03
 *  Reader
 *  InputStreamReader
 *
 *  InputStreamReader(InputStream inputStream)
 *  InputStreamReader(InputStream inputStream,String charSetName)
 *
 *  read() 每次读取一个字符 返回值为读取的内容  如果读取到末尾 返回值为-1
 *
 *  close() 关闭资源
 */
public class TestInputStreamReader1 {
    public static void main(String[] args) throws IOException {

        FileInputStream fileInputStream = new FileInputStream("C.txt");

        InputStreamReader reader = new InputStreamReader(fileInputStream);

        int read1 = reader.read();
        System.out.println("read1 = " + (char)read1);

        int read2 = reader.read();
        System.out.println("read2 = " + (char)read2);

        int read3 = reader.read();
        System.out.println("read3 = " + (char)read3);

        int read4 = reader.read();
        System.out.println("read4 = " + (char)read4);

        System.out.println("-----------------------------------------");

        int readData = -1;
        while((readData = reader.read()) != -1){
            System.out.println((char)readData);
        }

        // 先用 后关
        reader.close();
        fileInputStream.close();







    }
}

```

>  read(char [] data) 每次读取一个char数组 返回值为读取的字符数 读取的内容存在数组中  如果读取到末尾 返回值为-1

```java
package com.atguigu.test4;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.InputStreamReader;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/14 14:10
 *  read(char [] data) 每次读取一个char数组 返回值为读取的字符数 读取的内容存在数组中  如果读取到末尾 返回值为-1
 */
public class TestInputStreamReader2 {
    public static void main(String[] args) {
        FileInputStream fileInputStream = null;
        InputStreamReader reader = null;
        try {
            fileInputStream = new FileInputStream("C.txt");
            reader = new InputStreamReader(fileInputStream);

            char [] data = new char[10];
            int readCount = -1;
            while((readCount = reader.read(data)) != -1){

                System.out.println(new String(data, 0, readCount));
//                System.out.println(data);
            }

            int [] nums = {1,2,3,4,5};
            System.out.println("nums = " + nums);

            char [] chs = {'a','b','c'};
            System.out.println( chs);

        } catch (FileNotFoundException e) {
            throw new RuntimeException(e);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }finally{
            // 关闭资源

        }

    }
}

```

> 使用InputStreamReader解决读取文件乱码问题

```java
package com.atguigu.test4;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.InputStreamReader;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/14 14:16
 *  使用InputStreamReader解决读取文件乱码问题
 *
 */
public class TestInputStreamReader3 {
    public static void main(String[] args) throws Exception {
        FileInputStream inputStream = new FileInputStream("C:\\Users\\WHD\\Desktop\\0324.txt");

        InputStreamReader reader = new InputStreamReader(inputStream, "gbk");

        System.out.println(System.getProperty("file.encoding"));

        int readData = -1;

        while((readData = reader.read()) != -1){
            System.out.println((char)readData);
        }

        reader.close();
        inputStream.close();







    }
}

```



##### 2.1.2 FileReader

> Reader
>
> InputStreamReader
>
> FileReader  此类只能按照本地平台默认的编码格式读取文件 不能指定编码格式
>
>
>
> read() 读取一个字符

```java
package com.atguigu.test4;

import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/14 14:22
 *  Reader
 *  InputStreamReader
 *  FileReader  此类只能按照本地平台默认的编码格式读取文件 不能指定编码格式
 *
 *
 *  read() 读取一个字符
 *
 */
public class TestFileReader1 {
    public static void main(String[] args) throws IOException {
        FileReader reader = new FileReader("C.txt");

        int readData = -1;
        while((readData = reader.read()) != -1){
            System.out.println((char)readData);
        }

        reader.close();


    }
}

```

> read(char [] data) 读取一个字符数组

```java
package com.atguigu.test4;

import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/14 14:24
 *  read(char [] data) 读取一个字符数组
 */
public class TestFileReader2 {
    public static void main(String[] args) throws IOException {
        FileReader reader = new FileReader("C.txt");

        char [] data = new char[100];

        int readCount =  reader.read(data);

        System.out.println(new String(data,0,readCount));

        reader.close();

    }
}

```



##### 2.1.3 BufferedReader

>  Reader
>
> BufferedReader 可以提高读取文件的效率 是带有缓冲区的字符读取流 默认有缓冲大小  读取到的内容先保存在缓冲区
>
> 然后一次读取到程序中 以此 减少与内存 IO的次数 提高读取效率
> *
>
> readLine() 每次读取一行 返回值为String对象

```java
package com.atguigu.test4;

import java.io.*;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/14 14:30
 *  Reader
 *  BufferedReader 可以提高读取文件的效率 是带有缓冲区的字符读取流 默认有缓冲大小  读取到的内容先保存在缓冲区
 *  然后一次读取到程序中 以此 减少与内存 IO的次数 提高读取效率
 *
 *  readLine() 每次读取一行 返回值为String对象
 */
public class TestBufferedReader1 {
    public static void main(String[] args) throws IOException {
        FileInputStream inputStream = new FileInputStream("C.txt");
        InputStreamReader inputStreamReader = new InputStreamReader(inputStream);
        BufferedReader reader = new BufferedReader(inputStreamReader);

        String line = null;

        while((line = reader.readLine())!= null){
            System.out.println(line);
        }

        reader.close();
        inputStreamReader.close();
        inputStream.close();
    }
}

```



#### 2.2 写(Writer)

##### 2.2.1 OutputStreamWriter

> Writer
>
> OutputStreamWriter 可以指定写入文件编码格式的字符流
>
>
> OutputStreamWriter(OutputStream stream)
>
> OutputStreamWriter(OutputStream stream,String charSet) 指定写入文件的编码格式
>
>
> write(String str)  写入字符
>
> void close() 关闭资源 关闭资源之前会自动刷新
>
> flush() 将缓存中的内容刷新到硬盘

```java
package com.atguigu.test5;

import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStreamWriter;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/14 14:43
 *  Writer
 *  OutputStreamWriter 可以指定写入文件编码格式的字符流
 *
 *  OutputStreamWriter(OutputStream stream)
 *  OutputStreamWriter(OutputStream stream,String charSet) 指定写入文件的编码格式
 *
 *  write(String str)  写入字符
 *  void close() 关闭资源 关闭资源之前会自动刷新
 *  flush() 将缓存中的内容刷新到硬盘
 */
public class TestOutputStreamWriter1 {
    public static void main(String[] args) throws IOException {

        FileOutputStream outputStream = new FileOutputStream("D.txt");

        OutputStreamWriter outputStreamWriter = new OutputStreamWriter(outputStream);

        outputStreamWriter.write("abc hello world 世界你好 666");

        outputStreamWriter.close();

        outputStream.close();

    }
}

```

> 指定写入文件编码格式

```java
package com.atguigu.test5;

import java.io.*;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/14 14:48
 */
public class TestOutputStreamWriter2 {
    public static void main(String[] args) throws IOException {
        FileOutputStream fileOutputStream = new FileOutputStream("E.txt");

        OutputStreamWriter writer = new OutputStreamWriter(fileOutputStream,"GBK");

        writer.write("世界你好 hello world 6666");

        writer.close();

        InputStreamReader reader = new InputStreamReader(new FileInputStream("E.txt"), "GBK");

        char [] data = new char[100];

        int readCount = reader.read(data);

        System.out.println(new String(data,0,readCount));



    }
}

```



##### 2.2.2 Filewriter

>  Writer
>
> OutputStreamWriter
>
> FileWriter 只能按照本地平台默认的编码规则写入文件

```java
package com.atguigu.test5;

import java.io.FileWriter;
import java.io.IOException;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/14 14:51
 *  Writer
 *  OutputStreamWriter
 *  FileWriter 只能按照本地平台默认的编码规则写入文件
 */
public class TestFileWriter1 {
    public static void main(String[] args) throws IOException {
        FileWriter writer = new FileWriter("F.txt");

        writer.write("hello world 世界你好66666 ");

        writer.close();
    }
}

```



##### 2.2.3 BufferedWriter

> Writer
>
> BufferedWriter 带有缓冲区的写入流  可以提高写入文件的效率
>
> newLine() 换行

```java
package com.atguigu.test5;

import java.io.*;
import java.text.FieldPosition;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/14 14:53
 *  Writer
 *  BufferedWriter 带有缓冲区的写入流  可以提高写入文件的效率
 *  newLine() 换行
 */
public class TestBufferedWriter1 {
    public static void main(String[] args) throws IOException {

        FileOutputStream fileOutputStream = new FileOutputStream("G.txt",true);
        OutputStreamWriter outputStreamWriter = new OutputStreamWriter(fileOutputStream);
        BufferedWriter writer = new BufferedWriter(outputStreamWriter);


        writer.write("hello \n world \n  世界 \n  你好 \n 66666");

        writer.newLine();

        writer.flush();

        writer.close();

        outputStreamWriter.close();

        fileOutputStream.close();




    }
}

```



### 3. 数据流

> DataInputStream  负责读取二进制
>
> DataOutputStream  负责写入二进制文件
>
> 以上两个类依然属于字节流对象
>
>
> 读写二进制文件

```java
package com.atguigu.test6;

import java.io.*;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/14 15:18
 *  DataInputStream  负责读取二进制
 *  DataOutputStream  负责写入二进制文件
 *  以上两个类依然属于字节流对象
 *
 *  读写二进制文件
 */
public class TestDataStream {
    public static void main(String[] args) throws IOException {
        FileInputStream fileInputStream = new FileInputStream("D:\\beauty\\GOGO全球高清大胆美女花苑制服诱惑丝袜美腿丝袜亚洲综合\\1a160d77f4.jpg");

        DataInputStream dataInputStream = new DataInputStream(fileInputStream);

        byte [] data = new byte[fileInputStream.available()];

        dataInputStream.read(data);

        FileOutputStream fileOutputStream = new FileOutputStream("girl.jpg");

        DataOutput dataOutput = new DataOutputStream(fileOutputStream);

        dataOutput.write(data);







    }
}

```



### 4. 对象流

> 序列化：将对象保存在二进制文件中 称之为序列化
>
> 反序列化 ： 将保存有对象的二进制文件 读取为对象 称之为 反序列化
>
>
> ObjectInputStream   readObject()读取对象
>
> ObjectOutputStream  writeObject()写入对象
>
>
> 被序列化的对象所属的类 必须实现 Serializable接口
>
> 此接口是一个空接口 只相当于一个标识 没有任何抽象方法
>
>
> 如果需要某个属性不被序列化 可以使用transient关键字修饰

```java
package com.atguigu.test6;

import java.io.Serializable;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/14 15:33
 */
public class Person implements Serializable {
    private transient String name;
    private int age;

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public Person() {
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
}

```

```java
package com.atguigu.test6;

import java.io.*;

/**
 * @author WHD
 * @description TODO
 * @date 2023/6/14 15:30
 *  序列化：将对象保存在二进制文件中 称之为序列化
 *  反序列化 ： 将保存有对象的二进制文件 读取为对象 称之为 反序列化
 *
 *  ObjectInputStream   readObject()读取对象
 *  ObjectOutputStream  writeObject()写入对象
 *
 *  被序列化的对象所属的类 必须实现 Serializable接口
 *  此接口是一个空接口 只相当于一个标识 没有任何抽象方法
 *
 *  如果需要某个属性不被序列化 可以使用transient关键字修饰
 *
 */
public class TestObjectStream {
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        Person p1 = new Person("赵四", 20);
        Person p2 = new Person("大拿", 21);
        Person p3 = new Person("广坤", 22);

        FileOutputStream fileOutputStream = new FileOutputStream("person.txt");

        ObjectOutputStream outputStream = new ObjectOutputStream(fileOutputStream);


        outputStream.writeObject(p1);
        outputStream.writeObject(p2);
        outputStream.writeObject(p3);


        System.out.println("写入完毕");


        FileInputStream fileInputStream = new FileInputStream("person.txt");

        ObjectInputStream inputStream = new ObjectInputStream(fileInputStream);

        System.out.println("------------------------------------------");

        while(fileInputStream.available() > 0){
            System.out.println(inputStream.readObject());
        }


        if (fileInputStream.available() > 0) {
            Object o = inputStream.readObject();

            System.out.println("o = " + o);

            System.out.println("------------------------------------------");

            if(o instanceof  Person){
                Person p = (Person) o;
                System.out.println("p = " + p);
            }

            System.out.println("------------------------------------------");

            o = inputStream.readObject();

            System.out.println("o = " + o);

            System.out.println("------------------------------------------");

            o = inputStream.readObject();

            System.out.println("o = " + o);
        }else{
            System.out.println("已经读取完毕");
        }


    }
}

```



