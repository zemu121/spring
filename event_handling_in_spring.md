# Spring 中的事件处理

你已经看到了在所有章节中 Spring 的核心是 **ApplicationContext**，它负责管理 beans 的完整生命周期。当加载 beans 时，ApplicationContext 发布某些类型的事件。例如，当上下文启动时，*ContextStartedEvent* 发布，当上下文停止时，*ContextStoppedEvent* 发布。

通过 *ApplicationEvent* 类和 *ApplicationListener* 接口来提供在 *ApplicationContext* 中处理事件。如果一个 bean 实现 *ApplicationListener*，那么每次 *ApplicationEvent* 被发布到 ApplicationContext 上，那个 bean 会被通知。

Spring 提供了以下的标准事件：

<table class="table table-bordered">
<tr><th class="fivepct">序号</th><th>Spring 内置事件 &amp; 描述</th></tr>
<tr><td>1</td><td><p><b>ContextRefreshedEvent</b></p>
<p> <i>ApplicationContext</i> 被初始化或刷新时，该事件被发布。这也可以在 <i>ConfigurableApplicationContext</i> 接口中使用 refresh() 方法来发生。 </p></td></tr>
<tr><td>2</td><td><p><b>ContextStartedEvent</b></p>
<p> 当使用 <i>ConfigurableApplicationContext</i> 接口中的 start() 方法启动 <i>ApplicationContext</i> 时，该事件被发布。你可以调查你的数据库，或者你可以在接受到这个事件后重启任何停止的应用程序。</p></td></tr>
<tr><td>3</td><td><p><b>ContextStoppedEvent</b></p>
<p> 当使用 <i>ConfigurableApplicationContext</i> 接口中的 stop() 方法停止 <i>ApplicationContext</i> 时，发布这个事件。你可以在接受到这个事件后做必要的清理的工作。</p></td></tr>
<tr><td>4</td><td><p><b>ContextClosedEvent</b></p>
<p> 当使用 <i>ConfigurableApplicationContext</i> 接口中的 close() 方法关闭 <i>ApplicationContext</i> 时，该事件被发布。一个已关闭的上下文到达生命周期末端；它不能被刷新或重启。</p></td></tr>
<tr><td>5</td><td><p><b>RequestHandledEvent</b></p>
<p>这是一个 web-specific 事件，告诉所有 bean HTTP 请求已经被服务。</p></td></tr>
</table>


由于 Spring 的事件处理是单线程的，所以如果一个事件被发布，直至并且除非所有的接收者得到的该消息，该进程被阻塞并且流程将不会继续。因此，如果事件处理被使用，在设计应用程序时应注意。

## 监听上下文事件：

为了监听上下文事件，一个 bean 应该实现只有一个方法 **onApplicationEvent()** 的 *ApplicationListener* 接口。因此，我们写一个例子来看看事件是如何传播的，以及如何可以用代码来执行基于某些事件所需的任务。

让我们在恰当的位置使用 Eclipse IDE，然后按照下面的步骤来创建一个 Spring 应用程序：

<table class="table table-bordered">
<tr><th class="fivepct">步骤</th><th>描述</th></tr>
<tr><td>1</td><td>创建一个名称为 <i>SpringExample</i> 的项目，并且在创建项目的 <b>src</b> 文件夹中创建一个包 <i>com.tutorialspoint</i>。</td></tr>
<tr><td>2</td><td>使用 <i>Add External JARs</i> 选项，添加所需的 Spring 库，解释见 <i>Spring Hello World Example</i> 章节。</td></tr>
<tr><td>3</td><td>在 <i>com.tutorialspoint</i> 包中创建 Java 类 <i>HelloWorld</i>、<i>CStartEventHandler</i>、<i>CStopEventHandler</i> 和 <i>MainApp</i>。</td></tr>
<tr><td>4</td><td>在 <b>src</b> 文件夹中创建 Bean 的配置文件 <i>Beans.xml</i>。</td></tr>
<tr><td>5</td><td>最后一步是创建的所有 Java 文件和 Bean 配置文件的内容，并运行应用程序，解释如下所示。</td></tr>
</table>

	
这里是 **HelloWorld.java** 文件的内容：

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

下面是 **CStartEventHandler.java** 文件的内容：

``` 
package com.tutorialspoint;
import org.springframework.context.ApplicationListener;
import org.springframework.context.event.ContextStartedEvent;
public class CStartEventHandler 
   implements ApplicationListener<ContextStartedEvent>{
   public void onApplicationEvent(ContextStartedEvent event) {
      System.out.println("ContextStartedEvent Received");
   }
}
```

下面是 **CStopEventHandler.java** 文件的内容：

``` 
package com.tutorialspoint;
import org.springframework.context.ApplicationListener;
import org.springframework.context.event.ContextStoppedEvent;
public class CStopEventHandler 
   implements ApplicationListener<ContextStoppedEvent>{
   public void onApplicationEvent(ContextStoppedEvent event) {
      System.out.println("ContextStoppedEvent Received");
   }
}
```

下面是 **MainApp.java** 文件的内容：

``` 
package com.tutorialspoint;

import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MainApp {
   public static void main(String[] args) {
      ConfigurableApplicationContext context = 
      new ClassPathXmlApplicationContext("Beans.xml");

      // Let us raise a start event.
      context.start();
	  
      HelloWorld obj = (HelloWorld) context.getBean("helloWorld");

      obj.getMessage();

      // Let us raise a stop event.
      context.stop();
   }
}
```

下面是配置文件 **Beans.xml** 文件：

<pre class="prettyprint notranslate">
&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd"&gt;

   &lt;bean id="helloWorld" class="com.tutorialspoint.HelloWorld"&gt;
      &lt;property name="message" value="Hello World!"/&gt;
   &lt;/bean&gt;

   &lt;bean id="cStartEventHandler" 
         class="com.tutorialspoint.CStartEventHandler"/&gt;

   &lt;bean id="cStopEventHandler" 
         class="com.tutorialspoint.CStopEventHandler"/&gt;

&lt;/beans&gt;
</pre> 


一旦你完成了创建源和 bean 的配置文件，我们就可以运行该应用程序。如果你的应用程序一切都正常，将输出以下消息：

<pre class="result notranslate">
ContextStartedEvent Received
Your Message : Hello World!
ContextStoppedEvent Received
</pre>


如果你喜欢，可以发布自己的自定义事件，然后你同样可以捕捉处理那些自定义事件的动作。如果你对编写自己的自定义事件感兴趣，可以查看 [**Custom Events in Spring**](http://www.tutorialspoint.com/spring/custom_events_in_spring.htm)
