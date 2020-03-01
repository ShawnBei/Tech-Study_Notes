JavaWeb

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

## 5. Servlet

> **Servlet 是什么**：
>
> * Servlet 是一个 Java 程序（组件），运行在我们的 web 服务器上（服务器负责Sevelet和客户的通信以及调用Servlet的方法），Servlet和客户的通信采用“请求/响应”的模式。 

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



### Servlet 生命周期方法

* **init 方法**: 创建该 Servlet 实例时调用，一个 servlet 只会初始化一次。
* **service 方法**：重要客户端来了一个请求，就执行该方法。一个请求对应一个 service 方法。
* **destroy 方法**：servlet 被销毁时执行。
  - 该 servlet 从 Tomcat 中移除
  - 正常关闭 Tomcat 服务器



Servlet 通用写法

* Servlet --> GenericServlet --> HttpServlet(用于处理http	请求)
* 一般继承 HttpServlet, 并重写 doPost 和 doGet 方法。



让 init 方法提前执行（在访问该 servlet 之前创建实例）

* ``<load-on-startup>``中的数字越小，启动时机越早。

````
<servlet>
  	<servlet-name>HelloServlet04</servlet-name>
  	<servlet-class>com.itheima.servlet.HelloServlet04</servlet-class>
  	<load-on-startup>2</load-on-startup>
</servlet>
````



ServletConfig

* 在使用别人开发的 servlet 时，需要自己配置 web.xml（注册该 servlet），并写入初始化时的参数。
* 在开发 servlet 时，使用 ServletConfig 来获取用户写入的初始化参数。


### 5.1 HttpServletRequest 

1. Servlet 路径配置

    

2. Servlet Context （Servlet 上下文）

   > 每个 JVM 中的 每个 Web 工程都只有一个 ServletContext 对象，不管在哪个 Servlet 中获取都是同一个对象（指向同一个类型地址）。

   * 获取对象：``ServletContext context = getServletContext();``
   * 用来获取全局变量，任何 servlet 都可以获取 --> 对应 ServletConfig 获取单个 servlet 变量
   * 作用：
     * 获取全局配置参数
     * 获取web工程中的资源
     * 存取数据，servlet间共享数据  域对象
   * 



### 5.2 HttpServletResponse



## 6. JSP

> JSP 和 JavaScript 的区别：
>
> * JSP 技术是 WEB 网站的服务端技术，可以简单理解为 JSP 技术说是用来生成动态网页的。普通的网页是 HTML 的，它是静态的，需要事先用 HTML 语言编写好。那么我们在 HTML 页面中加入一些 Java 代码，用 Java 代码部分动态的内容插入到原来的 HTML 页面中，那么，这个页面就可以成为 JSP 页面。即，JSP = HTML + Java。
> * JavaScript 是 WEB 的客户端技术，它是一种脚本语言，不用编译，由浏览器解释执行。它也是插入在 HTML 页面当中。JavaScript 脚本的执行都是事件驱动的，当浏览器加载完 HTML 页面之后，用户点击页面中的按钮或者文本框的时候，如果页面中为这些按钮或文本框写好了响应事件 JavaScript 的脚本，那么用户在做响应动作时就会触发这些脚本的执行。JavaScript 脚本可以不与服务端进行通信，就对客户的动作作出响应。



JSP 和 Servlet 不同之处：

* Servlet 在 Java 代码中通过 HttpServletResponse 对象动态输出 HTML 内容。
* JSP 在静态 HTML 内容中嵌入 Java 代码，Java 代码被动态执行后生成 HTML 内容。
* jsp 就是在 html 里面写 java 代码，servlet 就是在 java 里面写 html 代码…其实 jsp 经过容器解释之后就是 servlet。