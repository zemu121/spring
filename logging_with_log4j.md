# Spring——使用 Log4J 记录日志

在 Spring 应用程序中使用 Log4J 的功能是非常容易的。下面的例子将带你通过简单的步骤解释 Log4J 和 Spring 之间的简单集成。

假设你已经在你的机器上安装了 **Log4J**，如果你还没有 Log4J，你可以从 [**http://logging.apache.org/**](http://logging.apache.org/) 中下载，并且仅仅在任何文件夹中提取压缩文件。在我们的项目中，我们将只使用 **log4j-x.y.z.jar**。

接下来，我们让 Eclipse IDE 在恰当的位置工作，遵循以下步骤，使用 Spring Web 框架开发一个基于 Web 应用程序的动态表单：

<table class="table table-bordered">
<tr><th class="fivepct">步骤</th><th>描述</th></tr>
<tr><td>1</td><td>创建一个名称为 <i>SpringExample</i> 的项目，并且在创建项目的 <b>src</b> 文件夹中创建一个包 <i>com.tutorialspoint</i>。  </td></tr>
<tr><td>2</td><td>使用 <i>Add External JARs</i> 选项，添加所需的 Spring 库，解释见 <i>Spring Hello World Example</i> 章节。</td></tr>
<tr><td>3</td><td>使用 <i>Add External JARs</i> 选项，同样在你的项目中添加 log4j 库 <i>log4j-x.y.z.jar</i>。</td></tr>
<tr><td>4</td><td>在 <i>com.tutorialspoint</i> 包下创建 Java 类 <i>HelloWorld</i> 和 <i>MainApp</i>。</td></tr>
<tr><td>5</td><td>在 <b>src</b> 文件中创建 Bean 配置文件 <i>Beans.xml</i>。</td></tr>
<tr><td>6</td><td>在 <b>src</b> 文件中创建 log4J 配置文件 <i>log4j.properties</i>。</td></tr>
<tr><td>7</td><td>最后一步是创建的所有 Java 文件和 Bean 配置文件的内容，并运行应用程序，解释如下所示。</td></tr>
</table>


这个是 **HelloWorld.java** 文件的内容：

``` 
package com.tutorialspoint;
public class HelloWorld {
   private String message;
   public void setMessage(String message){
      this.message  = message;
   }
   public void getMessage(){
      System.out.println("Your Message : " + message);
   }
}
```

下面的是第二个文件 **MainApp.java** 的内容：

``` 
package com.tutorialspoint;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.apache.log4j.Logger;
public class MainApp {
   static Logger log = Logger.getLogger(MainApp.class.getName());
   public static void main(String[] args) {
      ApplicationContext context = 
             new ClassPathXmlApplicationContext("Beans.xml");
      log.info("Going to create HelloWord Obj");
      HelloWorld obj = (HelloWorld) context.getBean("helloWorld");
      obj.getMessage();
      log.info("Exiting the program");
   }
}
```

使用与我们已经生成信息消息类似的方法，你可以生成**调试**和**错误**消息。现在让我们看看 **Beans.xml** 文件的内容：

<pre class="prettyprint notranslate">
&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd"&gt;

   &lt;bean id="helloWorld" class="com.tutorialspoint.HelloWorld"&gt;
       &lt;property name="message" value="Hello World!"/&gt;
   &lt;/bean&gt;

&lt;/beans&gt;
</pre>


下面是 **log4j.properties** 的内容，它定义了使用 Log4J 生成日志信息所需的标准规则：

<pre class="prettyprint notranslate">
# Define the root logger with appender file
log4j.rootLogger = DEBUG, FILE

# Define the file appender
log4j.appender.FILE=org.apache.log4j.FileAppender
# Set the name of the file
log4j.appender.FILE.File=C:\\log.out

# Set the immediate flush to true (default)
log4j.appender.FILE.ImmediateFlush=true

# Set the threshold to debug mode
log4j.appender.FILE.Threshold=debug

# Set the append to false, overwrite
log4j.appender.FILE.Append=false

# Define the layout for file appender
log4j.appender.FILE.layout=org.apache.log4j.PatternLayout
log4j.appender.FILE.layout.conversionPattern=%m%n
</pre>


一旦你完成了创建源和 bean 的配置文件后，我们就可以运行该应用程序。如果你的应用程序一切都正常，在 Eclipse 控制台将输出以下信息：

<pre class="result notranslate">
Your Message : Hello World!
</pre>


同时如果你检查你的 C:\\ 驱动，那么你应该发现含有各种日志消息的日志文件 **log.out**，其中一些如下所示： 

<pre class="prettyprint notranslate">
&lt;!-- initialization log messages --&gt;

Going to create HelloWord Obj
Returning cached instance of singleton bean 'helloWorld'
Exiting the program
</pre>


## Jakarta Commons Logging (JCL) API

或者，你可以使用 **Jakarta Commons Logging(JCL)** API 在你的 Spring 应用程序中生成日志。JCL 可以从 [**http://jakarta.apache.org/commons/logging/**](http://jakarta.apache.org/commons/logging/) 下载。我们在技术上需要这个包的唯一文件是 *commons-logging-x.y.z.jar* 文件，需要使用与上面的例子中你使用 *log4j-x.y.z.jar* 类似的方法来把 *commons-logging-x.y.z.jar* 放在你的类路径中。

为了使用日志功能，你需要一个 *org.apache.commons.logging.Log* 对象，然后你可以根据你的需要调用任何一个下面的方法：

- fatal(Object message)

- error(Object message)

- warn(Object message)

- info(Object message)

- debug(Object message)

- trace(Object message)
 
下面是使用 JCL API 对 MainApp.java 的替换：

``` 
package com.tutorialspoint;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
import org.apache.commons.logging. Log;
import org.apache.commons.logging. LogFactory;
public class MainApp {
   static Log log = LogFactory.getLog(MainApp.class.getName());
   public static void main(String[] args) {
      ApplicationContext context = 
             new ClassPathXmlApplicationContext("Beans.xml");
      log.info("Going to create HelloWord Obj");
      HelloWorld obj = (HelloWorld) context.getBean("helloWorld");
      obj.getMessage();
      log.info("Exiting the program");
   }
}
```

你应该确保在编译和运行该程序之前在你的项目中已经引入了 *commons-logging-x.y.z.jar* 文件。

现在保持在上面的例子中剩下的配置和内容不变，如果你编译并运行你的应用程序，你就会得到与使用 Log4J API 后获得的结果类似的结果。
