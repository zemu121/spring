# Spring——MVC 框架教程

Spring web MVC 框架提供了模型-视图-控制的体系结构和可以用来开发灵活、松散耦合的 web 应用程序的组件。MVC 模式导致了应用程序的不同方面(输入逻辑、业务逻辑和UI逻辑)的分离，同时提供了在这些元素之间的松散耦合。

- **模型**封装了应用程序数据，并且通常它们由 POJO 组成。

- **视图**主要用于呈现模型数据，并且通常它生成客户端的浏览器可以解释的 HTML 输出。

- **控制器**主要用于处理用户请求，并且构建合适的模型并将其传递到视图呈现。

## DispatcherServlet

Spring Web 模型-视图-控制（MVC）框架是围绕 *DispatcherServlet* 设计的，*DispatcherServlet* 用来处理所有的 HTTP 请求和响应。Spring Web MVC *DispatcherServlet* 的请求处理的工作流程如下图所示：

![](images/mvc1.png)

下面是对应于 *DispatcherServlet* 传入 HTTP 请求的事件序列：

- 收到一个 HTTP 请求后，*DispatcherServlet* 根据 *HandlerMapping* 来选择并且调用适当的*控制器*。

- *控制器*接受请求，并基于使用的 GET 或 POST 方法来调用适当的 service 方法。Service 方法将设置基于定义的业务逻辑的模型数据，并返回视图名称到 *DispatcherServlet* 中。

- *DispatcherServlet* 会从 *ViewResolver* 获取帮助，为请求检取定义视图。

- 一旦确定视图，*DispatcherServlet* 将把模型数据传递给视图，最后呈现在浏览器中。

上面所提到的所有组件，即 HandlerMapping、Controller 和 ViewResolver 是 *WebApplicationContext* 的一部分，而 *WebApplicationContext* 是带有一些对 web 应用程序必要的额外特性的 *ApplicationContext* 的扩展。

## 需求的配置

你需要映射你想让 *DispatcherServlet* 处理的请求，通过使用在 **web.xml** 文件中的一个 URL 映射。下面是一个显示声明和映射 **HelloWeb** *DispatcherServlet* 的示例：

``` 
<web-app id="WebApp_ID" version="2.4"
    xmlns="http://java.sun.com/xml/ns/j2ee" 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee 
    http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">
    <display-name>Spring MVC Application</display-name>
   <servlet>
      <servlet-name>HelloWeb</servlet-name>
      <servlet-class>
         org.springframework.web.servlet.DispatcherServlet
      </servlet-class>
      <load-on-startup>1</load-on-startup>
   </servlet>
   <servlet-mapping>
      <servlet-name>HelloWeb</servlet-name>
      <url-pattern>*.jsp</url-pattern>
   </servlet-mapping>
</web-app>
```

**web.xml** 文件将被保留在你的应用程序的 *WebContent/WEB-INF* 目录下。好的，在初始化 **HelloWeb** *DispatcherServlet* 时，该框架将尝试加载位于该应用程序的 *WebContent/WEB-INF* 目录中文件名为 **[servlet-name]-servlet.xml** 的应用程序内容。在这种情况下，我们的文件将是 **HelloWeb-servlet.xml**。

接下来，<servlet-mapping> 标签表明哪些 URLs 将被 DispatcherServlet 处理。这里所有以 **.jsp** 结束的 HTTP 请求将由 **HelloWeb** DispatcherServle t处理。

如果你不想使用默认文件名 *[servlet-name]-servlet.xml* 和默认位置 *WebContent/WEB-INF*，你可以通过在 web.xml 文件中添加 servlet 监听器 *ContextLoaderListener* 自定义该文件的名称和位置，如下所示：

``` 
<web-app...>
<!-------- DispatcherServlet definition goes here----->
....
<context-param>
   <param-name>contextConfigLocation</param-name>
   <param-value>/WEB-INF/HelloWeb-servlet.xml</param-value>
</context-param>
<listener>
   <listener-class>
      org.springframework.web.context.ContextLoaderListener
   </listener-class>
</listener>
</web-app>
```

现在，检查 **HelloWeb-servlet.xml** 文件的请求配置，该文件位于 web 应用程序的 *WebContent/WEB-INF* 目录下：

<pre class="prettyprint notranslate">
&lt;beans xmlns="http://www.springframework.org/schema/beans"
   xmlns:context="http://www.springframework.org/schema/context"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="
   http://www.springframework.org/schema/beans     
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
   http://www.springframework.org/schema/context 
   http://www.springframework.org/schema/context/spring-context-3.0.xsd"&gt;

   &lt;context:component-scan base-package="com.tutorialspoint" /&gt;

   &lt;bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"&gt;
      &lt;property name="prefix" value="/WEB-INF/jsp/" /&gt;
      &lt;property name="suffix" value=".jsp" /&gt;
   &lt;/bean&gt;

&lt;/beans&gt;
</pre>


以下是关于 **HelloWeb-servlet.xml** 文件的一些要点：

- *[servlet-name]-servlet.xml* 文件将用于创建 bean 定义，重新定义在全局范围内具有相同名称的任何已定义的 bean。

- *<context:component-scan...>* 标签将用于激活 Spring MVC 注释扫描功能，该功能允许使用注释，如 @Controller 和 @RequestMapping 等等。

- *InternalResourceViewResolver* 将使用定义的规则来解决视图名称。按照上述定义的规则，一个名称为 **hello** 的逻辑视图将发送给位于 */WEB-INF/jsp/hello.jsp* 中实现的视图。

下一节将向你展示如何创建实际的组件，例如控制器，模式和视图。

## 定义控制器

DispatcherServlet 发送请求到控制器中执行特定的功能。**@Controller** 注释表明一个特定类是一个控制器的作用。**@RequestMapping** 注释用于映射 URL 到整个类或一个特定的处理方法。

``` 
@Controller
@RequestMapping("/hello")
public class HelloController{
   @RequestMapping(method = RequestMethod.GET)
   public String printHello(ModelMap model) {
      model.addAttribute("message", "Hello Spring MVC Framework!");
      return "hello";
   }
}
```

**@Controller** 注释定义该类作为一个 Spring MVC 控制器。在这里，第一次使用的 **@RequestMapping** 表明在该控制器中处理的所有方法都是相对于 **/hello** 路径的。下一个注释 **@RequestMapping(method = RequestMethod.GET)** 用于声明 *printHello()* 方法作为控制器的默认 service 方法来处理 HTTP GET 请求。你可以在相同的 URL 中定义其他方法来处理任何 POST 请求。

你可以用另一种形式来编写上面的控制器，你可以在 *@RequestMapping* 中添加额外的属性，如下所示：

``` 
@Controller
public class HelloController{
   @RequestMapping(value = "/hello", method = RequestMethod.GET)
   public String printHello(ModelMap model) {
      model.addAttribute("message", "Hello Spring MVC Framework!");
      return "hello";
   }
}
```

**值**属性表明 URL 映射到哪个处理方法，**方法**属性定义了 service 方法来处理 HTTP GET 请求。关于上面定义的控制器，这里有以下几个要注意的要点：

- 你将在一个 service 方法中定义需要的业务逻辑。你可以根据每次需求在这个方法中调用其他方法。

- 基于定义的业务逻辑，你将在这个**方法**中创建一个模型。你可以设置不同的模型属性，这些属性将被视图访问并显示最终的结果。这个示例创建了一个带有属性 “message” 的模型。

- 一个定义的 service 方法可以返回一个包含**视图**名称的字符串用于呈现该模型。这个示例返回 “hello” 作为逻辑视图的名称。

## 创建 JSP 视图

对于不同的表示技术，Spring MVC 支持许多类型的视图。这些包括 JSP、HTML、PDF、Excel 工作表、XML、Velocity 模板、XSLT、JSON、Atom 和 RSS 提要、JasperReports 等等。但我们最常使用利用 JSTL 编写的 JSP 模板。所以让我们在 /WEB-INF/hello/hello.jsp 中编写一个简单的 **hello** 视图：

<pre class="prettyprint notranslate">
&lt;html&gt;
   &lt;head&gt;
   &lt;title&gt;Hello Spring MVC&lt;/title&gt;
   &lt;/head&gt;
   &lt;body&gt;
   &lt;h2&gt;${message}&lt;/h2&gt;
   &lt;/body&gt;
&lt;/html&gt;
</pre>


其中，**${message}** 是我们在控制器内部设置的属性。你可以在你的视图中有多个属性显示。


## Spring Web MVC 框架例子：

基于上述概念，让我们看看一些重要的例子来帮助你建立 Spring Web 应用程序：

<table class="table table-bordered">
<tr><th style="width:5%">序号</th><th>例子 &amp; 描述</th></tr>
<tr><td>1</td><td><a href="/spring/spring_mvc_hello_world_example.htm">Spring MVC Hello World Example</a>
<p>这个例子将解释如何编写一个简单的 Spring Web Hello World 应用程序。</p></td></tr>
<tr><td>2</td><td><a href="/spring/spring_mvc_form_handling_example.htm">Spring MVC Form Handling Example</a>
<p>这个例子将解释如何编写一个 Spring Web 应用程序，它使用 HTML 表单提交数据到控制器，并且显示处理结果。</p></td></tr>
<tr><td>3</td><td><a href="/spring/spring_page_redirection_example.htm">Spring Page Redirection Example</a>
<p>学习在 Spring MVC 框架中如何使用页面重定向功能。</p></td></tr>
<tr><td>4</td><td><a href="/spring/spring_static_pages_example.htm">Spring Static Pages Example</a>
<p>学习在 Spring MVC 框架中如何访问静态页面和动态页面。</p></td></tr>
<tr><td>5</td><td><a href="/spring/spring_exception_handling_example.htm">Spring Exception Handling Example</a>
<p>学习在 Spring MVC 框架中如何处理异常。</p></td></tr>
</table>



