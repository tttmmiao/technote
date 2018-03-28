# spring

## 基础



## 源码分析

### xmlBeanFactory

* spring配置文件是通过ClassPathResource封装的。
* XmlBeanDefinitionReader是对配置文件的读取
* ​



### Q

1.  **xml的验证模式**

   xml的验证模式是为了保证xml文件的正确性，常用的验证模式有DTD 和XSD.

   DTD(Document Type Definition)，文件类型定义。

   XSD(Xml Schemas Definition), xml schema描述了xml文档的结构。

2.  **spring 的context如何初始化**

*  可通过三种方式加载spring容器，实现bean的扫描和管理。
	- ClassPathXmlApplicationContext : 从类路径加载
	- FileSystemXmlApplicationContext: 从文件系统加载
	- XmlWebApplicationContext : 从web系统中加载（需要在web.xml中配置servlet或者listener的方式实现spring容器的加载）
		配置listener的方式
		
```xml
		<listener>  
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>  
	</listener>  
	<context-param>  
   	 <param-name>contextConfigLocation</param-name>  
    <param-value>/WEB-INF/spring-context.xml</param-value>  
	</context-param>
```
		
配置servlet的方式
		
```xml
		<servlet>
        <servlet-name>springServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath*:/spring-mvc*.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
```
	
	
spring mvc中需要同时使用以上两种方式。servlet方式负责配置controller等view层，listener方式负责配置service, dao等model层。	
	 

3.   **servlet是什么**
 
	Servlet是个接口，定义了一套处理网络请求的规范。
	所有实现Servlet接口的类，都要实现它的5个方法。
	![Servlet methods](https://pic1.zhimg.com/80/v2-85bf84640fbc6b6e195b9c5b513b918f_hd.jpg)
	
	最重要的3个类：
	* init()初始化时要做什么
	* destroy()销毁时要做什么
	* service()接收到请求后要做什么
	
	实现了servlet类，就能处理请求了吗？
	
	*不能！！*
	
	servlet不会和客户端打交道。
	
	请求如何到达servlet?
	
	使用servlet容器，比如tomcat, 将servlet部署到容器中。tomcat是直接和客户端打交道的，它监听了端口，请求到来后，根据url, 确定将请求交给哪个servlet去处理。接着调用对应servlet的service方法，service方法返回一个response对象，tomcat再把这个response返回给客户端。
	
	[how spring web mvc really works!](https://stackify.com/spring-mvc/)
	
	[tomcat interaction with servlet](https://www.mulesoft.com/cn/tcat/tomcat-servlet)
	
4. bean factory 和applicationContext之间的区别
	[bean-facotory vs applicationContext](https://docs.spring.io/spring/docs/2.5.x/reference/beans.html#context-introduction-ctx-vs-beanfactory)
	
   [the ioc container](https://docs.spring.io/spring/docs/2.5.x/reference/beans.html#context-introduction-ctx-vs-beanfactory)