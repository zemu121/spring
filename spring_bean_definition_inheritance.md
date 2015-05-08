# Spring——Bean 定义继承 

bean 定义可以包含很多的配置信息，包括构造函数的参数，属性值，容器的具体信息例如初始化方法，静态工厂方法名，等等。 

子 bean 的定义继承父定义的配置数据。子定义可以根据需要重写一些值，或者添加其他值。 

Spring Bean 定义的继承与 Java 类的继承无关，但是继承的概念是一样的。你可以定义一个父 bean 的定义作为模板和其他子 bean 就可以从父 bean 中继承所需的配置。

当你使用基于 XML 的配置元数据时，通过使用父属性，指定父 bean 作为该属性的值来表明子 bean 的定义。

## 例子： 

我们在适当的位置使用 Eclipse IDE，然后按照下面的步骤来创建一个 Spring 应用程序：

<table class="table table-bordered">
<tr><th class="fivepct">步骤</th><th>描述</th></tr>
<tr><td>1</td><td>创建一个名称为 <i>SpringExample</i> 的项目，并且在创建项目的 <b>src</b> 文件夹中创建一个包 <i>com.tutorialspoint</i>。 </td></tr>
<tr><td>2</td><td>使用 <i>Add External JARs</i> 选项，添加所需的 Spring 库，解释见 <i>Spring Hello World Example</i> 章节。</td></tr>
<tr><td>3</td><td>在 <i>com.tutorialspoint</i> 包中创建 Java 类 <i>HelloWorld</i>、<i>HelloIndia</i> 和 <i>MainApp</i>。 </td></tr>
<tr><td>4</td><td>在 <b>src</b> 文件夹中创建 Beans 配置文件 <i>Beans.xml</i>。  </td></tr>
<tr><td>5</td><td>最后一步是创建的所有 Java 文件和 Bean 配置文件的内容，并运行应用程序，解释如下所示。</td></tr>
</table>

	
下面是配置文件 **Beans.xml**，在该配置文件中我们定义有两个属性 *message1* 和 *message2* 的 “helloWorld” bean。然后，使用 **parent** 属性把 “helloIndia” bean 定义为 “helloWorld” bean 的孩子。这个子 bean 继承 *message2* 的属性，重写 *message1* 的属性，并且引入一个属性 *message3*。

<pre class="prettyprint notranslate">
&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd"&gt;

   &lt;bean id="helloWorld" class="com.tutorialspoint.HelloWorld"&gt;
      &lt;property name="message1" value="Hello World!"/&gt;
      &lt;property name="message2" value="Hello Second World!"/&gt;
   &lt;/bean&gt;

   &lt;bean id="helloIndia" class="com.tutorialspoint.HelloIndia" parent="helloWorld"&gt;
      &lt;property name="message1" value="Hello India!"/&gt;
      &lt;property name="message3" value="Namaste India!"/&gt;
   &lt;/bean&gt;

&lt;/beans&gt;
</pre>

这里是 **HelloWorld.java** 文件的内容：

``` 
package com.tutorialspoint;
public class HelloWorld {
   private String message1;
   private String message2;
   public void setMessage1(String message){
      this.message1  = message;
   }
   public void setMessage2(String message){
      this.message2  = message;
   }
   public void getMessage1(){
      System.out.println("World Message1 : " + message1);
   }
   public void getMessage2(){
      System.out.println("World Message2 : " + message2);
   }
}
```

这里是 **HelloIndia.java** 文件的内容：

``` 
package com.tutorialspoint;

public class HelloIndia {
   private String message1;
   private String message2;
   private String message3;

   public void setMessage1(String message){
      this.message1  = message;
   }

   public void setMessage2(String message){
      this.message2  = message;
   }

   public void setMessage3(String message){
      this.message3  = message;
   }

   public void getMessage1(){
      System.out.println("India Message1 : " + message1);
   }

   public void getMessage2(){
      System.out.println("India Message2 : " + message2);
   }

   public void getMessage3(){
      System.out.println("India Message3 : " + message3);
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

      objA.getMessage1();
      objA.getMessage2();

      HelloIndia objB = (HelloIndia) context.getBean("helloIndia");
      objB.getMessage1();
      objB.getMessage2();
      objB.getMessage3();
   }
}
```

一旦你创建源代码和 bean 配置文件完成后，我们就可以运行该应用程序。如果你的应用程序一切都正常，将输出以下信息：

<pre class="result notranslate">
World Message1 : Hello World!
World Message2 : Hello Second World!
India Message1 : Hello India!
India Message2 : Hello Second World!
India Message3 : Namaste India!
</pre> 


在这里你可以观察到，我们创建 “helloIndia” bean 的同时并没有传递 message2，但是由于 Bean 定义的继承，所以它传递了 message2。

## Bean 定义模板： 

你可以创建一个 Bean 定义模板，不需要花太多功夫它就可以被其他子 bean 定义使用。在定义一个 Bean 定义模板时，你不应该指定**类**的属性，而应该指定带 **true** 值的**抽象**属性，如下所示：

<pre class="prettyprint notranslate">
&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd"&gt;

   &lt;bean id="beanTeamplate" abstract="true"&gt;
      &lt;property name="message1" value="Hello World!"/&gt;
      &lt;property name="message2" value="Hello Second World!"/&gt;
      &lt;property name="message3" value="Namaste India!"/&gt;
   &lt;/bean&gt;

   &lt;bean id="helloIndia" class="com.tutorialspoint.HelloIndia" parent="beanTeamplate"&gt;
      &lt;property name="message1" value="Hello India!"/&gt;
      &lt;property name="message3" value="Namaste India!"/&gt;
   &lt;/bean&gt;

&lt;/beans&gt;
</pre> 

父 bean 自身不能被实例化，因为它是不完整的，而且它也被明确地标记为抽象的。当一个定义是抽象的，它仅仅作为一个纯粹的模板 bean 定义来使用的，充当子定义的父定义使用。

