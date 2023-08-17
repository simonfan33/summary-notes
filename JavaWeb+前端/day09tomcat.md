# 一、Tomcat
## 1、解压
解压到已非中文没有空格的目录。<br/>
<br/>

## 2、环境要求
要求正确安装JDK并且正确配置JAVA_HOME。<br/>
请使用java -version方式来测试是否正确。<br/>
<br/>
※特别提示：当初栋哥带我们装了两个版本的JDK，一个JDK8，一个JDK17。<br/>
现在Tomcat10要求使用JDK17。<br/>
此时要求JAVA_HOME环境变量一定要指向JDK17的安装目录（bin目录的上一级）。<br/>
这里要注意：Tomcat是不看PATH环境变量的，它只看JAVA_HOME。<br/>
<br/>

## 3、启动Tomcat
- ❤打开命令行窗口
- ❤进入Tomcat的bin目录
- ❤运行catalina run
<br/>

## 4、停止Tomcat
- ❤办法一：在启动Tomcat的窗口按Ctrl+c
- ❤办法二：运行shutdown.bat脚本
<br/>

## 5、解决乱码问题
- ❤找到安装目录下\apache-tomcat-10.1.7\conf\logging.properties文件
- ❤用文本编辑器打开这个文件
- ❤把所有UTF-8替换为GBK
- ❤Tomcat需要重新启动才能让配置生效

## 6、打开Tomcat自己的首页
http://localhost:8080<br/>
<br/>
http表示当前使用的协议。<br/>
localhost这个部分是IP地址，写IP地址是为了在网络上找到服务器所在的电脑。<br/>
localhost本身表示本机。<br/>
8080是在找到服务器所在电脑之后，找到Tomcat对应的进程。<br/>
8080是Tomcat进程监听的端口号。<br/>
<br/>
所以http://localhost:8080这个地址整体就是帮我们找到Tomcat服务器的根目录。<br/>

## 7、在Tomcat上部署Web应用
### [1]Web应用的目录结构
在Web应用根目录下可以存放各种静态资源。
- HTML页面
- CSS文件
- JavaScript文件
- 图片文件
为了让很多资源便于管理，通常会创建对应的目录。
- 专门存放HTML页面的目录。
- 专门存放CSS文件的目录。
- 专门存放JavaScript文件的目录。
- 专门存放图片文件的目录。
- ……
WEB-INF目录是必须有的，而且目录名必须是WEB-INF，一个字符都不能错。<br/>
WEB-INF目录下：
- .xml：是整个Web应用的核心配置文件。将来我们和Tomcat交互的Java程序都必须在web.xml中配置。
  - web.xml也被称为『部署描述符』，英文叫：deployment descriptor
- sses目录：这个目录就是我们常说的『类路径』 
  - 会放入类路径的资源一：src目录下的资源，通常是Java源程序。 
  - 会放入类路径的资源二：被标记为Resources Root的目录（通常存放配置文件）下的配置文件
- 目录：存放当前Web应用所需的第三方jar包。
<br/>

### [2]部署Web应用
把一个符合Web应用目录结构要求的目录放入Tomcat解压目录/webapps目录下。<br/>
<br/>

### [3]通过路径访问刚刚部署的Web应用
- ❤通过localhost:8080进入Tomcat服务器根目录
- ❤Tomcat服务器根目录映射到硬盘上，对应的就是webapps目录
- ❤在浏览器地址栏上想要在localhost:8080下面找到具体的Web应用，就相当于在webapps目录下找到Web应用的目录。
- ❤此时访问http://localhost:8080/demo/地址就进入了Web应用根目录。
- ❤此时访问http://localhost:8080/demo/index.html地址就是到Web应用根目录下找index.html这个具体的资源
- ❤此时访问http://localhost:8080/demo/static/tank.jpg地址就是Web应用根目录下再进入static目录，然后在static目录下再找到tank.jpg这个具体资源。
<br/>

## 8、在IDEA创建Tomcat镜像
https://gitee.com/heavy_code_industry/note-whole2023/blob/master/part03-FrontEnd/04_%E7%AC%AC%E5%9B%9B%E7%AB%A0%20XML_Tomcat_HTTP.md

## 9、浏览器访问Web应用路径和部署目录关系
![images](./note_images/img001.png)

<br/>

## 10、部署目录和module目录关系
![images](./note_images/img002.png)

<br/>

# 二、HTTP
## 1、概述
- HTTP：Hyper Text Transfer Protocol 超文本传输协议
- HTTP交互方式：
  - 建立连接
  - 发送请求
  - 返回响应
  - 关闭连接
- HTTP1.0和1.1区别
  - 1.0每个请求建立一个连接，效率较低
  - 1.1下载网页、下载网页中的图片等资源使用的是同一个连接

## 2、请求报文格式
### ①总体格式说明
- 报文首部：对当前请求和报文主体进行相关说明
- 空行：表示下面即将开始报文的正文
- 报文主体：当前请求主要要发送给服务器的部分

### ②报文首部
- 请求行：对当前请求进行简要说明
  - 请求方式：GET或POST
  - 请求地址：比如/module03_web_war_exploded/index.jsp
  - 协议名称和版本：HTTP/1.1
- 请求消息头：对当前请求、请求体进行详细说明
  - 格式都是键值对，键和值之间用冒号分开的
  - 常用需要留意的请求消息头：
    - Cookie：当前请求携带的Cookie信息
    - Referer：表示当前页面是从哪个页面来的
    - Content-Type：对请求体的数据类型进行说明

### ③GET请求和POST请求的区别
- GET请求没有请求体，POST请求有请求体
- 因为没有请求体，所以GET请求只能把请求参数附着在URL地址后面
- 因为附着在URL地址的后面
  - URL地址后面能够附着请求参数的数量是有限的
  - 敏感信息也会明文显示出来，不安全
- POST请求把请求参数放在请求体中，浏览器地址栏看不到
  - 请求体没有容量限制
  - 请求体不会被直接看到，更安全
- 表单建议使用POST方式提交
- 文件上传操作：必须使用POST请求，不能使用GET请求

<br/>

## 3、响应报文
### ①总体格式说明
- 报文首部：对当前响应和报文主体进行相关说明
- 空行：表示下面即将开始报文的正文
- 报文主体：当前响应主要要返回给浏览器的部分

### ②响应状态行
- 协议名称和版本号：HTTP/1.1
- 响应状态码：有很多，每一个码都有特定的含义
  - 比如：200表示请求处理成功，能够返回有效响应
- 响应状态码的说明信息
  - 比如：OK通常是对200进行说明

### ③响应消息头
- 总体格式和请求头一样，都是键值对格式，键和值之间用冒号分开
- Content-Type：说明响应体的内容类型
- Set-Cookie：服务器端给浏览器端返回Cookie信息

### ④响应体
浏览器拿到响应体就是要在浏览器窗口中显示的。例如：
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>目标页面</title>
</head>
<body>

    <h3>目标页面</h3>

</body>
</html>
```

### ⑤响应状态码
- 200： 请求成功，浏览器会把响应体内容（通常是html）显示在浏览器中；
- 404： 请求的资源没有找到，说明客户端错误的请求了不存在的资源；
- 405： 请求的方式不允许
- 500： 请求资源找到了，但服务器内部出现了错误；
- 302： 重定向，当响应码为302时，表示服务器要求浏览器重新再发一个请求，服务器会发送一个响应头Location指定新请求的URL地址；
- 304： 使用了本地缓存

### ⑥404问题解决
- 请求路径不正确，根据请求路径无法找到目标资源。
- 访问了WEB-INF目录下的资源
- Web应用启动时就抛出了异常，整个Web应用不可用，即使是正确的路径访问资源也是404
- 如果上面的方向都检查过了，没有问题，那么可能部署目录下不是按最新的代码运行的。此时重新构建、重新部署试试。
  - 把Tomcat停止
  - 到Edit Configurations...这里把Tomcat上现在部署的应用去掉
  - 到Build菜单下点Rebuild Project
  - 重新部署应用

# 三、总结
以下几点大家一定要做到：
- Tomcat解压、启动、访问操作
- 在IDEA中关联Tomcat
- 在IDEA中创建Web形式的module
- 把Web形式的module部署到IDEA关联的Tomcat上运行

<br/>

理论的部分结合以后的学习慢慢理解：
- Web应用的结构
- 通过浏览器访问Web应用时路径对照部署目录的目录结构来写
- 部署目录和开发时module的目录结构对应关系
- HTTP协议的结构
  - 请求报文
    - 报文首部
      - 请求行：请求方式 请求地址 协议名和版本
      - 请求消息头：键值对
    - 空行
    - 报文主体：只有POST请求才有请求体
  - 响应报文
    - 报文首部
      - 响应状态行：协议名称和版本 响应状态码 响应状态码说明
      - 响应消息头：键值对
    - 空行
    - 报文主体：给浏览器显示用的信息
    - 响应状态码：200、302、404、405、500
  - GET请求和POST请求的区别








