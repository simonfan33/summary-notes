# 一、学习目标
## 1、总目标
- 使用Servlet处理请求
- 使用Servlet返回响应

<br/>

## 2、拆解目标
### ①操作（重要）
- 给Web module添加Servlet开发所需依赖
- 基于IDEA创建Servlet
- 在web.xml中完成对Servlet的配置
- 点击超链接以GET请求方式发送请求访问Servlet的doGet()方法
- 提交表单以POST请求方式发送请求访问Servlet的doPost()方法
- 通过response对象返回响应数据

<br/>

### ②理论
- Servlet概念
- Servlet API体系
- Servlet生命周期与Servlet容器（重要）
- Servlet方法调用过程
- ServletConfig对象（了解）
- ServletContext对象（重要）

<br/>

### ③了解
- Servlet通过注解映射路径
- Servlet路径映射的其它方式
	- 精确匹配
	- 目录匹配
	- 扩展名匹配

<br/>

# 二、学习内容：操作部分
## 1、Servlet概念
- Server+applet=Servlet 连起来就是服务器端的小程序
- Server：服务器
- applet：小程序

## 2、Servlet功能
- 接收并处理前端发来的请求
- 给前端返回响应结果
- 控制页面跳转（转发、重定向）

## 3、创建Web module
- 创建普通module
- 在新建的module上点右键，点Add Frameworks Support
- 勾选Web Application
- 点OK
- 删除index.jsp

## 4、给Web module添加Servlet所需依赖
- File菜单下找到Project Structure
- 在Project Structure界面中找到modules
- 在modules中找到新建的module
- 切换到Dependencies选项卡
- 在Dependencies选项卡这里点加号
- 在弹出的菜单中选择Library...
- 选中Tomcat这一项
- Add Selected
- Apply然后OK

## 5、在IDEA协助下创建Servlet
- 指定Servlet的类名
- 包名
- 把基于注解开发的钩钩去掉
- 点击OK
- IDEA会自动帮助我们创建Servlet的类
- 我们需要把web.xml中Servlet的配置完成
```xml
<!-- servlet 标签：配置 Servlet 本身 -->  
<servlet>  
    <!-- servlet-name 标签：由于全类名太长，为了便于其它地方引用，声明一个简短、友好的名称 -->  
    <servlet-name>HelloServlet</servlet-name>  
  
    <!-- servlet-class 标签：指定 Servlet 全类名 -->  
    <servlet-class>com.atguigu.servlet.HelloServlet</servlet-class>  
</servlet>  
  
<!-- servlet-mapping 标签：配置 Servlet 的映射关系 -->  
<servlet-mapping>  
    <!-- servlet-name 标签：引用前面声明的 Servlet 名称 -->  
    <servlet-name>HelloServlet</servlet-name>  
  
    <!-- url-pattern 标签：配置能够映射到 HelloServlet 的 URL 地址 -->  
    <url-pattern>/HelloServlet</url-pattern>  
</servlet-mapping>
```

## 6、给HelloServlet发送GET请求
### ①编写超链接
```html
<!-- 浏览器认为路径开头的斜杠代表服务器根目录 -->  
<!-- 在服务器根目录下，我们还需要写上contextPath才能找到具体的某一个 Web 应用 -->  
<!-- 所以访问 Servlet 的路径在开头的斜杠前面还需要加上contextPath部分 -->  
<a href="/demo/HelloServlet">给HelloServlet发送GET请求</a>
```

### ②在doGet()方法中做一个打印
```java
public class HelloServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {  
	    // 这个打印是我们的一个里程碑：今天我们终于能够通过浏览器上的操作调用 Java 代码
        System.out.println("我是HelloServlet，我执行了doGet()方法！哇哈哈！");  
    }  
  
    @Override  
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {  
  
    }  
}
```

### ③在doGet()方法中给浏览器返回响应
```java
@Override  
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {  
    // 这个打印是我们的一个里程碑：今天我们终于能够通过浏览器上的操作调用 Java 代码  
    System.out.println("我是HelloServlet，我执行了doGet()方法！哇哈哈！");  
  
    // 调用 response 对象的方法获取字符输出流对象  
    PrintWriter writer = response.getWriter();  
  
    // 调用把我们希望返回给浏览器的响应数据写入字符输出流对象  
    writer.write("Hello!I am from HelloServlet doGet");  
}
```

## 7、给HelloServlet发送POST请求
### ①表单
```html
<form action="/demo/HelloServlet" method="post">  
    <button type="submit">给HelloServlet发送POST请求</button>  
</form>
```

### ②doPost()方法
```java
@Override  
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {  
    System.out.println("我是HelloServlet，我执行了doPost()方法！哇哈哈！");  
  
    response.getWriter().write("This is doPost method response.");  
}
```

# 四、学习内容：理论部分
## 1、Servlet的API体系
![images](./images/img001.png)

## 2、Servlet生命周期
### ①从Servlet接口说起
```java
public interface Servlet {
	// 做初始化操作（生命周期方法）
    void init(ServletConfig var1) throws ServletException;  

	// 获取 ServletConfig 对象
    ServletConfig getServletConfig();  

	// 每一次接收到请求都调用这个 service() 方法来处理请求（生命周期方法）
    void service(ServletRequest request, ServletResponse response) throws ServletException, IOException;  

	// 获取 Servlet 相关信息
    String getServletInfo();  

	// 在 Web 应用卸载之前执行清理操作（生命周期方法）
    void destroy();  
}
```

<br/>

### ②生命周期
#### [1]创建对象
- Tomcat根据Servlet的全类名（通过web.xml中的配置告诉了Tomcat）基于反射技术创建了Servlet的对象
- 在Servlet第一次接收到请求时创建对象，而且只创建一个。也就是说，在整个Web应用范围内，Servlet是单例的。
- 创建对象调用的是Servlet的无参构造器。

<br/>

#### [2]初始化
- 调用init()方法执行初始化操作
- 在创建对象之后立即执行
- 在整个生命周期中只执行一次

<br/>

#### [3]处理请求
- 每一次处理前端请求的时候执行
- 调用的方法
	- 从Servlet接口的角度来说：调用的是service()方法
	- 从继承HttpServlet的类来说：调用的是doXxx()方法
- 处理请求的操作可以执行多次


<br/>

#### [4]销毁or清理
- 调用destroy()方法执行销毁、清理操作
- 在Web应用卸载的过程中，Servlet对象被销毁之前执行
- 只执行一次

<br/>

#### [5]提出问题
在第一次接收到请求的时候，才执行创建对象、初始化的操作，这样的做法在特定场景下不合适。<br/><br/>

如果初始化环节要做的操作比较多，耗时较长，那么就会导致第一个请求的用户等待时间太长，用户体验很差。<br/><br/>

所以初始化操作最好是在Web应用启动的时候来做更合适。特别是将来我们使用框架之后。<br/><br/>

以后我们使用SpringMVC这个框架，它的核心就是一个名叫DispatcherServlet的组件。这个组件的初始化方法中要做的事情就很多：读取并解析配置文件、根据配置文件创建对象、组装各个组件……<br/><br/>

操作：
```xml
<!-- servlet 标签：配置 Servlet 本身 -->  
<servlet>  
    <!-- servlet-name 标签：由于全类名太长，为了便于其它地方引用，声明一个简短、友好的名称 -->  
    <servlet-name>HelloServlet</servlet-name>  
  
    <!-- servlet-class 标签：指定 Servlet 全类名 -->  
    <servlet-class>com.atguigu.servlet.HelloServlet</servlet-class>  
      
    <!-- 改变 Servlet 的启动顺序 -->  
    <load-on-startup>1</load-on-startup>  
</servlet>
```

<br/>

## 3、Servlet容器
### ①类比生活中的容器
|容器类别|生活中|代码中|
|---|---|---|
|简单容器|水杯|数组、List集合、Set集合|
|复杂容器|养鸡场|Servlet容器、IOC容器|

<br/>

复杂的容器不仅仅是存放对象，而且负责对象的一生：
- 创建对象
- 对象初始化
- 对象工作、干活儿
- 对象销毁

<br/>

负责管理Servlet生命周期的就是Servlet容器。具体来说，我们现在使用的Servlet容器就是Tomcat。

<br/>

Servlet的生命周期方法也都是Servlet容器调用的：
- 通过构造器创建对象
- 调用init()方法初始化
- 调用doXxx()方法处理请求，传入request、response对象
- 调用destroy()方法清理

<br/>

### ②Servlet标准和Servlet容器
类比JDBC：
- JDBC标准是为了屏蔽不同数据库产品之间的差异，让Java程序可以以相同的方式操作数据库
- JDBC标准是由一系列接口组成的
	- Connection：代表数据库连接
	- Statement：代表SQL语句
	- PreparedStatement：预编译的SQL语句
	- ResultSet：代表结果集
	- DataSource：代表数据源，也就是数据库连接池

<br/>

Servlet也包含一系列接口：
- jakarta.servlet.Servlet
- jakarta.servlet.Filter
- jakarta.servlet.ServletContextListener
- jakarta.servlet.http.HttpServletRequest
- jakarta.servlet.http.HttpServletResponse

<br/>

基于上述的接口，Servlet也是一套完备的标准。这套标准是为了屏蔽不同Servlet容器之间的差异。
- Tomcat
- Jetty
- JBoss
- Glassfish
- Weblogic
- ……

<br/>

只要Servlet容器和Web应用都遵循同一个版本的标准，就可以在不同容器之间平滑、无缝迁移。Servlet标准中的接口由各个Servlet容器提供实现。<br/>

例如：HttpServletRequest接口在Tomcat10的实现类是org.apache.catalina.connector.RequestFacade

<br/>

## 4、从service()到doGet()
![images](./images/img002.png)

## 5、ServletConfig对象
### ①来源
![images](./images/img003.png)

<br/>

### ②用途
参考ServletConfig接口就能够了解到它的用途：
```java
public interface ServletConfig {  
	// 获取web.xml中给Servlet起的名字
    String getServletName();  

	// 获取 ServletContext 对象
    ServletContext getServletContext();  

	// 获取 Servlet 的初始化参数：根据初始化参数名获取对应的值
    String getInitParameter(String paramName);  

	// 获取 Servlet 的初始化参数：获取所有初始化参数名称
    Enumeration<String> getInitParameterNames();  
}
```

### ③获取Servlet初始化参数
#### [1]配置Servlet初始化参数
```xml
<!-- servlet 标签：配置 Servlet 本身 -->  
<servlet>  
    <!-- servlet-name 标签：由于全类名太长，为了便于其它地方引用，声明一个简短、友好的名称 -->  
    <servlet-name>HelloServlet</servlet-name>  
  
    <!-- servlet-class 标签：指定 Servlet 全类名 -->  
    <servlet-class>com.atguigu.servlet.HelloServlet</servlet-class>  
  
    <!-- 改变 Servlet 的启动顺序 -->  
    <load-on-startup>1</load-on-startup>  
  
    <!-- 配置 Servlet 初始化参数 -->  
    <init-param>  
        <!-- 初始化参数的名称 -->  
        <param-name>SpringMVCConfigLocation</param-name>  
        <!-- 初始化参数的值 -->  
        <param-value>spring-mvc.xml</param-value>  
    </init-param>  
      
    <!-- 初始化参数可以配置多个 -->  
    <init-param>  
        <param-name>JDBCConfigLocation</param-name>  
        <param-value>jdbc.properties</param-value>  
    </init-param>  
</servlet>
```

#### [2]在Servlet中获取初始化参数
```java
// 获取 ServletConfig 对象  
ServletConfig servletConfig = super.getServletConfig();  
  
// 获取初始化参数  
String springMVCConfigLocation = servletConfig.getInitParameter("SpringMVCConfigLocation");  
System.out.println("springMVCConfigLocation = " + springMVCConfigLocation);  
  
String jdbcConfigLocation = servletConfig.getInitParameter("JDBCConfigLocation");  
System.out.println("jdbcConfigLocation = " + jdbcConfigLocation);
```

<br/>

### ④获取ServletContext对象
|生活例子|程序|
|---|---|
|将军|HelloServlet|
|随军主簿|ServletConfig|
|皇上|ServletContext|

```java
// 获取 ServletContext 对象  
ServletContext servletContext01 = servletConfig.getServletContext();  
System.out.println("servletContext01 = " + servletContext01);  
  
ServletContext servletContext02 = getServletContext();  
System.out.println("servletContext02 = " + servletContext02);
```

## 6、ServletContext
### ①介绍一下这个对象
Context：上下文、环境。所以ServletContext字面意思就表示Servlet工作的这个环境。<br/>

从对象本身的功能来说，ServletContext对象代表当前Web应用，在每个Web应用中有且只有一个ServletContext对象。<br/>

### ②ServletContext接口
- 接口全类名：jakarta.servlet.ServletContext
- Tomcat实现类：org.apache.catalina.core.ApplicationContextFacade

<br/>

### ③功能
- 作为域对象：存放数据
- 获取Web应用初始化参数
- 根据虚拟路径转换为真实物理路径（了解）

<br/>

### ④域对象
#### [1]为什么需要有域对象
在Java中，方法中局部变量的作用域仅限于方法内部。方法执行完，局部变量就会被释放，局部变量保存的数据也就没有了。<br/><br/>

可是Web应用开发过程中，经常出现一种情况：doXxx()执行完之后，里面有些数据别的方法还需要使用——我们需要让数据存在的时间周期超过方法本身的作用域。<br/><br/>

所以这样的时候就需要借助域对象来保存这样的数据。<br/><br/>

#### [2]域对象一共有三种
- 请求域：在一次请求的范围内有效。
- 会话域：在一次会话的范围内有效。
- 应用域：在整个Web应用范围内有效。

<br/><br/>

#### [3]测试
在HelloServlet存入数据：
```java
servletContext02.setAttribute("appAttrName", "appAttrValue");
```

<br/>

在AttrServlet中读取数据：
```java
Object appAttrValue = servletContext.getAttribute("appAttrName");
```

### ⑤获取Web应用初始化参数
#### [1]配置Web应用初始化参数
```xml
<!-- 配置Web应用初始化参数 -->  
<context-param>  
    <param-name>SpringConfigLocation</param-name>  
    <param-value>spring-core.xml</param-value>  
</context-param>
```

#### [2]读取Web应用初始化参数
```java
// 读取 Web 应用初始化参数  
String springConfigLocation = servletContext02.getInitParameter("SpringConfigLocation");  
System.out.println("springConfigLocation = " + springConfigLocation);
```

### ⑥把虚拟路径转换为真实物理路径（了解）
#### [1]概念
- 虚拟路径：通过Tomcat访问资源时，使用的路径。
- 真实物理路径：资源在物理硬盘上实际保存的路径。

#### [2]代码实现
```java
// 把虚拟路径转换为真实物理路径  
String virtualPath = "/images/wanghaodong.jpg";  
  
String realPath = servletContext02.getRealPath(virtualPath);
```

#### [3]意义何在
Web应用在不同环境下部署，各个服务器环境下真实物理路径都有所不同。<br/>
所以如果在Java代码中把真实物理路径写死，那么应用程序将不具备任何可迁移性。<br/>
而虚拟路径是以Web应用根目录为基准的路径，这个路径是不随部署环境不同而改变的。<br/>
所以基于不变的虚拟路径，调用ServletContext对象实时、动态获取当前环境下物理路径这就是最好的方案。<br/>

<br/>

#### [4]应用场景
什么时候一定要使用物理磁盘的真实路径？主要是I/O操作的时候需要使用。<br/>
比如通过new FileInputStream()读取服务器上某个文件时，就需要真实物理磁盘路径。

# 五、了解内容
## 1、使用注解映射Servlet路径
```java
@WebServlet(name = "AnnotationServlet", value = "/AnnotationServlet")  
public class AnnotationServlet extends HttpServlet {  
    @Override  
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {  
        response.getWriter().write("This Servlet mapping by annotation.");  
    }  
  
    @Override  
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {  
  
    }  
}
```

## 2、Servlet路径匹配其它方式
```xml
<servlet>  
    <servlet-name>PathServlet</servlet-name>  
    <servlet-class>com.atguigu.servlet.PathServlet</servlet-class>  
</servlet>  
<servlet-mapping>  
    <servlet-name>PathServlet</servlet-name>  
  
    <!-- 模糊匹配，在后半部分使用通配符 -->  
    <url-pattern>/atguigu/*</url-pattern>  
  
    <!-- 模糊匹配，在前半部分使用通配符，也可以称之为扩展名匹配 -->  
    <url-pattern>*.atguigu</url-pattern>  
  
    <!-- 错误写法，不允许中间使用通配符 -->  
    <url-pattern>/*.atguigu</url-pattern>  
</servlet-mapping>
```

# 六、今天的总结
- 操作部分：
	- 基于IDEA创建Servlet
	- 编写超链接访问doGet()方法
	- 编写表单访问doPost()方法
	- 使用response对象返回响应在浏览器显示
- 理论部分：
	- Servlet生命周期中的各个环节
		- 名称叫法
		- 在什么时候执行
		- 执行几次
	- Servlet创建对象
		- 默认在什么时候创建对象
		- 如何让Servlet在Web应用启动时创建对象
	- 理解Servlet容器的概念
	- ServletContext对象
		- 域对象的概念
		- 如何读写ServletContext域（应用域）
		- 通过ServletContext读取Web应用初始化参数