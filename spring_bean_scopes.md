# Spring——Bean 的作用域 

当在 Spring 中定义一个 <bean> 时，你必须声明该 bean 的作用域的选项。例如，为了强制 Spring 在每次需要时都产生一个新的 bean 实例，你应该声明 bean 的作用域的属性为 **prototype**。同理，如果你想让 Spring 在每次需要时都返回同一个bean实例，你应该声明 bean 的作用域的属性为 **singleton**。 

Spring 框架支持以下五个作用域，如果你使用 web-aware ApplicationContext 时，其中三个是可用的。 

<table class="table table-bordered">
<tr><th class="thirtypct">作用域</th><th>描述</th></tr>
<tr><td>singleton</td><td>该作用域将 bean 的定义的限制在每一个 Spring IoC 容器中的一个单一实例(默认)。</td></tr>
<tr><td>prototype</td><td>该作用域将单一 bean 的定义限制在任意数量的对象实例。</td></tr>
<tr><td>request</td><td>该作用域将 bean 的定义限制为 HTTP 请求。只在 web-aware Spring ApplicationContext 的上下文中有效。</td></tr> 
<tr><td>session</td><td>该作用域将 bean 的定义限制为 HTTP 会话。 只在web-aware Spring ApplicationContext的上下文中有效。</td></tr>
<tr><td style="width:19%;">global-session</td><td>该作用域将 bean 的定义限制为全局 HTTP 会话。只在 web-aware Spring ApplicationContext 的上下文中有效。</td></tr>
</table>


本章将讨论前两个范围，当我们将讨论有关 web-aware Spring ApplicationContext 时，其余三个将被讨论。

## singleton 作用域： 

如果作用域设置为 singleton，那么 Spring IoC 容器刚好创建一个由该 bean 定义的对象的实例。该单一实例将存储在这种单例 bean 的高速缓存中，以及针对该 bean 的所有后续的请求和引用都返回缓存对象。 

默认作用域是始终是 singleton，但是当仅仅需要 bean 的一个实例时，你可以在 bean 的配置文件中设置作用域的属性为 singleton，如下所示：

<pre class="prettyprint notranslate">
&lt;!-- A bean definition with singleton scope --&gt;
&lt;bean id="..." class="..." scope="singleton"&gt;
    &lt;!-- collaborators and configuration for this bean go here --&gt;
&lt;/bean&gt;
</pre> 


## 例子：

我们在适当的位置使用 Eclipse IDE，然后按照下面的步骤来创建一个 Spring 应用程序： 

<table class="table table-bordered">
<tr><th class="fivepct">步骤</th><th>描述</th></tr>
<tr><td>1</td><td>创建一个名称为 <i>SpringExample</i> 的项目，并且在创建项目的 <b>src</b> 文件夹中创建一个包 <i>com.tutorialspoint</i>。 </td></tr>
<tr><td>2</td><td>使用 <i>Add External JARs</i> 选项，添加所需的 Spring 库，在 <i>Spring Hello World Example</i> 章节解释。 </td></tr>
<tr><td>3</td><td>在 <i>com.tutorialspoint</i> 包中创建 Java 类 <i>HelloWorld</i> 和 <i>MainApp</i>。</td></tr>
<tr><td>4</td><td>在 <b>src</b> 文件夹中创建 Beans 配置文件 <i>Beans.xml</i>。</td></tr>
<tr><td>5</td><td>最后一步是创建的所有 Java 文件和 Bean 配置文件的内容，并运行应用程序，解释如下。</td></tr>
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

下面是 MainApp.java 文件的内容：

``` 
package com.tutorialspoint;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class MainApp {
   public static void main(String[] args) {
      ApplicationContext context = new ClassPathXmlApplicationContext("Beans.xml");
      HelloWorld objA = (HelloWorld) context.getBean("helloWorld");
      objA.setMessage("I'm object A");
      objA.getMessage();
      HelloWorld objB = (HelloWorld) context.getBean("helloWorld");
      objB.getMessage();
   }
}
```

下面是 singleton 作用域必需的配置文件 **Beans.xml**：

<pre class="prettyprint notranslate">
&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd"&gt;

   &lt;bean id="helloWorld" class="com.tutorialspoint.HelloWorld" 
      scope="singleton"&gt;
   &lt;/bean&gt;

&lt;/beans&gt;
</pre> 


一旦你创建源代码和 bean 配置文件完成后，我们就可以运行该应用程序。如果你的应用程序一切都正常，将输出以下信息：

<pre class="result notranslate">
Your Message : I&apos;m object A
Your Message : I&apos;m object A
</pre> 


## prototype 作用域：

如果作用域设置为 prototype，那么每次特定的 bean 发出请求时 Spring IoC 容器就创建对象的新的 Bean 实例。一般说来，满状态的 bean 使用 prototype 作用域和没有状态的 bean 使用 singleton 作用域。 

为了定义 prototype 作用域，你可以在 bean 的配置文件中设置作用域的属性为 prototype，如下所示：

<pre class="prettyprint notranslate">
&lt;!-- A bean definition with singleton scope --&gt;
&lt;bean id="..." class="..." scope="prototype"&gt;
   &lt;!-- collaborators and configuration for this bean go here --&gt;
&lt;/bean&gt;
</pre> 


## 例子： 

我们在适当的位置使用 Eclipse IDE，然后按照下面的步骤来创建一个 Spring 应用程序： 

<table class="table table-bordered">
<tr><th class="fivepct">步骤</th><th>描述</th></tr>
<tr><td>1</td><td>创建一个名称为 <i>SpringExample</i> 的项目，并且在创建项目的 <b>src</b> 文件夹中创建一个包<i>com.tutorialspoint</i>。</td></tr>
<tr><td>2</td><td>使用 <i>Add External JARs</i> 选项，添加所需的 Spring 库，解释见 <i>Spring Hello World Example</i> 章节。  </td></tr>
<tr><td>3</td><td>在 <i>com.tutorialspoint</i> 包中创建 Java 类 <i>HelloWorld</i> 和 <i>MainApp</i>。</td></tr>
<tr><td>4</td><td>在 <b>src</b> 文件夹中创建 Beans 配置文件<i>Beans.xml</i>。  </td></tr>
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

下面是 **MainApp.java** 文件的内容：

``` 
package com.tutorialspoint;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class MainApp {
   public static void main(String[] args) {
      ApplicationContext context = new ClassPathXmlApplicationContext("Beans.xml");
      HelloWorld objA = (HelloWorld) context.getBean("helloWorld");
      objA.setMessage("I'm object A");
      objA.getMessage();
      HelloWorld objB = (HelloWorld) context.getBean("helloWorld");
      objB.getMessage();
   }
}
```

下面是 **prototype** 作用域必需的配置文件 Beans.xml：

<pre class="prettyprint notranslate">
&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd"&gt;

   &lt;bean id="helloWorld" class="com.tutorialspoint.HelloWorld" 
      scope="prototype"&gt;
   &lt;/bean&gt;

&lt;/beans&gt;
</pre> 

一旦你创建源代码和 Bean 配置文件完成后，我们就可以运行该应用程序。如果你的应用程序一切都正常，将输出以下信息：

<pre class="result notranslate">
Your Message : I&apos;m object A
Your Message : null
</pre> 
