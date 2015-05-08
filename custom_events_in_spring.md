# Spring 中的自定义事件

编写和发布自己的自定义事件有许多步骤。按照在这一章给出的说明来编写，发布和处理自定义 Spring 事件。

<table class="table table-bordered">
<tr><th class="fivepct">步骤</th><th>描述</th></tr>
<tr><td>1</td><td>创建一个名称为 <i>SpringExample</i> 的项目，并且在创建项目的 <b>src</b> 文件夹中创建一个包 <i>com.tutorialspoint</i>。</td></tr>
<tr><td>2</td><td>使用 <i>Add External JARs</i> 选项，添加所需的 Spring 库，解释见 <i>Spring Hello World Example</i> 章节。</td></tr>
<tr><td>3</td><td>通过扩展 <b>ApplicationEvent</b>,创建一个事件类 <i>CustomEvent</i>。这个类必须定义一个默认的构造函数，它应该从 ApplicationEvent 类中继承的构造函数。</td></tr>
<tr><td>4</td><td>一旦定义事件类，你可以从任何类中发布它，假定 <i>EventClassPublisher</i> 实现了 <i>ApplicationEventPublisherAware</i>。你还需要在 XML 配置文件中声明这个类作为一个 bean，之所以容器可以识别 bean 作为事件发布者，是因为它实现了 ApplicationEventPublisherAware 接口。</td></tr>
<tr><td>5</td><td>发布的事件可以在一个类中被处理，假定 <i>EventClassHandler</i> 实现了 <i>ApplicationListener</i> 接口，而且实现了自定义事件的 <i>onApplicationEvent</i> 方法。</td></tr>
<tr><td>6</td><td>在 <b>src</b> 文件夹中创建 bean 的配置文件 <i>Beans.xml</i> 和 <i>MainApp</i> 类，它可以作为一个 Spring 应用程序来运行。</td></tr>
<tr><td>7</td><td>最后一步是创建的所有 Java 文件和 Bean 配置文件的内容，并运行应用程序，解释如下所示。</td></tr>
</table>


这个是 **CustomEvent.java** 文件的内容：

``` 
package com.tutorialspoint;
import org.springframework.context.ApplicationEvent;
public class CustomEvent extends ApplicationEvent{ 
   public CustomEvent(Object source) {
      super(source);
   }
   public String toString(){
      return "My Custom Event";
   }
}
```

下面是 **CustomEventPublisher.java** 文件的内容：

``` 
package com.tutorialspoint;
import org.springframework.context.ApplicationEventPublisher;
import org.springframework.context.ApplicationEventPublisherAware;
public class CustomEventPublisher 
   implements ApplicationEventPublisherAware {
   private ApplicationEventPublisher publisher;
   public void setApplicationEventPublisher
              (ApplicationEventPublisher publisher){
      this.publisher = publisher;
   }
   public void publish() {
      CustomEvent ce = new CustomEvent(this);
      publisher.publishEvent(ce);
   }
}
```

下面是 **CustomEventHandler.java** 文件的内容：

``` 
package com.tutorialspoint;
import org.springframework.context.ApplicationListener;
public class CustomEventHandler 
   implements ApplicationListener<CustomEvent>{
   public void onApplicationEvent(CustomEvent event) {
      System.out.println(event.toString());
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
      CustomEventPublisher cvp = 
      (CustomEventPublisher) context.getBean("customEventPublisher");
      cvp.publish();  
      cvp.publish();
   }
}
```

下面是配置文件 **Beans.xml**：

<pre class="prettyprint notranslate">
&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd"&gt;

   &lt;bean id="customEventHandler" 
      class="com.tutorialspoint.CustomEventHandler"/&gt;

   &lt;bean id="customEventPublisher" 
      class="com.tutorialspoint.CustomEventPublisher"/&gt;

&lt;/beans&gt;
</pre> 


一旦你完成了创建源和 bean 的配置文件后，我们就可以运行该应用程序。如果你的应用程序一切都正常，将输出以下信息：

<pre class="result notranslate">
My Custom Event
My Custom Event
</pre>
