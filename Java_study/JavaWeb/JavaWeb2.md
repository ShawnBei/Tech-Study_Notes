# JavaWeb

## 1. 概述

> 1. Java Web 官方解决方案（官方架构）：
>
>    * Model2 结构，使用了 Servlet + JavaBean (EJB) + JSP。
>
>    * 但是目前主流的开发架构并非官方给出的架构，而是开源框架，典型代表：Struts、Spring 和 Hibernate，简称SSH。
>
> 2. J2EE 包含的技术：
>
>    * Servlet
>    * EJB
>    * JSP
>    * JDBC
>
>



## 2. MVC 设计模式

### 概述

![mvc-framework](img/mvc-framework.png)
MVC （Model-View-Controller）是一种设计模式，它把应用程序分成三个核心模块：模块、视图、控制器，各自处理自己的任务。

**模型（Model）**：

  - 模型是应用程序的主题部分，代表业务数据和业务逻辑。
  - 一个模型能为多个试图提供数据。
  - 由于应用于模型的代码只需要写一次就可以被多个视图重用，所以提高了代码的可重用性。

**视图（View）**：
  - 试图是用户看到并与之交互的界面，作用如下：
    - 试图想用户显示相关的数据。
    - 接收用户的输入。
    - 部进行任何实际的业务处理。

 **控制器**：
  - 控制器接收用户的输入并调用模型和试图去完成用户的需求。
  - 控制器接收请求并决定调用哪个模型组件去处理请求，然后决定调用哪个视图来显示模型处理返回的数据。












## 3. XML & Tomcat

> Tomcat 和 apache 的区别：
>
> * apache 是支持静态页的 WEB 服务器，而 Tomcat 是支持动态的（支持 JSP）。
> * apache 是用 C/C++ 编写的，而 Tomcat 是 Java 编写的。
> * apache 是 WEB 服务器，而 Tomcat 是应用服务器，两者是独立运行的，但都是 apache 组织开发的。
> * apache + Tomcat + JDK 组成的支持 JSP 的 WEB 服务器，如果客户端请求的是静态页面，则只需要 apache 服务器响应请求 如果客户端请求动态页面，则是Tomcat服务器响应请求。

### 3.1 Tomcat 目录结构

![img](img/Screenshot 2020-02-18 at 16.27.21.png)

注释：classes 文件夹中放我们编写的 java 文件编译出来的 .class 文件，lib 放 jar 包，web.xml 是服务器的配置文件。

**如何搭建一个 Tomcat 服务器** （使用 Eclipse 搭建一个简单的 WEB 工程）

````
http://loclhost:8080/HelloWeb/index.html
网址 		/ 项目名	 / 	子网页
````







## 4. HTTP

> HTTP (Hypertext Transfer Prototype)

1. 数据格式：
   * 请求行 / 请求头 / 请求体
   * 响应行 / 响应头 / 响应体

2. GET 和 POST 请求的区别
   * POST: 
     - 以流的形式传递数据，不会再地址栏上显示。
     - 一般客户端向服务器提交数据都用 POST，不用 GET。
   * GET: 
     - 会在地址栏中拼接数据，有安全隐患，一般从服务器获取数据，而且客户端不需要提交数据的时候用。
     - 拼接的数据有限制，1kb。


## Servlet

> **Servlet 是什么**：
> Servlet 是一个 Java 程序（组件），运行在我们的 web 服务器上（服务器负责Sevelet和客户的通信以及调用Servlet的方法），Servlet和客户的通信采用“请求/响应”的模式。 

**Servlet 能做的事**：
  - 创建并返回基于客户请求的动态HTML页面。
  - 创建可嵌入到现有HTML页面中的部分HTML页面。
  - 与其他服务器资源进行通信（比如数据库或者基于Java的应用程序）。

![servelt-c/s](img/servelt-c-s.png)

**注意**：Servelt容器掌控着 Servlet 的生命周期。



### 如何创建一个 Servlet （helloworld）

1. 在 Web 工程上创建一个类，实现 Servlet 接口。

2. 在 web.xml 中配置和映射这个 Servlet（因为希望可以通过浏览器来调用这个 java 类，所以需要映射）。

````xml
<servlet>
  <!-- Servlet 注册的名字-->
	<servlet-name>HelloServlet</servlet-name>
  <!-- Servlet 的全类名-->
	<servlet-class>com.ning.bei.HelloServlet</servlet-class>
</servlet>

<servlet-mapping>
  <!-- 需要和某一个Servlet节点的servlet-name子节点的文本节点一致-->
	<servlet-name>HelloServlet</servlet-name>
  <!-- 映射具体的访问路径：/ 代表当前的 web 应用的根目录-->
	<url-pattern>/hello</url-pattern>
</servlet-mapping>
````

3. **http://loclhost:8080/HelloWeb/**hello（加粗为根目录，通过url映射找到全类名，然后找到对应的java文件）

### Servlet 容器

运行Servlet、JSP、Filter等软件环境：

1. 可以用来创建Servlet，并调用Servlet的相关生命周期方法。
2. JSP，Filter，Listener，Tag....

### Servlet 生命周期方法

以下方法都是由servlet容器负责调用。

- **构造器**：只被调用一次。只有第一次请求Servlet时，创建Servlet实例，调用构造器。这说明Servlet是单例的！（有线程安全问题）

- **init 方法**：只被调用一次。在创建好实例后立即被调用。用于初始化当前Servlet。
- **service 方法**：被多次调用。每次请求都会调用 service 方法，实际用于响应请求。
- **destroy 方法**：只被调用一次。在当前Servlet所在的web应用被卸载前调用，用于释放当前servlet所占用的资源。

### load-on-startup参数

1. 配置在servlet节点中

````xml
<servlet>
  	<servlet-name>HelloServlet</servlet-name>
  	<servlet-class>com.ning.bei.HelloServlet</servlet-class>
  	<load-on-startup>2</load-on-startup>
</servlet>
````

2. 可以指定servlet被创建的时机，若为负数，则在第一次请求时被创建，若为0或正数，则在web应用被servlet容器加载时被创建，且数值越小越早被创建。

![servlet-response](img/servlet-response.png)

![servlet-sign-in](img/servlet-sign-in.png)

![servlet-mapping](img/servlet-mapping.png)

``*.扩展名`` 表示以扩展名结尾的url都调用这个servlet。



### **ServletConfig**

ServletConfig 是做为传入参数传入到 init 方法中的。

1. 封装了servlet的配置信息，并且可以获取 ServletContext 对象（getServletContext()）。

````xml
<!-- 配置 Servlet 的初始化参数，且节点必须在 load-on-startup 节点的前面 -->
<init-param>
  <!-- 参数名 -->
  <param-name>user</param-name>
  <!-- 参数值 -->
  <param-value>root</param-value>
</init-param>
````

2. 获取当前servlet的初始化参数：
   - getInitParameter(String name)：获取指定参数名的初始化参数。
   - getInitParameterNames()：获取参数名组成的 Enumeration。

> - 在使用别人开发的 servlet 时，需要自己配置 web.xml（注册该 servlet），并写入初始化时的参数。
> - 在开发 servlet 时，使用 ServletConfig 来获取用户写入的初始化参数。

### ServletContext

1. 可以由 ServletConfig 获取 ``ServletContext sc = servletConfig.getServletContext();`` 

2. 该对象代表当前 WEB 应用：可以认为 ServletContext 是当前web 应用的大管家，可以获取到当前 web 应用的各个方面的信息。

  - 获取当前 web 应用的初始化参数:
    1. 配置初始化参数：（可以为所有的servlet获取，而servlet的初始化参数只能是那个servlet可以获取）
   ````xml
   <!-- 配置当前 web 应用的初始化参数 -->
   <context-param>
     <param-name>driver</param-name>
     <!-- 参数值 -->
     <param-value>com.mysql.jdbc.Driver</param-value>
   </context-param>
   ````

    2. 方法：
      getInitParameter(String name)
      getInitParameterNames()

  - 获取当前web应用的某一个文件在服务器上的绝对路径，而不是部署前的路径：getRealPath(String path)；
  
  - 获取当前web应用的名称：getContextPath(); ``/note.txt``

  - 获取当前web应用的某一个文件对应的输入流：getResourceAsStream(String path): path 的 / 为当前web应用的根目录（和 classLoader 的获取路径有所区别）。

  - 和 attributes 相关的几个方法。





Servlet 通用写法

* Servlet --> GenericServlet --> HttpServlet(用于处理http	请求)
* 一般继承 HttpServlet, 并重写 doPost 和 doGet 方法。


## 通讯过程

### HTTP

浏览器和 web 服务器之间的连接过程是短暂的，每次连接只是处理一个请求和响应。对每一个页面的访问，浏览器和 web 服务器都要简历一次单独的连接。

浏览器到 web 服务器之间的所有通讯都是完全独立分开的请求和响应对。（例如，加载一个页面，包含三张图片，实际上需要四次连接用来传输三张图片和这页文档）

### 使用 GET 方式传递参数：

1. 在浏览器地址栏中输入某个 url 地址或单击网页上的一个超链接时，浏览器发出的 HTTP 请求消息的请求方式为 GET。
2. 如果在网页中的<form> 表单元素的 method 属性被设置成了 “GET”，浏览器提交这个 form 表单时生成的http请求消息的请求方式也是 GET。
3. 使用 GET 请求方式给 web 服务器传递参数的格式：http://www.ning.com/counter.jsp?name=user&password=123
4. 传送的数据量一半限制在 1kB 以下。

### 使用 POST 方式传递参数：

1. POST 请求方式主要用于向 web 程序提交 form 表单中的数据：form 表单的method 设置为 POST。
2. POST 方式将各个表单的字段元素及其数据作为 http消息的尸体内容发送给 web 服务器，传送的数据量要比使用 GET 方式传送的数据量要大很多。

### 如何在 Servlet 中获取请求信息

1. **Servlet 的 service() 方法用于应答请求**：因为每次请求都会调用该方法。service 方法有两个参数：（这两个接口的实现类都是服务器实现的，并在服务器调用 service 方法时传入）
  - ServletRequest：封装了请求信息，可以从中获取任何的请求信息。
  - ServletResponse：封装了响应信息，使用该接口的方法实现给用户具体的响应。

2. **ServletRequest**：

  - **获取请求参数**：

    - String **getParameter(String name)**：根据请求参数的名字，返回参数值。若请求参数有多个值，如 checkbox，该方法只能获取第一个值。
    - String[] **getParameterValues(String name)**：根据请求参数的名字，返回请求参数对应的字符串数组。
    - Map **getParameterMap()**：返回请求参数的健值对：key：参数名，value：参数值，String数组类型。
    - Enumeration **getParameterNames()**：返回参数名对应的 Enumeration 对象，类似于 ServletConfig（或 ServletContext）的getInitParameterNames() 。

  - 和 attribute 相关的几个方法

3. **HttpServletRequest**：是 ServletRequest 的子接口，是针对 HTTP 请求所定义的，其中包含了大量获取 HTTP 请求相关的方法。(将 servletRequest 强转成 HttpServletServletRequest，``HttpServletRequest hsr = (HttpServletRequest) request;`` )

  - **getRequestURI()**：获取请求的URL.

  - **getMethod()**：获取请求的方式.

  - **getQueryString()**：若是一个 GET 请求，获取请求参数对应的那个字符串，即 ？后面的字符串。

  - **getServletPath()**：获取请求的 Servlet 的映射路径.

4. **ServletResponse**：

  - **getWriter()**：返回PrintWriter 对象，调用该对象的 print() 方法，将把 print() 中的参数直接打印到浏览器上。

  - **setContentType(String Type)**：设置响应的内容类型.

  - **sendRedirect(String location)**:请求的重定向。（此方法为 HttpServletResponse 中定义，需进行强转）





### HttpServletRequest 




### HttpServletResponse



## JSP

> JSP 和 JavaScript 的区别：
>
> * JSP 技术是 WEB 网站的服务端技术，可以简单理解为 JSP 技术说是用来生成动态网页的。普通的网页是 HTML 的，它是静态的，需要事先用 HTML 语言编写好。那么我们在 HTML 页面中加入一些 Java 代码，用 Java 代码部分动态的内容插入到原来的 HTML 页面中，那么，这个页面就可以成为 JSP 页面。即，JSP = HTML + Java。
> * JavaScript 是 WEB 的客户端技术，它是一种脚本语言，不用编译，由浏览器解释执行。它也是插入在 HTML 页面当中。JavaScript 脚本的执行都是事件驱动的，当浏览器加载完 HTML 页面之后，用户点击页面中的按钮或者文本框的时候，如果页面中为这些按钮或文本框写好了响应事件 JavaScript 的脚本，那么用户在做响应动作时就会触发这些脚本的执行。JavaScript 脚本可以不与服务端进行通信，就对客户的动作作出响应。



JSP 和 Servlet 不同之处：

* Servlet 在 Java 代码中通过 HttpServletResponse 对象动态输出 HTML 内容。
* JSP 在静态 HTML 内容中嵌入 Java 代码，Java 代码被动态执行后生成 HTML 内容。
* jsp 就是在 html 里面写 java 代码，servlet 就是在 java 里面写 html 代码…其实 jsp 经过容器解释之后就是 servlet。


