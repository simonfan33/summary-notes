# 今日目标
- 会话控制
- 过滤器
- 监听器

# 一、会话控制
## 1、提出需求
用户登录，假设正确的用户名、密码分别是：
- 账号：peter
- 密码：123456
用户通过表单填写账号、密码并提交给UserServlet。<br/>
UserServlet中检查用户名、密码是否正确。<br/>
- 登录成功：跳转到ListServlet查看数据列表，此时要求**保持用户的登录状态**
- 登录失败：回到登录表单页面

## 2、基础操作
略<br/>

## 3、会话控制核心操作（重中之重）
- 数据存入会话域：session.setAttribute("loginInfo", userName + "&" + userPwd);
- 从会话域读取数据：String loginInfo = (String) session.getAttribute("loginInfo");

<br/>

## 4、Cookie的工作机制
### ①设定场景
咖啡店发布了一个促销活动：消费五杯咖啡，赠送一杯。<br/>
又不能要求用户一次性消费完这五杯，用户肯定是不同的日期来店里消费，累积够五杯之后，再赠送。<br/>
可是店员不认识顾客，不能单凭顾客的一面之词就相信他消费了五杯。<br/>
所以咖啡店设计了一个卡片，顾客每消费一杯，就在卡片上盖一个印章。<br/>
集齐五个印章，赠送一杯。<br/>
本质上这里解决的是识别用户身份的问题。<br/>
对于我们服务器端程序，也有相同的问题：如何识别上一个、下一个请求是来自同一个浏览器。<br/>

### ②Cookie工作机制要点
- Cookie数据是在服务器端生成的
- 服务器端生成Cookie之后通过响应消息头返回给浏览器
- 浏览器保存Cookie信息
- 浏览器以后访问服务器都会自动携带Cookie
- 服务器端可以从请求对象中读取Cookie信息

<br/>

### ③操作：服务器创建并返回Cookie
```java
// 1、创建 Cookie 对象  
Cookie cookie = new Cookie("atguiguCookieName01", "atguiguCookieValue01");  
  
// 2、把 Cookie 对象添加到响应中  
// 底层会在响应消息头中附加下面内容：  
// Set-Cookie: atguiguCookieName01=atguiguCookieValue01  
response.addCookie(cookie);  
  
// 3、返回响应  
response.setContentType("text/html;charset=UTF-8");  
response.getWriter().write("success");
```

<br/>

响应消息头截图：<br/>

![images](./images/img001.png)

<br/>

![images](./images/img002.png)

<br/>

### ④观察：浏览器再次访问服务器自动携带Cookie

![images](./images/img003.png)

<br/>

### ⑤操作：服务器端读取Cookie信息
```java
response.setContentType("text/html;charset=UTF-8");  
PrintWriter writer = response.getWriter();  
  
// 1、通过 request 对象获取 Cookie 数组  
// ※注意：这个 Cookie 数组不一定有值，只有浏览器发送请求时携带的 Cookie 才有  
Cookie[] cookies = request.getCookies();  
  
// 2、对 Cookie 数组做一个判空保护  
if (cookies != null && cookies.length > 0) {  
  
    // 3、遍历 Cookie 数组  
    for (int i = 0; i < cookies.length; i++) {  
        Cookie cookie = cookies[i];  
        writer.write("Cookie Name:" + cookie.getName());  
        writer.write("<br/>");  
        writer.write("Cookie Value:" + cookie.getValue());  
    }  
} else {  
    writer.write("没有获取到任何Cookie信息。");  
}
```

<br/>

### ⑥理论：Cookie时效性
#### [1]从时效性角度Cookie分成两种
- 会话级：保存在浏览器的内存中，只要浏览器开着就一直在。浏览器关闭的时候释放。这是默认情况。
- 持久化：保存在浏览器的硬盘中，此时必然是服务器端设定的过期时间。到过期时间时被释放。

<br/>

#### [2]setMaxAge()方法
- expire参数是正数情况：设置 Cookie 以秒为单位作为过期时间
- expire参数是0情况：告诉浏览器删除这个 Cookie
- expire参数是负数情况：把 Cookie 设置为会话级

<br/>

```java
// 1、创建 Cookie 对象  
Cookie cookie = new Cookie("timedCookieName", "timedCookieValue");  
  
// 2、给 Cookie 设置过期时间  
// expire参数是正数情况：设置 Cookie 以秒为单位作为过期时间  
// expire参数是0情况：告诉浏览器删除这个 Cookie
// expire参数是负数情况：把 Cookie 设置为会话级  
cookie.setMaxAge(-10);  
  
// 3、在响应中添加 Cookieresponse.addCookie(cookie);  
  
// 4、返回响应  
response.setContentType("text/html;charset=UTF-8");  
response.getWriter().write("success");
```

<br/>

### ⑦理论：Cookie的domain和path
- Cookie的domain属性：设置Cookie所属的域名。发送请求时，域名不一致，不会携带这个Cookie。
- Cookie的path属性：设置Cookie在某个网站下，属于哪个具体资源。发送请求时，具体资源的路径不同，不会携带这个Cookie。

<br/>

domain举例说明：
- www.baidu.com 网站给浏览器返回了Cookie，那么这些Cookie只有访问百度这个网站时才会带。
- www.sina.com 网站给浏览器返回了Cookie，那么这些Cookie只有访问新浪这个网站时才会带。
- ……

<br/>

path举例说明：
- 通过path属性设定Cookie属于/aaa/bbb/ccc资源，那么访问/xxx/yyy/zzz资源就不携带这个Cookie

<br/>

如果强行把Cookie的domain设置为其它网站域名，那么浏览器会不接受：<br/>

![images](./images/img004.png)

### ⑧、Cookie的使用建议
- Cookie有如下限制：
	- 单个Cookie能够保存的数据非常有限
	- 浏览器保存Cookie时，不同网站的数量也是有限的
	- 来自于同一个网站的Cookie也只能保存有限的个数（约20个）
	- 用户随时可能删除浏览器端的所有Cookie
- Cookie的开发建议
	- 不要在Cookie中保存太多数据
	- 同一个域名下，Cookie保存的数量不要太多
	- 不要过于依赖Cookie作为数据保存的手段

<br/>

## 5、Session的工作机制
### ①咖啡买五送一
参见PPT<br/>

### ②代码演示Session工作机制
调用 request.getSession(); 方法获取 HttpSession 对象时：
- 当前请求中没有携带 JSESSIONID 的 Cookie
	- 服务器端创建新的 HttpSession 对象
	- 每一个 HttpSession 对象都有一个唯一标识：JSESSIONID
	- 基于 JSESSIONID 创建 Cookie
	- Cookie的名字是 "JSESSIONID"这个字符串，值就是 JESSIONID 对应的唯一值
	- 然后这个名叫 JSESSIONID 的 Cookie 会随着响应返回给浏览器
- 当前请求中携带了 JSESSIONID 的 Cookie
	- 根据 JSESSIONID 在服务器端查找对应的 HttpSession 对象
		- 能找到：把找到的 HttpSession 对象作为 getSession() 方法的返回值
		- 找不到：服务器端创建新的 HttpSession 对象……

```java
response.setContentType("text/html;charset=UTF-8");  
PrintWriter writer = response.getWriter();  
  
// 1、尝试获取 HttpSession 对象  
HttpSession session = request.getSession();  
  
// 2、调用 HttpSession 对象的 isNew() 方法查看这个 HttpSession 对象是否为新创建的  
boolean isNew = session.isNew();  
writer.write(isNew?"当前HttpSession对象是新创建的":"当前HttpSession对象是旧的，以前创建的");  
writer.write("<br/>");  
// 3、获取当前 HttpSession 对象的 id 值  
String id = session.getId();  
writer.write("当前HttpSession对象的id=" + id);
```

### ③时效性
#### [1]介绍
- 服务器端做 HttpSession 对象的管理有个目标：不能无限创建 HttpSession 对象。
- 一旦浏览器端 JSESSIONID 的 Cookie 丢失，留在服务器端的 HttpSession 对象就再也用不上了。
- 用不上的 HttpSession 对象留在服务器端就只是白白占用内存空间，时间长了会把内存耗尽。
- 所以我们不允许 HttpSession 对象永远保存在服务器。
- 具体做法是：一旦 HttpSession 对象空闲达到指定的时间，就会强行把它释放。
- 在Tomcat的conf/web.xml中可以看到默认设置：
```xml
  <!-- ==================== Default Session Configuration ================= -->
  <!-- You can set the default session timeout (in minutes) for all newly   -->
  <!-- created sessions by modifying the value below.                       -->

    <session-config>
        <session-timeout>30</session-timeout>
    </session-config>
```

<br/>

#### [2]测试代码
```java
// 1、获取 HttpSession 对象  
HttpSession session = request.getSession();  
  
// 2、修改最大空闲时间  
// Inactive：不活跃的  
// Interval：时间间隔  
session.setMaxInactiveInterval(10);
```

<br/>

#### [3]让Session立即失效
```java
session.invalidate();
```

<br/>

使用场景举例：用户退出登录时，把HttpSession对象彻底干掉，整个会话域全部释放。<br/>
所以要结合业务需求考虑清楚，是“一锅端”还是“定点清除”。
- 一锅端：session.invalidate();
- 定点清除：session.removeAttribute("属性名");

<br/>

# 二、过滤器
## 1、提出问题
![images](./images/img005.png)

<br/>

## 2、三要素
### ①拦截
作为过滤器这样的组件，首先需要能够把请求拦截住，然后才能做后续的相关操作。

<br/>

### ②过滤
通常是基于业务功能的需要，在拦截到请求之后编写特定的代码，对请求进行相关的处理或检查。<br/>

最典型的就是登录检查：检查当前请求是否已经登录。

<br/>

### ③放行
如果当前请求满足过滤条件，那么就应该放行：让请求继续去找它原本要访问的资源。

<br/>

## 3、HelloWorld
### ①创建Filter类
要求实现接口：jakarta.servlet.Filter。更简洁的做法是继承jakarta.servlet.http.HttpFilter类。

<br/>

```java
/**  
 * 假设请求中携带一个特定的请求参数表示用户已经登录，可以访问私密资源。  
 * 特定请求参数名称：message，特定的值：monster  
 */public class Filter01HelloWorld extends HttpFilter {  
  
    @Override  
    public void doFilter(HttpServletRequest request, HttpServletResponse response, FilterChain chain) throws ServletException, IOException {  
  
        // 1、获取请求参数  
        String message = request.getParameter("message");  
  
        // 2、检查请求参数是否满足预设的要求  
        if ("monster".equals(message)) {  
            // 3、满足条件的请求放行  
            chain.doFilter(request, response);  
        } else {  
            // 4、不满足预设条件就把请求转发到拒绝页面  
            request.getRequestDispatcher("/WEB-INF/pages/forbidden.html").forward(request, response);  
        }  
  
    }  
}
```

### ②注册
```xml
<!-- 注册 Filter --><filter>  
    <!-- Filter 友好名称 -->  
    <filter-name>Filter01HelloWorld</filter-name>  
  
    <!-- Filter 全类名 -->  
    <filter-class>com.atguigu.filter.filter.Filter01HelloWorld</filter-class>  
</filter>  
<filter-mapping>  
    <!-- 引用 Filter 友好名称 -->  
    <filter-name>Filter01HelloWorld</filter-name>  
  
    <!-- 当前 Filter 要拦截的请求的 URL 地址的匹配模式 -->  
    <url-pattern>/private/*</url-pattern>  
</filter-mapping>
```

<br/>

# 三、总结
## 1、操作
- 会话控制：
	- 把数据存入会话域
	- 从会话域读取数据
- Filter：完成HelloWorld
	- Filter类：继承HttpFilter，重写doFilter()方法
	- Filter类：放行操作，调用chain.doFilter(request, response);
	- web.xml注册

<br/>

## 2、理论
- 会话控制：
	- Cookie工作机制
	- Cookie时效性管理
	- HttpSession工作机制
	- HttpSession时效性管理
- Filter：
	- 应用场景
	- 过滤器的三要素