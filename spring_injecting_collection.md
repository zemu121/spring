# Spring——注入集合

你已经看到了如何使用 **value** 属性来配置基本数据类型和在你的 bean 配置文件中使用 <property> 标签的 **ref** 属性来配置对象引用。这两种情况下处理奇异值传递给一个 bean。

现在如果你想传递多个值，如 Java Collection 类型 List、Set、Map 和 Properties，应该怎么做呢。为了处理这种情况，Spring 提供了四种类型的集合的配置元素，如下所示： 

<table class="table table-bordered">
<tr><th class="twentypct">元素</th><th>描述</th></tr>
<tr><td>&lt;list&gt;</td><td>它有助于连线，如注入一列值，允许重复。</td></tr>
<tr><td>&lt;set&gt;</td><td>它有助于连线一组值，但不能重复。</td></tr>
<tr><td>&lt;map&gt;</td><td>它可以用来注入名称-值对的集合，其中名称和值可以是任何类型。</td></tr>
<tr><td>&lt;props&gt;</td><td>它可以用来注入名称-值对的集合，其中名称和值都是字符串类型。</td></tr>
</table>

	
你可以使用 <list> 或 <set> 来连接任何 java.util.Collection 的实现或数组。

你会遇到两种情况（a）传递集合中直接的值（b）传递一个 bean 的引用作为集合的元素。 

## 例子: 

我们在适当的位置使用 Eclipse IDE，然后按照下面的步骤来创建一个 Spring 应用程序：

<table class="table table-bordered">
<tr><th class="fivepct">步骤</th><th>描述</th></tr>
<tr><td>1</td><td>创建一个名称为 <i>SpringExample</i> 的项目，并且在创建项目的 <b>src</b> 文件夹中创建一个包 <i>com.tutorialspoint</i> 。</td></tr>
<tr><td>2</td><td>使用 <i>Add External JARs</i> 选项，添加所需的 Spring 库，解释见 <i>Spring Hello World Example</i> 章节。  option as explained in the  chapter.</td></tr>
<tr><td>3</td><td>在 <i>com.tutorialspoint</i> 包中创建Java类<i>TextEditor</i>、<i>SpellChecker</i> 和 <i>MainApp</i>。</td></tr>
<tr><td>4</td><td>在 <b>src</b> 文件夹中创建 Beans 配置文件 <i>Beans.xml</i>。 </td></tr>
<tr><td>5</td><td>最后一步是创建的所有Java文件和Bean配置文件的内容，并运行应用程序，解释如下所示。</td></tr>
</table>


这里是 **JavaCollection.java** 文件的内容：

``` 
package com.tutorialspoint;
import java.util.*;
public class JavaCollection {
   List addressList;
   Set  addressSet;
   Map  addressMap;
   Properties addressProp;
   // a setter method to set List
   public void setAddressList(List addressList) {
      this.addressList = addressList;
   }
   // prints and returns all the elements of the list.
   public List getAddressList() {
      System.out.println("List Elements :"  + addressList);
      return addressList;
   }
   // a setter method to set Set
   public void setAddressSet(Set addressSet) {
      this.addressSet = addressSet;
   }
   // prints and returns all the elements of the Set.
   public Set getAddressSet() {
      System.out.println("Set Elements :"  + addressSet);
      return addressSet;
   }
   // a setter method to set Map
   public void setAddressMap(Map addressMap) {
      this.addressMap = addressMap;
   }  
   // prints and returns all the elements of the Map.
   public Map getAddressMap() {
      System.out.println("Map Elements :"  + addressMap);
      return addressMap;
   }
   // a setter method to set Property
   public void setAddressProp(Properties addressProp) {
      this.addressProp = addressProp;
   } 
   // prints and returns all the elements of the Property.
   public Properties getAddressProp() {
      System.out.println("Property Elements :"  + addressProp);
      return addressProp;
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
      ApplicationContext context = 
             new ClassPathXmlApplicationContext("Beans.xml");
      JavaCollection jc=(JavaCollection)context.getBean("javaCollection");
      jc.getAddressList();
      jc.getAddressSet();
      jc.getAddressMap();
      jc.getAddressProp();
   }
}
```

下面是配置所有类型的集合的配置文件 **Beans.xml** 文件：

<pre class="prettyprint notranslate">
&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd"&gt;

   &lt;!-- Definition for javaCollection --&gt;
   &lt;bean id="javaCollection" class="com.tutorialspoint.JavaCollection"&gt;

      &lt;!-- results in a setAddressList(java.util.List) call --&gt;
      &lt;property name="addressList"&gt;
         &lt;list&gt;
            &lt;value&gt;INDIA&lt;/value&gt;
            &lt;value&gt;Pakistan&lt;/value&gt;
            &lt;value&gt;USA&lt;/value&gt;
            &lt;value&gt;USA&lt;/value&gt;
         &lt;/list&gt;
      &lt;/property&gt;

      &lt;!-- results in a setAddressSet(java.util.Set) call --&gt;
      &lt;property name="addressSet"&gt;
         &lt;set&gt;
            &lt;value&gt;INDIA&lt;/value&gt;
            &lt;value&gt;Pakistan&lt;/value&gt;
            &lt;value&gt;USA&lt;/value&gt;
            &lt;value&gt;USA&lt;/value&gt;
        &lt;/set&gt;
      &lt;/property&gt;

      &lt;!-- results in a setAddressMap(java.util.Map) call --&gt;
      &lt;property name="addressMap"&gt;
         &lt;map&gt;
            &lt;entry key="1" value="INDIA"/&gt;
            &lt;entry key="2" value="Pakistan"/&gt;
            &lt;entry key="3" value="USA"/&gt;
            &lt;entry key="4" value="USA"/&gt;
         &lt;/map&gt;
      &lt;/property&gt;
      
      &lt;!-- results in a setAddressProp(java.util.Properties) call --&gt;
      &lt;property name="addressProp"&gt;
         &lt;props&gt;
            &lt;prop key="one"&gt;INDIA&lt;/prop&gt;
            &lt;prop key="two"&gt;Pakistan&lt;/prop&gt;
            &lt;prop key="three"&gt;USA&lt;/prop&gt;
            &lt;prop key="four"&gt;USA&lt;/prop&gt;
         &lt;/props&gt;
      &lt;/property&gt;

   &lt;/bean&gt;

&lt;/beans&gt;
</pre> 

一旦你创建源代码和 bean 配置文件完成后，我们就可以运行该应用程序。你应该注意这里不需要配置文件。如果你的应用程序一切都正常，将输出以下信息：

<pre class="result notranslate">
List Elements :[INDIA, Pakistan, USA, USA]
Set Elements :[INDIA, Pakistan, USA]
Map Elements :{1=INDIA, 2=Pakistan, 3=USA, 4=USA}
Property Elements :{two=Pakistan, one=INDIA, three=USA, four=USA}
</pre> 


## 注入 Bean 引用： 

下面的 Bean 定义将帮助你理解如何注入 bean 的引用作为集合的元素。甚至你可以将引用和值混合在一起，如下所示：

<pre class="prettyprint notranslate">
&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd"&gt;

   &lt;!-- Bean Definition to handle references and values --&gt;
   &lt;bean id="..." class="..."&gt;

      &lt;!-- Passing bean reference  for java.util.List --&gt;
      &lt;property name="addressList"&gt;
         &lt;list&gt;
            &lt;ref bean="address1"/&gt;
            &lt;ref bean="address2"/&gt;
            &lt;value&gt;Pakistan&lt;/value&gt;
         &lt;/list&gt;
      &lt;/property&gt;
      
      &lt;!-- Passing bean reference  for java.util.Set --&gt;
      &lt;property name="addressSet"&gt;
         &lt;set&gt;
            &lt;ref bean="address1"/&gt;
            &lt;ref bean="address2"/&gt;
            &lt;value&gt;Pakistan&lt;/value&gt;
         &lt;/set&gt;
      &lt;/property&gt;
      
      &lt;!-- Passing bean reference  for java.util.Map --&gt;
      &lt;property name="addressMap"&gt;
         &lt;map&gt;
            &lt;entry key="one" value="INDIA"/&gt;
            &lt;entry key ="two" value-ref="address1"/&gt;
            &lt;entry key ="three" value-ref="address2"/&gt;
         &lt;/map&gt;
      &lt;/property&gt;
      
   &lt;/bean&gt;

&lt;/beans&gt;
</pre> 


为了使用上面的 bean 定义，你需要定义 setter 方法，它们应该也能够是用这种方式来处理引用。

## 注入 null 和空字符串的值 

如果你需要传递一个空字符串作为值，那么你可以传递它，如下所示：

<pre class="prettyprint notranslate">
&lt;bean id="..." class="exampleBean"&gt;
   &lt;property name="email" value=""/&gt;
&lt;/bean&gt;
</pre> 


前面的例子相当于 Java 代码：exampleBean.setEmail("")。

如果你需要传递一个 NULL 值，那么你可以传递它，如下所示：

<pre class="prettyprint notranslate">
&lt;bean id="..." class="exampleBean"&gt;
   &lt;property name="email"&gt;&lt;null/&gt;&lt;/property&gt;
&lt;/bean&gt;
</pre> 


前面的例子相当于 Java 代码：exampleBean.setEmail(null)。
