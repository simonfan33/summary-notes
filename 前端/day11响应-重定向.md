# 今日目标
- 关于页面渲染的简单说明
- request对象的使用：获取请求参数（重点）
- request对象的使用：获取请求相关信息
- request对象的使用：请求转发（重点）
- request对象的使用：请求域（重点）
- response对象的使用：PrintWriter
- response对象的使用：设置响应消息头
- response对象的使用：请求重定向（重点）
- 字符乱码问题
- 请求路径问题（重点）
- 会话控制概述

# 一、关于页面渲染的简单说明
## 1、提出问题
目前我们只能通过下面方式给浏览器返回响应：
```java
response.getWriter().write("This is doPost method response.");
```

<br/>

如果我们希望返回一个HTML页面：
```java
@Override  
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {  
    // 1、假设我们从数据库查询到了员工的总人数  
    int empCount = 20;  
  
    // 2、通过响应对象获取字符输出流  
    PrintWriter writer = response.getWriter();  
  
    // 3、拼响应结果  
    writer.write("<!DOCTYPE html>                   \n");  
    writer.write("<html lang='en'>                  \n");  
    writer.write("<head>                            \n");  
    writer.write("    <meta charset='UTF-8'>        \n");  
    writer.write("    <title>显示数据</title>       \n");  
    writer.write("</head>                           \n");  
    writer.write("<body>                            \n");  
    writer.write("                                  \n");  
    writer.write("    <p style='color:blue;'>员工总人数："+empCount+"</p>        \n");  
    writer.write("                                  \n");  
    writer.write("</body>                           \n");  
    writer.write("</html>                           \n");  
}
```

<br/>

这么做的缺陷：控制器和视图发生了耦合。

## 2、MVC
### ①概念
MVC：Model View Controller
- Model：模型（实体类Employee、Student、User、Book……）
- View：视图（能够给用户展示操作界面的技术：HTML、JSP、Thymeleaf……）
- Controller：控制器（XxxServlet）

<br/>

MVC是Web层项目开发所建议使用的程序的结构。但是MVC并不是二十三种设计模式中的一种。

<br/>

### ②含义
MVC这种设计理念主张在Web应用开发时，把模型、视图、控制器分开，基于组件化开发的思路，把Web应用拆分为各个不同组件，再把组件组装起来构成一个完整的Web应用。<br/><br/>

本质上还是体现了『高内聚、低耦合』的思维。<br/>

![images](./images/img001.png)
<br/>

组件化开发的好处：
- 开发过程：把一个很大、很复杂的功能拆分到各个组件中，每个组件独立开发，逻辑就简单多了。
- 维护过程：每个组件有它各自的功能，比较容易定位问题、解决问题。

<br/>

## 3、页面渲染
### ①页面不能写死的数据
![images](./images/img002.png)

### ②不能写死咋办？
要想让这些动态获取数据的地方真的实现数据的动态获取，那么就必须在这些位置写表达式。不管具体渲染的技术是什么。<br/>

<br/>

页面渲染：页面上不能写死、需要动态获取数据的地方写表达式。到程序真正执行的时候，通过执行表达式背后的代码（Java或JavaScript代码）计算出这里具体要显示什么数据。用实际的数据替换表达式的过程，就称为页面渲染。<br/>

![images](./images/img003.png)
<br/>

### ③页面渲染的技术
- 前端渲染
	- 使用axios这个Ajax框架从Servlet这获取数据
	- 使用JavaScript的框架Vue在页面上渲染
- 后端渲染
	- JSP
	- Velocity
	- Freemarker
	- Thymeleaf（后面学）
	- ……

<br/>

# 二、请求：获取请求参数（重点）
## 1、请求参数发送的形式
- 写在 URL 地址后面：
  - 手动在浏览器地址中包含的地址后面手写
  - 写在超链接href属性中地址后面
- 表单

```html
<a href="/demo/param?stuId=5&stuName=tom&subject=Java&subject=MySQL">通过超链接发送请求参数</a><br/>

<br/>

<form action="/demo/param" method="post">
    学生编号：<input type="text" name="stuId" value="66" /><br/>
    学生姓名：<input type="text" name="stuName" value="jerry" /><br/>
    学生科目：<input type="checkbox" name="subject" value="PHP" checked/>
    <input type="checkbox" name="subject" value="C++" checked /><br/>
    <button type="submit">保存数据</button>
</form>
```

## 2、请求参数接收的形式
```java
// 1、服务器端请求参数的保存的格式是：Map<String, String[]>
Map<String, String[]> parameterMap = request.getParameterMap();

// 2、遍历 Map 获取所有请求参数
Set<Map.Entry<String, String[]>> entries = parameterMap.entrySet();
for (Map.Entry<String, String[]> entry : entries) {
    String paramName = entry.getKey();
    String[] entryValueArr = entry.getValue();
    System.out.println(paramName + "=" + Arrays.toString(entryValueArr));
}

System.out.println("-------------------------------");

// 3、直接获取请求参数名对应的一个值
String stuId = request.getParameter("stuId");
System.out.println("stuId = " + stuId);

String stuName = request.getParameter("stuName");
System.out.println("stuName = " + stuName);

// 4、直接获取请求参数名对应的多个值
String[] subjects = request.getParameterValues("subject");
System.out.println("subjects = " + Arrays.asList(subjects));

// 5、请求参数名对应多个值，但是我用 getParameter() 方法获取
// ※此时只返回数组中的第一个值，实际开发时要注意，谨防数据遗漏
String subjectSingle = request.getParameter("subject");
System.out.println("subjectSingle = " + subjectSingle);
```

## 3、API
和请求方式无关：
- 获取全部请求参数：request.getParameterMap()
- 获取请求参数名对应的一个值：request.getParameter("stuId")
- 获取请求参数名对应的多个值：request.getParameterValues("subject")

# 三、请求：获取请求相关信息
- 获取上下文路径（重点）：request.getContextPath()
- 获取端口号：request.getServerPort()
- 获取主机名称：request.getServerName()
- 获取协议名：request.getScheme()
- 读取指定请求消息头：request.getHeader("User-Agent")

# 四、请求：请求的转发[重点]
## 1、概念
Servlet处理请求之后，再把请求转交给另一个资源继续处理。整个过程发生在服务器端，浏览器感知不到。<br/>

![images](./images/img0047.png)

<br/>

应用场景举例：
- Servlet先处理请求：调用Service执行业务运算，返回查询结果数据
- Servlet把查询结果数据存入请求域
- 把请求转发给pages/list.jsp页面
- list.jsp页面从请求域把数据取出来显示（渲染视图）

<br/>

## 2、实现方式
```java
public class ServletOne extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        System.out.println("ServletOne 执行了。☆☆☆");

        // 1、获取转发器对象，指定目标地址
        RequestDispatcher requestDispatcher = request.getRequestDispatcher("/servletTwo");

        // 2、调用转发器对象的 forward() 方法前往目标资源
        requestDispatcher.forward(request, response);
    }
}
```

<br/>

```java
public class ServletTwo extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("ServletTwo 执行了。☆☆☆");

        // 当前 Servlet 负责返回响应
        response.setContentType("text/html;charset=UTF-8");
        response.getWriter().write("来自ServletTwo的响应结果。");
    }
}
```

## 3、请求转发的特点
- 在请求转发的整个过程中，浏览器只发送了一个请求
- 转发的过程是在服务器端内部完成的，浏览器感知不到
- 请求完成之后，浏览器地址栏仍然保持着第一个资源（Servlet）的访问地址
- 因为从头到尾都是同一个请求，所以request对象也是同一个
- 转发的目标资源只能是当前Web应用内部的资源，不能转发到Web应用外部

<br/>

> 举例：请求转发用借钱来打个比方。<br/>
> 你找栋哥借钱。<br/>
> 但是栋哥自己没有，但是也没有拒绝你。<br/>
> 然后栋哥找别人借了钱，转交给你。

<br/>

## 4、相关问题
WEB-INF目录下的资源不允许浏览器直接访问，如果访问会看到404。<br/>
请求转发的方式可以访问。<br/>
```java
request.getRequestDispatcher("/WEB-INF/test.html").forward(request, response);
```

# 五、请求：向请求域保存数据[重点]
## 1、读写操作
读写操作必须使用同一个属性名，否则读取不到：
- 将数据存入请求域：request.setAttribute(属性名,属性值);
- 从请求域读取数据：request.getAttribute(属性名);

## 2、代码举例
### ①第一个Servlet存入数据
```java
public class ServletReqAttr extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 1、把数据存入请求域
        request.setAttribute("attrReqName", "attrReqValueAtguigu");

        // 2、转发到下一个 Servlet
        request.getRequestDispatcher("/ServletReqAttrGet").forward(request, response);
    }
}
```

### ②第二个Servlet读取数据
```java
public class ServletReqAttrGet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 1、从请求域读取数据
        Object attrReqValue = request.getAttribute("attrReqName");

        // 2、打印到页面上
        response.getWriter().write(attrReqValue.toString());
    }
}
```

<br/>

# 六、响应：PrintWriter
```java
@Override
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    // 1、获取 PrintWriter 对象
    PrintWriter writer = response.getWriter();

    // 2、写入响应数据
    writer.write("<html>");
    writer.write("<body>");
    writer.write("<p>hello</p>");
    writer.write("</body>");
    writer.write("</html>");
}
```

<br/>

# 七、响应：如何设置响应消息头
- 响应消息头相当于对本次响应和响应体进行说明

<br/>

```java
public class ServletResponseHeader extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 调用经过封装的方法，设置响应体内容类型
        // response.setContentType("text/html;charset=UTF-8");

        // 通过响应消息头形式设置：传入两个参数
        // 参数1：响应消息头属性名
        // 参数2：响应消息头属性值
        response.setHeader("Content-type", "text/html;charset=UTF-8");

        PrintWriter writer = response.getWriter();
        writer.write("你好！二姑！");
    }
}
```

<br/>

# 八、响应：请求的重定向(重点)
## 1、概念
服务器端给浏览器返回一个特殊响应，告诉浏览器去访问另一个资源

![images](./images/img0048.png)

> 用借钱来举例：<br/>
> 小明找栋哥借钱<br/>
> 栋哥说：我也没钱。但是我可以告诉你，谁有钱，你去找他就行。<br/>
> 然后小明就按栋哥说的，找另外那个人借到了钱。

<br/>

## 2、实现
```java
// response.sendRedirect("告诉浏览器需要继续访问的资源");
response.sendRedirect("/demo/target/target.html");
```

## 3、重定向的特点
- 整个过程浏览器会发送两个请求
- 服务器端通过两个设置告诉浏览器发生了重定向
  - 响应状态码：302
  - 响应消息头：Location设置为重定向的目标资源地址
- 浏览器参与了重定向的过程，所以能够感知到一共请求了两个资源
- 浏览器地址栏会变成第二个资源的地址
- 重定向的目标资源可以是Web应用外部的资源

## 4、重定向不能传递请求域
整个重定向过程涉及到两个请求，在服务器端是两个不同的请求对象，所以把数据存入第一个请求对象的属性域，无法在第二个请求中获取。

# 九、字符编码问题
## 1、编码和解码
一个字符，我们看到的是字符本身：'a'，但是在计算机底层实际是二进制的方式存储。
- 编码：把字符转换成对应的二进制数据就是编码。
- 解码：把二进制数据恢复为对应的字符就是解码。

乱码的原因就是：编码和解码用的不是同一个字符集。
- 编码：GBK（'中'：2233）
- 解码：UTF-8（2233：'大'）

## 2、请求参数
### ①GET请求
Tomcat版本≤7，需要修改配置。修改方式：在conf/server.xml配置文件中找到第一个Connector，新增属性：
```xml
<Connector URIEncoding="UTF-8" ... />
```
对GET请求来说，设置一次一劳永逸。

### ②POST请求
在获取请求参数前，设置请求体解码字符集为：UTF-8。每一个请求需要单独设置，做不到一劳永逸。Tomcat10不需要设置。<br/>

```java
// 设置请求体解码字符集为：UTF-8
request.setCharacterEncoding("UTF-8");

String yourName = request.getParameter("yourName");
```

## 3、返回响应
注意：如果Tomcat版本较低（比如Tomcat7），必须先设置响应体内容类型，然后再获取PrintWriter！<br/>

```java
// 设置响应体的编码字符集，但是并没有告诉浏览器用什么字符集解码
// response.setCharacterEncoding("UTF-8");

// 设置响应体内容类型，告诉浏览器使用 UTF-8 来解码
// 底层隐含的，服务器会自动使用相同字符集编码
response.setContentType("text/html;charset=UTF-8");
PrintWriter writer = response.getWriter();
writer.write("大西瓜，圆又圆。");
```

# 十、请求路径问题
> 路径总共有三种：
> ❤相对路径：在有Servlet请求转发的因素存在时，相对路径会出错。以后不建议使用。<br/>
> ❤绝对路径：建议使用的路径写法。<br/>
> ❤物理路径：不同部署环境下，物理路径大概率不同，会导致项目没有可迁移性，所以项目中不能使用。<br/>

<br/>

## 1、故障演示
### ①故障产生的流程
超链接访问Servlet，Servlet转发请求到target/target.html。目标页面有超链接：
```html
<a href="../index.html">回首页</a>
```

### ②故障原因
![images](./images/img0049.png)

## 2、相对路径的本质
并不是以开发的时候，我们的工程目录为参考依据；而是以实际运行时，浏览器地址栏上地址的目录结构为依据。<br/>
所以以后我们就不写相对路径了，都写绝对路径。

## 3、绝对路径的概念
### ①概念
- 路径在编写时，以“/”开头
- 路径开头的斜杠所代表的含义有区别：
  - 服务器：开头的斜杠代表当前Web应用
  - 浏览器：开头的斜杠代表当前服务器的根目录

![images](./images/img0050.png)

### ②区分
- 服务器解析的路径：请求转发时指定的路径、web.xml中配置的路径
- 浏览器解析的路径：HTML标签中、JavaScript代码中、Servlet中重定向代码里的路径

<br/>

> 本质上来说，路径由浏览器解析就比服务器解析的多一层contextPath

<br/>

## 4、绝对路径的写法
### ①浏览器解析的路径
- 因为路径开头的斜杠代表当前服务器，所以整个路径以contextPath开头（contextPath本身就是斜杠开头）
- 格式：/contextPath/具体资源路径
- 例如：/demo/index.html

<br/>

### ②服务器解析的路径
- 因为路径开头的斜杠代表当前Web应用，所以不需要写contextPath了，斜杠后面直接写具体资源的路径
- 格式：/具体资源路径
- 例如：/ServletSecond

<br/>

### ③步骤总结
- 第一步：先写一个斜杠
- 第二步：判断当前路径是谁解析的
  - 浏览器解析的路径：写contextPath，然后再写后面资源的路径
  - 服务器解析的路径：不用写contextPath，直接写具体资源

<br/>

## 5、从开发到部署
- 开发过程目录结构以及文件：活蹦乱跳的鸡
- 部署到Tomcat实例运行的目录结构以及文件：煮熟的鸡

从开发时的目录和文件到部署后的目录和文件，中间的过程是Java源程序的编译和目录的重组。<br/>

![iamges](./images/img0010.webp)

## 6、动态获取 contextPath
```java
// 由于 contextPath 每次部署时可以修改，所以最好动态获取
String contextPath = request.getContextPath();

// 然后再根据动态获取的 contextPath 执行重定向就稳妥了
response.sendRedirect(contextPath + "/target/target.html");
```

<br/>

# 十一、总结
## 1、理论
- 理解页面渲染概念
- 理解请求转发的机制
- 理论请求重定向的机制
- 会表述请求转发和重定向的区别

<br/>

## 2、操作
- 获取请求参数：一个名字带一个值
- 获取请求参数：一个名字带多个值
- 动态获取contextPath
- 转发请求的操作
- 重定向请求的操作
- 向请求域保存数据
- 从请求域读取数据
- 给响应体指定内容类型
- 能够正确编写请求转发的路径
- 能够正确编写请求重定向的路径