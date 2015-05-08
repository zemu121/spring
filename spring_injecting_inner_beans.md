# Spring——注入内部 Beans 

正如你所知道的 Java 内部类是在其他类的范围内被定义的，同理，**inner beans** 是在其他 bean 的范围内定义的 bean。因此在 <property/> 或 <constructor-arg/> 元素内 <bean/> 元素被称为内部bean，如下所示。

<pre class="prettyprint notranslate">
&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd"&gt;

   &lt;bean id="outerBean" class="..."&gt;
      &lt;property name="target"&gt;
         &lt;bean id="innerBean" class="..."/&gt;
      &lt;/property&gt;
   &lt;/bean&gt;

&lt;/beans&gt;
</pre>

## 例子： 

我们在适当的位置使用 Eclipse IDE，然后按照下面的步骤来创建一个 Spring 应用程序：

<table class="table table-bordered">
<tr><th class="fivepct">步骤</th><th>描述</th></tr>
<tr><td>1</td><td>创建一个名称为 <i>SpringExample</i> 的项目，并且在创建项目的 <b>src</b> 文件夹中创建一个包 <i>com.tutorialspoint</i> 。</td></tr>
<tr><td>2</td><td>使用 <i>Add External JARs</i> 选项，添加所需的 Spring 库，解释见 <i>Spring Hello World Example</i> 章节。  option as explained in the  chapter.</td></tr>
<tr><td>3</td><td>在 <i>com.tutorialspoint</i> 包中创建Java类<i>TextEditor</i>、<i>SpellChecker</i> 和 <i>MainApp</i>。</td></tr>
<tr><td>4</td><td>在 <b>src</b> 文件夹中创建 Beans 配置文件 <i>Beans.xml</i>。 </td></tr>
<tr><td>5</td><td>最后一步是创建的所有Java文件和Bean配置文件的内容，并运行应用程序，解释如下所示。</td></tr>
</table>


这里是 **TextEditor.java** 文件的内容：

``` 
package com.tutorialspoint;
public class TextEditor {
   private SpellChecker spellChecker;
   // a setter method to inject the dependency.
   public void setSpellChecker(SpellChecker spellChecker) {
      System.out.println("Inside setSpellChecker." );
      this.spellChecker = spellChecker;
   }  
   // a getter method to return spellChecker
   public SpellChecker getSpellChecker() {
      return spellChecker;
   }
   public void spellCheck() {
      spellChecker.checkSpelling();
   }
}
```

下面是另一个依赖的类文件 **SpellChecker.java** 内容：

``` 
package com.tutorialspoint;
public class SpellChecker {
   public SpellChecker(){
      System.out.println("Inside SpellChecker constructor." );
   }
   public void checkSpelling(){
      System.out.println("Inside checkSpelling." );
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
      TextEditor te = (TextEditor) context.getBean("textEditor");
      te.spellCheck();
   }
}
```

下面是使用**内部 bean** 为基于 setter 注入进行配置的配置文件 **Beans.xml** 文件：

<pre class="prettyprint notranslate">
&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd"&gt;

   &lt;!-- Definition for textEditor bean using inner bean --&gt;
   &lt;bean id="textEditor" class="com.tutorialspoint.TextEditor"&gt;
      &lt;property name="spellChecker"&gt;
         &lt;bean id="spellChecker" class="com.tutorialspoint.SpellChecker"/&gt;
       &lt;/property&gt;
   &lt;/bean&gt;

&lt;/beans&gt;
</pre>

一旦你创建源代码和 bean 配置文件完成后，我们就可以运行该应用程序。如果你的应用程序一切都正常，将输出以下信息：

<pre class="result notranslate">
Inside SpellChecker constructor.
Inside setSpellChecker.
Inside checkSpelling.
</pre>