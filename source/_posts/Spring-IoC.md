---
title: Spring-IoC 容器
copyright: true
date: 2017-09-06 09:15:29
tags: 
    - Spring
categories:
    - Java框架
    - Spring
    - Ioc
---


### 概念

 - IoC ： Inversion of Control ， 控制反转， 是 Spring 容器的内核， AOP、 声明式事务等功能在此基础上开花结果
  

<!--more-->

     -  某一接口具体实现类的选择控制权从调用类中移除，转交给第三方决定，由Spring容器借由Bean配置来进行控制
     

 - DI  ：  Dependency Injection ， 依赖注入， 此概念用来代替IoC
 
     - 让调用类对某一接口实现类的依赖关系由第三方（ 容器或协作类 ）注入，以移除调用类对某一接口实现类的依赖

### 注入方式：  
  
- 构造函数注入（ constructor-base ）

     通过调用类的构造函数， 将接口实现类通过构造函数变量传入  
```java
    <bean id="date" class="java.util.Date" />

    <!-- constructor -->
        <bean id="customer"  class="io.spring.ioc.di.Customer" >
        <constructor-arg name="id" value="1003" />
        <constructor-arg name="name" value="Leo" />
        <constructor-arg name="gender" value="男" />
        <constructor-arg name="birthdate" ref="date" />
    </bean>

    public Customer(Integer id, String name, char gender, Date birthdate) {
        super();
        this.id = id;   
        this.name = name;
        this.gender = gender;
        this.birthdate = birthdate;
    }
```

- 属性注入（ setter-base ）

    属性注入可以有选择地通过Setter方法完成调用类所需依赖的注入（ 方便灵活 ）
#### Spring Framework 3.0 之前 
    ```java
    <bean id="date" class="java.util.Date" />

    <!-- setter -->
    <bean id="customer" class="io.spring.ioc.di.Customer" >
        <property name="id" value="1002" />
        <property name="name" value="Leo" />
        <property name="gender" value="男" />
        <property name="birthdate" ref="date" />
    </bean>
    ```                                         
#### Spring Framework 3.0 开始支持 p 命名空间 :

   ```java
    <bean id="date" class="java.util.Date" />
    
    <!-- setter -->
    <bean id="customer" 
        class="io.spring.ioc.di.Customer" 
        p:id="1002" 
        p:name="Leo"
        p:gender="男"
        p:birthdate-ref="date" /> 
    ```

- 接口注入（ interface-base ）
 
    将调用类所有依赖注入的方法抽取到一个接口中， 调用类通过实现该接口提供相应的注入方法（ 额外声明接口， 增加类的数目， 效果和属性并无本质区别， 不提倡 ）

###  自动装配（ autowiring ）

###### 将某个Bean实例中所应用的其它的Bean自动注入到当前Bean实例中

1. no ： 默认值， 不启用自动装配，需要显示引用相应的Bean
```java
    <bean id="date" class="java.util.Date"/>
    
    <bean id="student" class="Student">
    <property name="birthdate" ref="date" /> <!-- 显示引用Bean -->
    </bean>
```

2. byName ： 根据属性名称和被引用的Bean的名称来实现自动注入(setter)

    ```java
        <bean id="birthdate" class="java.util.Date"/>
    
        <!-- 假设Student类内部有一个birthdate属性(有getter和setter) -->
        <bean id="student" class="Student" autowire="byName"/>
    ```

3. byType： 根据属性类型和被引用的Bean的类型来实现自动注入(setter)
  
    ```java
        <bean id="d" class="java.util.Date"/>
    
        <!-- 假设Student类内部有一个Date类型的属性(有getter和setter) -->
        <bean id="student" class="Student" autowire="byType"/>
    ```

4. constructor： 根据构造方法的参数类型和被引用的Bean的类型来实现自动注入(constructor)
    
    - 当存在多个同种类型的Bean与构造方法中的参数类型相同时

         a->  如果某个Bean的名称跟构造方法的参数的名称一致， 则根据名称自动装配

         b->  如果所有Bean的名称跟构造方法的参数的名称都不相同， 则不装配，抛出空指针异常， 不抛出NoUniqueBeanDefinitionException异常

    - 当且仅当与构造方法中的参数类型相同的Bean只有一个时， 此时根据类型自动装配

    ```java
        <!-- 不使用构造实现自动装配时的写法 -->
        <bean id="dog" class="Dog" />
        <bean id="p" class="Person">
            <property name="id" value="1111" />
            <property name="name" value="华安" />
            <constructor-arg name="wangcai" ref="dog" /> 
        </bean>
    
        <!-- 使用构造实现自动装配时的写法 -->
        <bean id="dog" class="Dog" />
        <bean id="p" class="Person" autowire="constructor">
            <property name="id" value="1111" />
            <property name="name" value="华安" />
        </bean>
    ```

### 资源加载
###### Spring 支持的资源类型的地址前缀

地址前缀 | 示例 | 对应的资源类型
-----|------|------------
 classpath:| classpath:com/smart/beanfactory/beans.xml| 从类路径中加载资源， classpath: 和 classpath:/是等价的， 都是相对于类的根路径。 资源文件可以在标准的文件系统中， 也可以在 JAR 或 ZIP 的类包中
file:| file:/conf/com/smart/beanfactory/beans.xml| 使用 UrlResource 从文件系统目录中装置资源，可以采用绝对或相对路径
http://| http://www.smart.com/resource/beans.xml | 使用 UrlResource 从Web 服务器中装载资源
ftp://| http://www.smart.com/resource/beans.xml | 使用 UrlResource 从FTP 服务器中装载资源
没有前缀 |com/smart/beanfactory/beans.xml | 根据 ApplicationContext 的具体实现类采用对应类型的Resource

###### 三种通配符的使用

  假设io.spring.ioc.autowiring 包中有个 constructor-autowiring.xml

- （\*）匹配同一级别路径的多个字符：io/\*/ioc/autowiring/constructor-autowiring.xml

- （\*\*）匹配多级路径中的多个字符：io/\*\*/constructor-autowiring.xml

- （？） 仅匹配一个字符

Ant风格资源路径的示例
- classpath:com/t ? st.xml： 匹配 com 类路径下的 com/test.xml、com/tast.xml 或者 com/txst.xml 文件

- file:D:/conf/\*.xml： 匹配文件系统 D:/conf 目录下所有以 .xml 为后缀的文件。

- classpath:/com/\*\*/test.xml： 匹配 com 类路径下（ 当前 目录及其子孙目录 ）的 test.xml 文件。

- classpath:org/springframework/\*\*/\*.xml： 匹配类路径 org/springframework 下所有以 .xml  为后缀的文件。

- classpath:org/\*\*/servlet/servlet/bla.xml： 不仅匹配类路径 org/springframework/servlet/bla.xml， 也匹配 org/springframework/testing/servlet/bla.xml， 还匹配 org/servlet/bla.xml


#### BeanFactory 和 ApplicationContext
   - BeanFactory （Bean工厂， IoC 容器）
        1. BeanFactory 是Spring 框架最核心的接口， 它提供了高级 IoC 的配置机制
       
        2. BeanFactory 是Spring 框架的基础设施， 面向Spring本身
       
        3. 类的通用工厂， 可以创建并管理各种类的对象
       
        4. 在初始化 BeanFactory ， 必须提供一种日志框架， 如Log4J
         
        5.  初始化容器时， 并未实例化 Bean， 第一次访问某个 Bean 时才实例化目标 Bean （ “ 第一次惩罚 ” ）

   - ApplicationContext （ 应用上下文， Spring 容器 ）
   
        1. ApplicationContext 建立在 BeanFactory 基础之上， 提供了面向应用的功能
        
        2. ApplicationContext 面向使用 Spring 框架的开发者， 几乎在所有场景都可以直接使用
        
        3. 主要实现类：
           -  ClassPathXmlApplicationContext ： 默认从类路径加载配置文件
           
           -   FileSystemXmlApplicationContext ： 默认从文件系统中加载配置文件
     
        4. ApplicationContext 初始化
       
           - 配置文件在类路径下， 优先考虑 ClassPathXmlApplicationContext
           
    ```java 
    ApplicationContext ctx = 
        new ClassPathXmlApplicationContext("com/smart/context/beans.xml");
    ```
           - 配置文件在文件系统路径下， 优先考虑 FileSystemXmlApplicationContext
           
    ```java
    ApplicationContext ctx = 
        new FileSystemXmlApplicationContext("com/smart/context/beans.xml"); 
    ```

           - 指定一组配置文件， Spring 会自动将多个配置文件在内存中 “ 整合 ”  成一个配置文件
           
    ```java
        ApplicationContext ctx = 
            new ClassPathXmlApplicationContext(
            new String[] { "conf/beans1.xml", "conf/beans2.xml" });
    ```
           - 初始化应用上下文时就实例化所有单实例的 Bean 


   - WebApplicationContext 
        1.  专门为 Web 应用准备， 允许从相对于 Web 根目录的路径中装载配置文件完成初始化工作
        
        2.  从 WebApplicationContext 中可以获得 ServletContext 的引用， 整个 WebApplicationContext 将作为属性放置到ServletContext 中
        
        3.  WebApplicationContext 作用域 ： singleton 、 prototype、 request 、 session、 global session 
        
        4.  Spring 的 Web 应用上下文和 Web 容器的上下文应用实现互访： WebApplicationContextUtils # getWebApplicationContext(ServletContext sc)
        
    ```java
    WebApplicationContext wac = (WebApplicationContext)servletContext.getAttribute
    (WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE)
    ```

        5. WebApplicationContext  初始化， 需要 ServletContext 实例，必须在拥有 Web 容器的前提下才能完成启动工作
            Spring 分别提供了用于启动 WebApplicationContext 的 Servlet 和 Web 容器监听器( 需要在 web.xml 中完成配置 )：

                    - org.springframework.web.context.ContextLoaderServlet
                   
                    - org.springframework.web.context.ContextLoaderListener

总结： 
1. 发现自己还有太多东西不懂， 对 Servlet 不够熟悉导致有些内容不能理解

2. 涉及到原理性的东西比如 Bean 生命周期以及核心工作原理的很难看懂

3. 知识学了但是运用得太少， 只是知道有这个东西不会运用， 应该多动手， 尝试着读别人的代码并且自己写

注： 以上内容来自上课笔记以及《 Spring4.x-企业应用开发实战》





