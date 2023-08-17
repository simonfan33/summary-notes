# 一、原生Tomcat
Tomcat解压目录/conf/server.xml配置文件中找到第一个Connector标签：
```xml
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```



# 二、IDEA整合的Tomcat
- 修改原生Tomcat，IDEA中会自动同步新配置
- 仅仅只是修改IDEA中的配置，那么原生Tomcat不会被修改