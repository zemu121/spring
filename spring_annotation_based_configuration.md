#Spring——基于注解的配置

从 Spring 2.5 开始就可以使用**注解**来配置依赖注入。而不是采用 XML 来描述一个 bean 连线，你可以使用相关类，方法或字段声明的注解，将 bean 配置移动到组件类本身。

在 XML 注入之前进行注解注入，因此后者的配置将通过两种方式的属性连线被前者重写。

注解连线在默认情况下在 Spring 容器中不打开。因此，在可以使用基于注解的连线之前，我们将需要在我们的 Spring 配置文件中启用它。所以如果你想在 Spring 应用程序中使用的任何注解，可以考虑到下面的配置文件。

<pre class="prettyprint notranslate">
&lt;?xml version="1.0" encoding="UTF-8"?&gt;

&lt;beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context-3.0.xsd"&gt;

   &lt;context:annotation-config/&gt;
   &lt;!-- bean definitions go here --&gt;

&lt;/beans&gt;
</pre> 


一旦 <context:annotation-config/> 被配置后，你就可以开始注解你的代码，表明 Spring 应该自动连接值到属性，方法和构造函数。让我们来看看几个重要的注解，并且了解它们是如何工作的：

<table class="table table-bordered">
<tr><th class="fivepct">序号</th><th>注解 &amp; 描述</th></tr>
<tr><td>1</td><td><a href="/spring/spring_required_annotation.htm">@Required</a>
<p>@Required 注解应用于 bean 属性的 setter 方法。</p></td></tr>
<tr><td>2</td><td><a href="/spring/spring_autowired_annotation.htm">@Autowired</a>
<p>@Autowired 注解可以应用到 bean 属性的 setter 方法，非 setter 方法，构造函数和属性。</p></td></tr>
<tr><td>3</td><td><a href="/spring/spring_qualifier_annotation.htm">@Qualifier</a>
<p>通过指定确切的将被连线的 bean，@Autowired 和 @Qualifier 注解可以用来删除混乱。</p></td></tr>
<tr><td>4</td><td><a href="/spring/spring_jsr250_annotations.htm">JSR-250 Annotations</a>
<p>Spring 支持 JSR-250 的基础的注解，其中包括了 @Resource，@PostConstruct 和 @PreDestroy 注解。</p></td></tr>
</table>





