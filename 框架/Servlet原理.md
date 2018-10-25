# Servlet原理

Tomcat 执行流程

![1532941257159](E:\mydairy\框架\Tomcat执行流程.png)

Servlet是处理Http的请求并返回处理结果的一个组件

Tomcat与Servlet的关系

Tomcat 是Web应用服务器,是一个Servlet/JSP容器. Tomcat 作为Servlet容器,负责处理客户请求,把请求传送给Servlet,并将Servlet的响应传送回给客户.而Servlet是一种运行在支持Java语言的服务器上的组件. Servlet最常见的用途是扩展Java Web服务器功能,提供非常安全的,可移植的,易于使用的CGI替代品. 



Servlet的生命周期：

三大方法：

init()方法：第一次请求该servlet事，初始化一个Servlet对象，该方法只执行一次

service()方法：处理所有客户端的请求（doGet()  doPost() ）

destroy()方法：最后服务器关闭时，调用该方法销毁servlet对象。