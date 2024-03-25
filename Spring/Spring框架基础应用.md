[toc]

# Spring框架基础应用

## Spring入门与IoC

### 什么是Spring

- 轻量级的开源框架
- 以简化企业级应用开发复杂性而生：Spring致力于提供一个以统一的、高效的方式构造整个应用，并且可以将单层框架以最佳的组合揉和在一起建立一个连贯的体系
- 以IOC 和 AOP 为核心的容器框架

### Spring的特性和优势

从Spring 框架的**特性**来看：

- 非侵入式：基于Spring开发的应用中的对象可以不依赖于Spring的API
- 控制反转：IOC——Inversion of Control，指的是将对象的创建权交给 Spring 去创建。使用 Spring 之前，对象的创建都是由我们自己在代码中new创建。而使用 Spring 之后。对象的创建都是给了 Spring 框架。
- 依赖注入：DI——Dependency Injection，是指依赖的对象不需要手动调用 setXX 方法去设置，而是通过配置赋值。
- 面向切面编程：Aspect Oriented Programming——AOP
- 容器：Spring 是一个容器，因为它包含并且管理应用对象的生命周期
- 组件化：Spring 实现了使用简单的组件配置组合成一个复杂的应用。在 Spring 中可以使用XML和Java注解组合这些对象。
- 一站式：在 IOC 和 AOP 的基础上可以整合各种企业应用的开源框架和优秀的第三方类库（实际上 Spring 自身也提供了表现层的 SpringMVC 和持久层的 Spring JDBC）

从使用Spring 框架的**好处**看：

- Spring 可以使开发人员使用 POJOs 开发企业级的应用程序。只使用 POJOs 的好处是你不需要一个 EJB 容器产品，比如一个应用程序服务器，但是你可以选择使用一个健壮的 servlet 容器，比如 Tomcat 或者一些商业产品。
- Spring 在一个单元模式中是有组织的。即使包和类的数量非常大，你只要担心你需要的，而其它的就可以忽略了。
- Spring 不会让你白费力气做重复工作，它真正的利用了一些现有的技术，像 ORM 框架、日志框架、JEE、Quartz 和 JDK 计时器，其他视图技术。
- 测试一个用 Spring 编写的应用程序很容易，因为环境相关的代码被移动到这个框架中。此外，通过使用 JavaBean-style POJOs，它在使用依赖注入注入测试数据时变得更容易。
- Spring 的 web 框架是一个设计良好的 web MVC 框架，它为比如 Structs 或者其他工程上的或者不怎么受欢迎的 web 框架提供了一个很好的供替代的选择。MVC 模式导致应用程序的不同方面(输入逻辑，业务逻辑和UI逻辑)分离，同时提供这些元素之间的松散耦合。模型(Model)封装了应用程序数据，通常它们将由 POJO 类组成。视图(View)负责渲染模型数据，一般来说它生成客户端浏览器可以解释 HTML 输出。控制器(Controller)负责处理用户请求并构建适当的模型，并将其传递给视图进行渲染。
- Spring 对 JavaEE 开发中非常难用的一些 API（JDBC、JavaMail、远程调用等），都提供了封装，使这些API应用难度大大降低。
- 轻量级的 IOC 容器往往是轻量级的，例如，特别是当与 EJB 容器相比的时候。这有利于在内存和 CPU 资源有限的计算机上开发和部署应用程序。
- Spring 提供了一致的事务管理接口，可向下扩展到（使用一个单一的数据库，例如）本地事务并扩展到全局事务（例如，使用 JTA）

### Spring有哪些组件?

![](.\spring-overview.png)

#### Core Container（Spring的核心容器）

Spring 的核心容器是其他模块建立的基础，由 Beans 模块、Core 核心模块、Context 上下文模块和 SpEL 表达式语言模块组成，没有这些核心容器，也不可能有 AOP、Web 等上层的功能。具体介绍如下。

- **Beans 模块**：提供了框架的基础部分，包括控制反转和依赖注入。
- **Core 核心模块**：封装了 Spring 框架的底层部分，包括资源访问、类型转换及一些常用工具类。
- **Context 上下文模块**：建立在 Core 和 Beans 模块的基础之上，集成 Beans 模块功能并添加资源绑定、数据验证、国际化、Java EE 支持、容器生命周期、事件传播等。ApplicationContext 接口是上下文模块的焦点。
- **SpEL 模块**：提供了强大的表达式语言支持，支持访问和修改属性值，方法调用，支持访问及修改数组、容器和索引器，命名变量，支持算数和逻辑运算，支持从 Spring 容器获取 Bean，它也支持列表投影、选择和一般的列表聚合等。

#### AOP、Aspects、Instrumentation和Messaging

在 Core Container 之上是 AOP、Aspects 等模块，具体介绍如下：

- **AOP 模块**：提供了面向切面编程实现，提供比如日志记录、权限控制、性能统计等通用功能和业务逻辑分离的技术，并且能动态的把这些功能添加到需要的代码中，这样各司其职，降低业务逻辑和通用功能的耦合。
- **Aspects 模块**：提供与 AspectJ 的集成，是一个功能强大且成熟的面向切面编程（AOP）框架。
- **Instrumentation 模块**：提供了类工具的支持和类加载器的实现，可以在特定的应用服务器中使用。
- **messaging 模块**：Spring 4.0 以后新增了消息（Spring-messaging）模块，该模块提供了对消息传递体系结构和协议的支持。
- **jcl 模块**： Spring 5.x中新增了日志框架集成的模块。

####  Data Access/Integration（数据访问／集成）

数据访问／集成层包括 JDBC、ORM、OXM、JMS 和 Transactions 模块，具体介绍如下。

- **JDBC 模块**：提供了一个 JDBC 的样例模板，使用这些模板能消除传统冗长的 JDBC 编码还有必须的事务控制，而且能享受到 Spring 管理事务的好处。
- **ORM 模块**：提供与流行的“对象-关系”映射框架无缝集成的 API，包括 JPA、JDO、Hibernate 和 MyBatis 等。而且还可以使用 Spring 事务管理，无需额外控制事务。
- **OXM 模块**：提供了一个支持 Object /XML 映射的抽象层实现，如 JAXB、Castor、XMLBeans、JiBX 和 XStream。将 Java 对象映射成 XML 数据，或者将XML 数据映射成 Java 对象。
- **JMS 模块**：指 Java 消息服务，提供一套 “消息生产者、消息消费者”模板用于更加简单的使用 JMS，JMS 用于用于在两个应用程序之间，或分布式系统中发送消息，进行异步通信。
- **Transactions 事务模块**：支持编程和声明式事务管理。

#### Web模块

Spring 的 Web 层包括 Web、Servlet、WebSocket 和 Webflux 组件，具体介绍如下。

- **Web 模块**：提供了基本的 Web 开发集成特性，例如多文件上传功能、使用的 Servlet 监听器的 IOC 容器初始化以及 Web 应用上下文。
- **Servlet 模块**：提供了一个 Spring MVC Web 框架实现。Spring MVC 框架提供了基于注解的请求资源注入、更简单的数据绑定、数据验证等及一套非常易用的 JSP 标签，完全无缝与 Spring 其他技术协作。
- **WebSocket 模块**：提供了简单的接口，用户只要实现响应的接口就可以快速的搭建 WebSocket Server，从而实现双向通讯。
- **Webflux 模块**： Spring WebFlux 是 Spring Framework 5.x中引入的新的响应式web框架。与Spring MVC不同，它不需要Servlet API，是完全异步且非阻塞的，并且通过Reactor项目实现了Reactive Streams规范。Spring WebFlux 用于创建基于事件循环执行模型的完全异步且非阻塞的应用程序。

### 创建Spring应用

![](.\spring-framework-helloworld.png)

#### 通过xml方式

顾名思义，就是将bean的信息配置.xml文件里，通过Spring加载文件为我们创建bean。这种方式出现很多早前的SSM项目中，将第三方类库或者一些配置工具类都以这种方式进行配置，主要原因是由于第三方类不支持Spring注解。

1. 在pom.xml中添加依赖

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.2.8.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-beans</artifactId>
    <version>5.2.8.RELEASE</version>
</dependency>
```

2. 创建JavaBean

```java
public class Car {
    private String brand;
    private String color;
    private Float price;
    // 构造方法、set|get方法、toString方法
}
```

3. 在applicationContext.xml文件中配置需要使用的bean

```xml
<!--
<bean> - 配置一个Bean对象
id属性 - 当前配置bean对象的唯一标识
class属性 - JavaBean的完全限定名
-->
<bean id="car1" class="com.gupao.vip.bean.Car">
    <!--
<property> - 属性标签[用于对于<bean>标签中的对象赋值]
name属性 - 对应JavaBean对象中的setXxx()方法中的Xxx
value属性 - 属性的值
-->
    <property name="brand" value="BYD仰望U8"></property>
    <property name="color" value="黑色"></property>
    <property name="price" value="1000000"></property>
</bean>
```

4. 获取容器中的对象

```java
@Test
public void getBeanTest() {
    //1.创建spring容器对象 applicationContext
    //使用xmlApplicationContext
    ApplicationContext applicationContext =
        new ClassPathXmlApplicationContext("applicationContext.xml");
    Car car = (Car) applicationContext.getBean("car1");
    System.out.println(car);
}
```

- **优点**： 可以使用于任何场景，结构清晰，通俗易懂
- **缺点**： 配置繁琐，不易维护，枯燥无味，扩展性差

#### 通过注解方式

通过在类上加注解的方式，来声明一个类交给Spring管理，Spring会自动扫描带有@Component，@Controller，@Service，@Repository这四个注解的类，然后帮我们创建并管理，前提是需要先配置Spring的注解扫描器。

1. 在pom.xml增加projectlombok

   ```xml
   <!-- 省略上面的 -->
   <dependency>
       <groupId>org.projectlombok</groupId>
       <artifactID>lombok</artifactID>
       <version>1.8.6</version>
   </dependency>
   ```

2. 使用标签简化bean的配置

   ```java
   @Data //自动生成getter/setter/toString
   @AllArgsConstructor //自动生成使用全部参数的构造器
   @NoArgsConstructor //自动生成无参构造器
   public class Emp {
       private String id;
       private String name;
       private String sex;
       private Integer age;
   }
   ```

3. 增加一个beanConfig.java, 在里面设置bean的额外方法

   ```java
   @Configuration
   @ComponentScan(value="com.gupao.vip.annotation.bean")//自动扫描所需的class
   public class BeanConfig {
       @Bean(value="myEmp")
       public Emp getEmp(){
           return new Emp();
       }
   }
   ```

4. 获取容器中的对象

   ```java
   @Test
   public void test() {
       //使用annotationConfigApplicationContext
       AnnotationConfigApplicationContext context =
           new AnnotationConfigApplicationContext(beanConfig.class);
       Emp emp = (Emp)context.getBean("myEmp");
       System.out.println(emp);
   }
   ```

- **优点**：开发便捷，通俗易懂，方便维护。
- **缺点**：具有局限性，对于一些第三方资源，无法添加注解。只能采用XML或JavaConfig的方式配置

###  这个例子体现了Spring的哪些核心要点

#### 控制反转 - IOC

- **如果没有Spring框架，我们需要自己创建User/Dao/Service等**，比如：

```java
UserDaoImpl userDao = new UserDaoImpl();
UserSericeImpl userService = new UserServiceImpl();
userService.setUserDao(userDao);
List<User> userList = userService.findUserList();
```

- **有了Spring框架，可以将原有Bean的创建工作转给框架, 需要用时从Bean的容器中获取即可，这样便简化了开发工作**

- Bean的创建和使用分离了。

  ```java
  // create and configure beans
  ApplicationContext context =
          new ClassPathXmlApplicationContext("aspects.xml", "daos.xml", "services.xml");
  
  // retrieve configured instance
  UserServiceImpl service = context.getBean("userService", UserServiceImpl.class);
  
  // use configured instance
  List<User> userList = service.findUserList();
  ```

1. Spring框架管理这些Bean的创建工作，即由用户管理Bean转变为框架管理Bean，这个就叫**控制反转 - Inversion of Control (IoC)**
2. Spring 框架托管创建的Bean放在哪里呢？ 这便是**IoC Container**;
3. Spring 框架为了更好让用户配置Bean，必然会引入**不同方式来配置Bean？ 这便是xml配置，Java配置，注解配置**等支持
4. Spring 框架既然接管了Bean的生成，必然需要**管理整个Bean的生命周期**等；
5. 应用程序代码从Ioc Container中获取依赖的Bean，注入到应用程序中，这个过程叫 **依赖注入(Dependency Injection，DI)** ； 所以说控制反转是通过依赖注入实现的，其实它们是同一个概念的不同角度描述。通俗来说就是**IoC是设计思想，DI是实现方式**
6. 在依赖注入时，有哪些方式呢？这就是构造器方式，@Autowired, @Resource, @Qualifier... 同时Bean之间存在依赖（可能存在先后顺序问题，以及**循环依赖问题**等）

#### Spring何时帮我们创建对象

容器启动时: `new ClassPathXmlApplicationContext("applicationContext.xml");` - ApplicationContext里面的所有bean都会被创建。

## Spring Bean装配与生命周期

IoC Container管理的是Spring Bean， 那么Spring Bean是什么呢？

### Spring Bean是什么

Spring里面的bean就类似是定义的一个组件，而这个组件的作用就是实现某个功能的，这里所定义的bean就相当于给了你一个更为简便的方法来调用这个组件去实现你要完成的功能。

### IOC容器的种类

BeanFactory

- Spring框架自用
- 延迟加载(IoC容器初始化的时候不会实例化对象, 在调用获取的时候才会创建对象)

ApplicationContext

- BeanFactory的子接口
- 面向使用Spring框架的开发者
- 扩展了很多BeanFactory不具备的功能【事件广播，资源加载，web支持等等...】
- 立即加载

### 注入Bean

#### 工厂注入

**静态工厂**

```java
public class StaticFactoryDemo {

    public static Map<String,UserBean> hashMap ;

    static {
        hashMap = new HashMap<String, UserBean>();
        hashMap.put("a1",new UserBean());
        hashMap.put("a2",new UserBean());
        hashMap.put("a3",new UserBean());
    }

    /**
     * 静态工厂提供的方法
     * @return
     */
    public static UserBean getInstance(){
        return hashMap.get("a1");
    }
    
    public static UserBean createInstance(String name) {
        return new UserBean(name);
    }
}
```

```xml
<!--  通过静态工厂的方式注入 -->
<bean class="com.gupaoedu.factory.StaticFactoryDemo" factory-method="getInstance" id="user"></bean>
```

使用：

```java
@Test
public void test() {
    ClassPathXmlApplicationContext context =
        new ClassPathXmlApplicationContext(applicationContext.xml);
    UserBean user = (UserBean)context.getBean("user");
    System.out.println(user);
}
```

动态注入:

```java
public class DynamicFactoryDemo {
    
    public UserBean getInstance(){
        return new UserBean();
    }
}
```

```xml
<!--  通过动态工厂的方式注入 -->
<bean class="com.gupaoedu.factory.DynamicFactoryDemo" id="dynamicFactoryDemo" ></bean>

<!--  从工厂对象中获取 需要的对象-->
<bean id="user2" factory-bean="dynamicFactoryDemo" factory-method="getInstance"/>
```

使用：

```java
@Test
public void test() {
    ClassPathXmlApplicationContext context =
        new ClassPathXmlApplicationContext(applicationContext.xml);
    UserBean user = (UserBean)context.getBean("user2");
    System.out.println(user);
}
```



#### 常用依赖注入方式

**setter 注入**

```xml
<bean id="car1" class="com.test.spring.bean.Car">
    <property name="brand" value="宝马730Li"></property>
    <property name="color" value="黑色"></property>
    <property name="price" value="870000"></property>
</bean>
```

本质上包含两步：

1. 第一步，需要`new car1()`创建对象, 所以需要默认构造函数
2. 第二步，调用`setCar1()`函数注入car的值, 所以需要`setCar1()`函数

**构造器注入**

```xml
<bean id="car3" class="com.test.spring.bean.Car">
    <!-- index,price,type 都是用来识别该条在构造器中对应的参数的 -->
    <constructor-arg value="奥迪A8" index="0"></constructor-arg>
    <constructor-arg value="1000000" name="price"></constructor-arg>
    <constructor-arg value="绿色" type="java.lang.String"></constructor-arg>
</bean>
```

本质上是new car3(String brand, Float price, String color)创建对象

#### 为什么推荐构造器注入方式

这个构造器注入的方式**能够保证注入的组件不可变，并且确保需要的依赖不为空**。此外，构造器注入的依赖总是能够在返回客户端（组件）代码的时候保证完全初始化的状态。

- **依赖不可变**：其实说的就是final关键字。
- **依赖不为空**（省去了我们对其检查）：当要实例化UserServiceImpl的时候，由于自己实现了有参数的构造函数，所以不会调用默认构造函数，那么就需要Spring容器传入所需要的参数，所以就两种情况：1、有该类型的参数->传入，OK 。2：无该类型的参数->报错。
- **完全初始化的状态**：这个可以跟上面的依赖不为空结合起来，向构造器传参之前，要确保注入的内容不为空，那么肯定要调用依赖组件的构造方法完成实例化。而在Java类加载实例化的过程中，构造方法是最后一步（之前如果有父类先初始化父类，然后自己的成员变量，最后才是构造方法），所以返回来的都是初始化之后的状态。

### 处理 Bean 的引用关系

```java
// Person中引用了另一个java bean - car
public class Person {
    private String name;
    private String sex;
    private int age;
    private Car car;
}

public class Car {
    private String brand;
    private String color;
    private Integer price;
}
```



```xml
<bean id="person1" class="com.test.spring.bean.Person">
    <property name="name" value="张三"></property>
    <property name="sex" value="男"></property>
    <property name="age" value="28"></property>
    <!-- 注入给引用的Car对象 -->
    <property name="car">
        <!-- setter注入 -->
        <bean class="com.test.spring.bean.Car">
            <property name="brand" value="本田CRV"></property>
            <property name="color" value="银色"></property>
            <property name="price" value="180000"></property>
        </bean>
        <!-- 可改为构造器注入 -->
        <!-- <bean class="com.test.spring.bean.Car">
<constructor-arg value="卡罗拉" type="java.lang.String"></constructor-arg>
<constructor-arg value="黑色" type="java.lang.String"></constructor-arg>
<constructor-arg value="120000" type="java.lang.Integer"></constructor-arg>
</bean> -->
        <!-- 还可改为引用 -->
        <!-- <ref bean="car1" /> -->
    </property>
</bean>
```

### Bean 装配细节

#### value值存在特殊符号

```xml
<bean id="car3" class="com.test.spring.bean.Car">
    <property name="brand">
        <!-- 使用![CDATA[]] 包装带有特殊符号的属性 -->
        <value><![CDATA[<BWM530>]]></value>
    </property>
    <property name="color" value="银色"></property>
    <property name="price" value="480000"></property>
</bean>
```

#### value的null值处理

```xml
<!-- Bean中存在另一个Bean的引用关系[null引用] -->
<bean id="person4" class="com.test.spring.bean.Person">
    <property name="name" value="张三"></property>
    <property name="sex" value="男"></property>
    <property name="age" value="28"></property>
    <!--Car对象-->
    <property name="car"><null/></property>
</bean>
```

#### 使用级联属性赋值

```xml
<!-- 级联属性的配置[将car2中的品牌覆盖] -->
<bean id="person5" class="com.test.spring.bean.Person">
    <constructor-arg value="八戒"></constructor-arg>
    <constructor-arg value="女"></constructor-arg>
    <constructor-arg value="38"></constructor-arg>
    <constructor-arg ref="car2"></constructor-arg>
    <!-- car.brand中的car在调用Person类中的getCar()方法 -->
    <property name="car.brand" value="皇冠"></property>
</bean>
```

#### Bean引用List

```java
public class Person {
    private String name; private String sex; private int age;
    private List<Car> cars;
}
```

```xml
<bean id="person2" class="com.test.spring.bean.list.Person">
    <property name="name" value="八戒"></property>
    <property name="sex" value="男"></property>
    <property name="age" value="80"></property>
    <property name="cars">
        <list>
            <bean class="com.test.spring.bean.list.Car">
                <property name="brand" value="拖拉机"></property>
                <property name="color" value="奶奶灰"></property>
                <property name="price" value="20000"></property>
                <!-- 或通过ref引用 -->
                <!-- <ref bean="car1"/> -->
                <!-- <ref bean="car2"/> -->
            </bean>
        </list>
    </property>
</bean>
```

#### Bean引用Map

```java
public class Person {
    private String name; private String sex; private int age;
    private Map<String, Car> cars;
}
```

```xml
<bean id="person1" class="com.test.spring.bean.map.Person">
    <property name="name" value="沙悟净"></property>
    <property name="sex" value="男"></property>
    <property name="age" value="0"></property>
    <property name="cars">
        <map>
            <entry key="C1" value-ref="car1"></entry>
            <entry key="C2" value-ref="car2"></entry>
        </map>
    </property>
</bean>
```

#### Bean引用Properties

```java
public class MyProps {
    private Properties propertie;
    public void setPropertie(Properties propertie) {
        this.propertie = propertie;
    } public String toString() {
        return "MyProps [propertie=" + propertie + "]";
    }
}
```

```xml
<bean id="prop1" class="com.test.spring.props.MyProps">
    <property name="propertie">
        <props>
            <prop key="driver">com.mysql.jdbc.Driver</prop>
            <prop key="url">jdbc:mysql:localhost:3306</prop>
            <prop key="username">root</prop>
            <prop key="password">123</prop>
        </props>
    </property>
</bean>
```

### 自动装配bean

`@Autowired` 注解是Spring框架的一部分，用于自动装配（自动注入）Spring Bean到其他Bean中。它的主要作用是帮助我们消除手动配置Bean依赖关系的繁琐工作，使代码更加简洁和可维护。通过使用 `@Autowired` 注解，我们可以轻松地将一个Bean注入到另一个Bean中，而无需手动创建实例或者进行繁琐的配置。

**使用**

- 通过xml配置文件： （`<bean autowire="byName"></bean>`）
- 标注注解：（`@Autowired`）

**自动装配方式**

- byType
- byName

#### 通过xml配置文件

```java
public class Address {
    private String city;
    private String street;
}

public class Car {
    private String brand;
    private String color;
    private Integer price;
}

public class Person {
    private String name;
    private String sex;
    private int age;
    private Car car1;
    private Address address1;
}
```

```xml
<beans xmlns:p="http://www.springframework.org/schema/p">
    <bean id="car1" class="com.test.spring.bean.autowire.Car"
          p:brand="宝马X3" p:color="白色" p:price="470000"></bean>
    <bean id="address1" class="com.test.spring.bean.autowire.Address"
          p:city="长沙" p:street="银盆岭街道"></bean>
    <!-- 未使用自动装配 -->
    <bean id="person1" class="com.test.spring.bean.autowire.Person"
          p:name="八戒" p:sex="男" p:age="18" p:car-ref="car1"
          p:address-ref="address1"></bean>
    <!-- 当autowire="byType" 请注意在bean配置时不能出现多个相同类型的bean，如：不能出现car1 car2... -->
    <bean id="person2" class="com.test.spring.bean.autowire.Person"
          p:name="八戒" p:sex="男" p:age="18" autowire="byType"></bean>
    <!-- 当autowire="byName" 请注意Bean的ID一定要与之对应的JavaBean中要设置的属性的属性名相同 -->
    <bean id="person3" class="com.test.spring.bean.autowire.Person"
          p:name="八戒" p:sex="男" p:age="18" autowire="byName"></bean>
</beans>
```

### Bean的继承和依赖

父子 Bean

- 被继承者称为父 Bean
- 继承者称为子 Bean

子 Bean 从 父 Bean 中继承配置（修饰有autowire、abstract的属性不会被继承）

应用场景：如果有一些数据是公共的、通用的，可以让它成为一个父Bean，其
他Bean通过继承拿到它的配置（目的在于避免XML中的大量重复定义）

```java
public class Car {
    private String brand;
    private String color;
    private Integer price;
}
```

```xml
<!-- Bean的继承 一般而言指 子Bean 通过继承获取父Bean属性信息 -->
<!-- 父Bean -->
<bean id="car1" p:brand="大众辉腾" p:color="黑色" p:price="1000000" abstract="true"></bean>
<!-- 子Bean -->
<!--
parent="car1" - 表示当前的car2继承了car1的属性信息
car2也可以在继承获取到父Bean信息后 再次进行属性描述[覆盖从父Bean中获取的信息]
-->
<bean id="car2" class="com.test.spring.bean.Car" p:brand="大牛" parent="car1"></bean>
```

通过 depends-on 属性设定 Bean 的前置依赖 Bean，前置依赖的 Bean 会在本Bean 实例化之前创建好；可以通过逗号或空格配置多个前置Bean

```java
public class Person {
    private String name;
    private String sex;
    private Integer age;
    private Car car;
}
```

```xml
<!-- 没有依赖 -->
<bean id="person1" class="com.test.spring.bean.Person"
p:name="张三" p:sex="男" p:age="18" p:car-ref="car3"></bean>
<!-- 有依赖 -->
<bean id="person2" class="com.test.spring.bean.Person"
p:name="张三" p:sex="男" p:age="18" depends-on="car3" p:car-ref="car3"></bean>
<!-- 前置依赖的Bean[car3] 会在Bean[person2] 实例化之前创建好 -->
<!-- 否则spring会按照bean在xml中出现的顺序加载 -->
<bean id="car3" class="com.test.spring.bean.Car" p:brand="大众辉腾"
p:color="黑色" p:price="1000000"></bean>
```

### Bean的Scope

| Scope                                                        | Description                                                  |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [singleton](https://docs.spring.io/spring-framework/reference/core/beans/factory-scopes.html#beans-factory-scopes-singleton) | (Default) Scopes a single bean definition to a single object instance for each Spring IoC container. |
| [prototype](https://docs.spring.io/spring-framework/reference/core/beans/factory-scopes.html#beans-factory-scopes-prototype) | Scopes a single bean definition to any number of object instances. |
| [request](https://docs.spring.io/spring-framework/reference/core/beans/factory-scopes.html#beans-factory-scopes-request) | Scopes a single bean definition to the lifecycle of a single HTTP request. That is, each HTTP request has its own instance of a bean created off the back of a single bean definition. Only valid in the context of a web-aware Spring `ApplicationContext`. |
| [session](https://docs.spring.io/spring-framework/reference/core/beans/factory-scopes.html#beans-factory-scopes-session) | Scopes a single bean definition to the lifecycle of an HTTP `Session`. Only valid in the context of a web-aware Spring `ApplicationContext`. |
| [application](https://docs.spring.io/spring-framework/reference/core/beans/factory-scopes.html#beans-factory-scopes-application) | Scopes a single bean definition to the lifecycle of a `ServletContext`. Only valid in the context of a web-aware Spring `ApplicationContext`. |
| [websocket](https://docs.spring.io/spring-framework/reference/web/websocket/stomp/scope.html) | Scopes a single bean definition to the lifecycle of a `WebSocket`. Only valid in the context of a web-aware Spring `ApplicationContext`. |

When you create a bean definition, you create a recipe for creating actual instances of the class defined by that bean definition. The idea that a bean definition is a recipe is important, because it means that, as with a class, you can create many object instances from a single recipe.

You can control not only the various dependencies and configuration values that are to be plugged into an object that is created from a particular bean definition but also control the scope of the objects created from a particular bean definition. This approach is powerful and flexible, because you can choose the scope of the objects you create through configuration instead of having to bake in the scope of an object at the Java class level.

```java
public class Address {
    private String city;
}
```

```xml
<!--
默认的情况下Bean使用的scope属性值为singleton
IoC容器 只会对这个Bean实例化一次
-->
<bean id="address" class="com.test.spring.bean.Address" p:city="武汉" scope="singleton"></bean>
<!--
将scope属性值修改为prototype IoC容器启动时不会装载这个类的实例
在每一次 getBean() 时都会对这个Bean实例化一次
-->
<bean id="address2" class="com.test.spring.bean.Address" p:city="长沙" scope="prototype"></bean>
```

The `request`, `session`, `application`, and `websocket` scopes are available only if you use a web-aware Spring `ApplicationContext` implementation (such as `XmlWebApplicationContext`). If you use these scopes with regular Spring IoC containers, such as the `ClassPathXmlApplicationContext`, an `IllegalStateException` that complains about an unknown bean scope is thrown.

### 使用外部属性文件

有时需要将一些部署细节 和 Bean 配置相分离

Spring 提供了名为 PropertyPlaceholderConfigurer 的 BeanFactory 后置处理器

```xml
<context:property-placeholder location="classpath:db.properties"/>
```

```xml
<!--
解决方案-1：原始方案
此处使用C3P0的数据库连接池作为数据源
使用了属性注入的方式构建Bean [直接填充value属性]
-->
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
    <property name="user" value="root"></property>
    <property name="password" value="123"></property>
    <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
    <property name="jdbcUrl" value="jdbc:mysql:///mytest"></property>
</bean>

<!--
解决方案-2：外部属性文件方案
此处使用C3P0的数据库连接池作为数据源
使用了属性注入的方式构建Bean [读取外部属性文件的内容填充value属性]
-->
<context:property-placeholder location="classpath:db.properties"/>
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource"
      p:user="${user}" p:password="${password}"
      p:driverClass="${driverclass}" p:jdbcUrl="${jdbcurl}">
</bean>
```

### SpEL表达式运用

**定界符**： #{…}（大框号中的字符都将被认为是 SpEL）

SpEL的作用：

#### 字面量表达

整数：`<property name="count" value="#{5}" />`
小数：`<property name="frequency" value="#{89.7}" />`
Boolean：`<property name="enabled" value="#{false}" />`
科学计数法：`<property name="capacity" value="#{1e4}" />`
String：`<property name='name' value='#{"Chuck"}' />`

#### 比较运算符

- 小于：< （lt）
- 大于：>（gt）
- 大于等于：>=（ge）
- 等于：==（eq）
- 小于等于：<=（le）

```xml
<property name="equal" value="#{counter.total == 100}"/>
<property name="equalEL" value="#{counter.total le 100000}"/>
```

#### if-else 运算符

```xml
<property name="singer" value="#{music.select() == '冷雨夜' ? ‘Beyond’: '其他'}"/>
<!-- 标准写法 -->
<property name="song" value="#{beyond.song != null ? beyond.song : liudehua.song}"/>
<!-- 简化写法 -->
<property name="song" value="#{beyond.song ?: liudehua.song}"/>
```

在注解中使用:

```java
// 标准写法
@Value("#{person.name != null ? person.name : 'zhangsanfeng'}")
private String empName;
// 简化写法
@Value("#{person.name ?: 'zhangsanfeng'}")
private String empName;
```

#### 正则表达式

比如验证邮箱格式:

```xml
<property name="validEmail"
value="#{user.email matches '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,4}'}"/>
```

#### 定义JavaBean

```xml
<!--
创建Car对象，涉及知识点：T() 调用静态方法或静态属性
通过 T()调用一个类的静态方法 它将返回一个 Class Object 然后再调用相应的方法或属性
-->
<bean id="car" class="com.test.spring.bean.spel.Car">
    <property name="brand" value="#{'奥迪'}"></property>
    <property name="color" value="黑色"></property>
    <property name="price" value="#{300000}"></property>
    <property name="discount" value="#{T(java.lang.Math).PI * 2.5}"></property>
</bean>
<bean id="address" class="com.test.spring.bean.spel.Address">
    <property name="city" value="武汉"></property>
    <property name="street" value="金银路"></property>
</bean>
<bean id="person" class="com.test.spring.bean.spel.Person">
    <property name="name" value="八戒"></property>
    <!-- 引用 Bean、属性和方法 -->
    <property name="info" value="#{address.city}"></property>
    <!--SpEL支持的运算符号:
	算数运算符：+, -, *, /, %, ^（+加号还可以用作字符串连接）
	比较运算符： <, >, ==, <=, >=, lt, gt, eq, le, ge -->
    <property name="city" value="#{car.price ge 200000 ? '高富帅' : '矮矬穷'}"></property>
    <property name="car" value="#{car}"></property>
    <property name="address" ref="address"></property>
</bean>
```

### Bean的生命周期

Spring IoC 容器可以管理 Bean 的生命周期，允许在 Bean 生命周期的特定点执行定制的任务

IoC 容器对 Bean 的生命周期进行管理的过程：

1. 通过构造器或工厂方法创建 Bean 实例
2. Bean 的属性设置值和对其他 Bean 的引用
3. 调用 Bean 的初始化方法(init-method)
4. Bean 可以被使用
5. 当容器关闭时, 调用 Bean 的销毁方法(destory-method)

#### 初始化和销毁方法

Bean 声明中设置 init-method 和 destroy-method 属性

```java
public class Person {
    private String name;
    public Person() { System.out.println("生命周期-1：通过构造器或工厂方法创建 Bean 实例"); }
    public String getName() { return name; }
    public void setName(String name) {
        System.out.println("生命周期-2：为 Bean 的属性设置值和对其他 Bean 的引用");
        this.name = name;
    }
    //init() [init-method="initTest"]
    public void initTest() { System.out.println("生命周期-3：调用 Bean 的初始化方法"); }
    //destroy() [destroy-method="destroyTest"]
    public void destroyTest() { System.out.println("生命周期-5：当容器关闭时, 调用 Bean 的销毁方法"); }
    public String toString() {
        System.out.println("生命周期-4：Bean被调用");
        return "Person [name=" + name + "]";
    }
}
```

```xml
<bean id="person" class="com.test.spring.bean.lifecycle.Person" init-method="initTest" destroy-method="destroyTest">
    <property name="name" value="八戒"></property>
</bean>
```

#### 诸多钩子方法

在 Bean 的整个生命周期中，Spring提供了许多钩子方法，方便程序员参与管理Bean

- InitializingBean
- DisposableBean
- BeanNameAware、BeanClassLoaderAware、BeanFactoryAware
- BeanPostProcessor
- DestructionAwareBeanPostProcessor
- BeanFactoryPostProcessor
- BeanDefinitionRegistryPostProcessor
- FactoryBean

以后置处理器为例，看看如何介入Bean的管理

1、创建JavaBean

```java
public class Address {
    private String city;
    private String street;
    public Address() { System.out.println("生命周期-1：通过构造器或工厂方法创建 Bean 实例"); }
    public String getCity() { return city; }
    public void setCity(String city) {
        System.out.println("生命周期-2：属性注入");
        this.city = city;
    }
    public String getStreet() { return street; }
    public void setStreet(String street) {
        System.out.println("生命周期-2：属性注入");
        this.street = street;
    }
    public void initTest(){ System.out.println("生命周期-4：调用 Bean 的初始化方法"); }
    public void destroyTest(){ System.out.println("生命周期-7：当容器关闭时, 调用 Bean 的销毁方法"); }
}
```

2、实现BeanPostProcessor接口

```java
public class MyBeanPostProcessor implements BeanPostProcessor {
    // 在Bean-init()之后执行
    public Object postProcessAfterInitialization(Object arg0, String arg1)
        throws BeansException {
        System.out.println("生命周期-5：init()之后执行");
        return arg0;
    }
    // 在Bean-init()之前执行
    public Object postProcessBeforeInitialization(Object arg0, String arg1)
        throws BeansException {
        System.out.println("生命周期-3：init()之前执行");
        //在后置处理器中对Bean的属性进行修改
        Address address=(Address)arg0;
        address.setStreet("火车西站");
        return address;
    }
}
```

3、配置文件applicationContextLifecycle.xml

```xml
<!-- 将后置处理器配置进来 -->
<bean class="com.test.spring.bean.lifecycle.MyBeanPostProcessor"></bean>
<bean id="address" class="com.test.spring.bean.lifecycle.Address"
      init-method="initTest" destroy-method="destroyTest">
    <property name="city" value="北京"></property>
    <property name="street" value="北海公园"></property>
</bean>
```

#### 钩子方法的调用

- 步骤-1：实现Spring提供给的接口 并重写其方法
- 步骤-2：Spring 容器启动时 自动探测实现了对应接口的Bean
- 步骤-3：Spring会去应用实现了对应接口Bean

## 使用Spring注解

```java
@Configuration
@ComponentScan
public class JavaConfig {

    @Bean
    public IUserDao userDao(){
        return new UserDaoImpl();
    }

    /**
     *
     * @param userDao 本方法调用的时候会从IoC容器中查找类型为IUserDao的名称为 userDao的对象
     * @return
     */
    @Bean
    public IUserService userService(IUserDao userDao){
        IUserService userService = new UserServiceImpl();
        ((UserServiceImpl) userService).setDao(userDao);
        return userService;
    }

    @Bean
    public UserController userController(IUserService userService){
        UserController controller = new UserController();
        controller.setService(userService);
        return controller;
    }
}
```

### 配置注解

| 注解名称       | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| @Configuration | 把一个类作为一个IoC容器，它的某个方法头上如果注册了@Bean，就会作为这个Spring容器中的Bean。 |
| @ComponentScan | 在配置类上添加 @ComponentScan 注解。该注解默认会扫描该类所在的包下所有的配置类，相当于之前的 <context:component-scan> |
| @Scope         | 用于指定scope作用域的（用在类上）                            |
| @Lazy          | 表示延迟初始化                                               |
| @Conditional   | Spring4开始提供，它的作用是按照一定的条件进行判断，满足条件给容器注册Bean。 |
| @Import        | 导入外部资源                                                 |
| 生命周期控制   | @PostConstruct用于指定初始化方法（用在方法上）@PreDestory用于指定销毁方法（用在方法上）@DependsOn：定义Bean初始化及销毁时的顺序 |

#### @PostConstruct @PreDestory @DependsOn

@PostConstruct 系统初始化完成后的回调方法

@PreDestory 系统销毁时的回调方法

@DependsOn指定实例化对象的先后顺序

```java
@Component
@DependsOn({"user"}) // Person的实例化依赖于User对象的实例化，也就是User先于Person实例化
public class Person {

    public Person(){
        System.out.println("Person 构造方法执行了...");
    }
    
    @PostConstruct
    public void init() {
        System.out.println("实例初始化完成");
    }
}
```

#### @Import注解

将类型加入到IoC容器中的方式有哪些？

> 1.基于XML文件的方式<bean >
>
> 2.基于XML文件的方式<context:Component-Scan> + @Component
>
> 3.基于Java配置类@Bean
>
> 4.基于Java配置类@ComponentScan + @Component
>
> 5.FactoryBean + getObject方法
>
> 6.@Import

**静态使用**

直接在@Import注解中直接定义要引入到IoC容器中的类型

```java
@Configuration
@ComponentScan
@Import({Student.class})//如果Student没有@Component标识的话可以用Import导入
public class JavaConfig{}
```

缺点是：无法灵活的指定引入的类型

**动态使用-ImportSelector**

通过重写selectImports方法来达到灵活控制引入类型的目的

```java
public class GpImportSelector implements ImportSelector {

    /**
     *
     * @param annotationMetadata
     * @return IoC 要加载的类型的全路径的字符串数组
     */
    public String[] selectImports(AnnotationMetadata annotationMetadata) {
        // 在此处实现不同的业务逻辑控制
        return new String[]{LoggerService.class.getName(),CacheService.class.getName()};
    }
}
```

```java
@Configuration
@ComponentScan
@Import({GpImportSelector.class})
public class JavaConfig{
}
```

使用:

```java
ApplicationContext ac = new AnnotationConfigApplicationContext(JavaConfig.class);
String[] beanNames = ac.getBeanDefinitionNames();
for (String name : beanNames) {
    System.out.println(name);
}
//这里之前引入的LoggerService和CacheService都会被print出来
```

动态使用-**ImportBeanDefinitionRegistrar**

```java
public class GpImportBeanDefinitionRegistrar implements ImportBeanDefinitionRegistrar {
    /**
     *
     * @param annotationMetadata
     * @param beanDefinitionRegistry IoC容器中管理对象的一个注册器
     */
    public void registerBeanDefinitions(AnnotationMetadata annotationMetadata, BeanDefinitionRegistry beanDefinitionRegistry) {
        // 需要将添加的对象包装为一个RootBeanDefinition对象
        RootBeanDefinition cache = new RootBeanDefinition(CacheService.class);
        beanDefinitionRegistry.registerBeanDefinition("cache",cache);

        RootBeanDefinition logger = new RootBeanDefinition(LoggerService.class);
        beanDefinitionRegistry.registerBeanDefinition("logger",logger);
    }
}
```

```java
@Configuration
@ComponentScan
@Import({GpImportBeanDefinitionRegistrar.class})
public class JavaConfig{
}
```

### 条件注解

@Conditional注解  通过重写matches方法来动态实现是否加载某类型

```java
public class ConditionalOnBean implements Condition {
    /**
     *  如果IoC容器中有Person对象就返回true 否在返回false
     *
     * @param conditionContext
     * @param annotatedTypeMetadata
     * @return
     *    true 表示IoC容器加载该类型
     *    false 表示IoC容器不加载该类型
     */
    public boolean matches(ConditionContext conditionContext, AnnotatedTypeMetadata annotatedTypeMetadata) {
        boolean flag = conditionContext.getRegistry().containsBeanDefinition("person");
        System.out.println(flag + " **** ");
        return flag;
    }
}
```

使用

```java
@Configuration
public class JavaConfig {

    /**
     * @Conditional(ConditionalOnBean.class)
     *    是一个条件注解 表示如果ConditionalOnBean中的matches方法返回true就加载
     *        返回false就不加载
     * @return
     */
    @Bean
    @Conditional(ConditionalOnBean.class)
    public User user(){
        return new User();
    }
}
```

扩展：SpringBoot中的ConditionalXXX

| @Conditional扩展注解            | 作用(判断是否满足当前指定条件)                   |
| ------------------------------- | ------------------------------------------------ |
| @ConditionalOnJava              | 系统的Java版本是否符合要全                       |
| @ConditionalOnBean              | 容器中存在指定的Bean                             |
| @ConditionalOnMissingBean       | 容器中不存在指定的Bean                           |
| @ConditionalOnExpression        | 满足SpEL表达式                                   |
| @ConditionalOnClass             | 系统中有指定的类                                 |
| @ConditionalOnMissingClass      | 系统中没有指定的类                               |
| @ConditionalOnSingleCandidate   | 容器中只有一个指定的Bean，或者这个Bean是首选Bean |
| @ConditionalOnProperty          | 系统中指定的属性是否有指定的值                   |
| @ConditionalOnResource          | 类路径下是否存在指定的资源文件                   |
| @ConditionalOnWebApplication    | 当前是Web环境                                    |
| @ConditionalOnNotWebApplication | 当前不是Web环境                                  |
| @ConditionalOnJndi              | JNDI存在指定项                                   |



### 赋值注解

| 注解名称        | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| @Component      | 泛指组件，当组件不好归类的时候，我们可以使用这个注解进行标注。 |
| @Service        | 用于标注业务层组件                                           |
| @Controller     | 用于标注控制层组件                                           |
| @Repository     | 用于标注数据访问组件，即DAO组件。                            |
| @Value          | 普通数据类型赋值                                             |
| @Autowired      | 默认按类型装配，如果我们想使用按名称装配，可以结合@Qualifier注解一起使用 |
| @PropertySource | 读取配置文件赋值                                             |
| @Qualifier      | 如存在多个实例配合使用                                       |
| @Primary        | 自动装配时当出现多个Bean候选者时，被注解为@Primary的Bean将作为首选者，否则将抛出异常 |
| @Resource       | 默认按名称装配，当找不到与名称匹配的bean才会按类型装配。     |

- @Value  注入int、float、String等基本数据类型，只能标注在成员变量、setter方法上。

- @Autowired  按类型自动装配，可标注在成员变量（官方不推荐）、构造方法、setter方法上。
  - @Qualifier  按名称自动装配，需要和@Autowired搭配使用，只能标注在成员变量（官方不推荐）、setter方法上。
- @Resource  按名称或类型自动装配，需要第三方包 javax.annotation.jar 的支持，只能标注在成员变量、setter方法上。

#### 过程

1. Instantiate objects through the reflection mechanism
   - The Spring framework instantiates the bean object to be injected through the reflection mechanism and stores it in a Map, where Key is the bean name and Value is the bean instance object.
2. bean property injection via the JavaBean specification
   - Next, Spring will find the property to be injected based on the JavaBean specification (i.e. the property name and the setter method of the property) and call the corresponding setter method to inject the property through the reflection mechanism.
3. Search strategies for dependency injection
   - Search by type: When the type of the property to be injected matches the type of multiple beans in the container, a different search strategy is chosen depending on the annotation. For example, when using the @Autowired annotation, the default is to find the corresponding bean by type matching.
   - Search by name: When the type of the property to be injected is not unique, or the name of the bean to be injected does not match the name of the property, you can use the @Qualifier annotation to specify the name of the bean to be injected.

#### @Resource和@Autowired的区别

- Different sources: `@Resource` is an annotation defined by the Java EE specification, whereas `@Autowired` is an annotation provided by Spring.
- Attributes are different: The `@Resource` annotation does not have a required attribute, but a name attribute that indicates the name of the bean to be injected, while the `@Autowired` annotation has a required and a name attribute, where required indicates whether the object must be injected and defaults to true, and name indicates the name of the bean to be injected.
- Different lookup method: `@Resource` annotation by default is based on byName, if not found then byType, while `@Autowired` by default is based on byType.

#### @Value

@Value 帮助我们给数组动态的设值

```java
@Configuration
@ComponentScan("com.gupaoedu")
public class JavaConfig {

}
```



```java
@Component
public class User {

    @Value("bobo") // 注入普通的字符串
    private String userName ;

    @Value("#{systemProperties['os.name']}")
    private String systemPropertiesName; // 注入操作系统的信息

    @Value("#{T(java.lang.Math).random()*100}")
    private double randomNumber; // 注入表达式的结果

    @Value("#{person.personName}")
    private String fromPersonName; // 注入其他Bean的属性


    @Value("classpath:test.txt")
    private Resource resourceFile;

    @Value("http://www.baidu.com")
    private Resource baiduFile;
}
```

读取第三方的Properties文件中的信息

```properties
# 文件名: spring-db.properties
jdbc.url = 192.168.120.1:3306/...
jdbc.drivername = oracle
```

注意我们需要在Java配置类中通过@PropertySouce注解来显示的引入属性文件

```java
@Configuration
@ComponentScan("com.gupaoedu")
// 显示的指定要加载的属性文件
@PropertySource({"classpath:spring-db.properties"})
public class JavaConfig {

}
```

```java
@Value("${jdbc.url}")
private String JdbcUrl;

@Value("${jdbc.drivername}")
private String classDriverName;
```



### 注解编程的使用

当我们在将一个类上标注@Service或者@Controller或@Component或@Repository注解之后，***spring的组件扫描就会自动发现它，并且会将其初始化为spring applicationContext中的bean。*** 而且初始化是根据无参构造函数。

```java
@Component // 需要被IoC容器加载
public class UserController {
    
    @Autowired
    private IUserService service;

    public List<User> query() {
        return service.query();
    }
}
```

#### 基于配置文件的方式

```xml
<beans>
    <!-- 添加扫描的路径 指定从哪些package下加载被 @Component标注的类型-->
    <!--<context:component-scan base-package="com.gupaoedu"/>-->
    <context:component-scan base-package="com.gupaoedu.controller" />
    <context:component-scan base-package="com.gupaoedu.service.impl" />
    <context:component-scan base-package="com.gupaoedu.dao.impl" />
</beans>
```

可以指定只扫描某种类型的标签

```xml
<!--
    use-default-filters="false" 表示不适用默认的过滤器  
    默认过滤器会识别 @Componenet @Controller @Service @Repository
-->
<context:component-scan base-package="com.gupaoedu.controller" use-default-filters="false">
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
<context:component-scan base-package="com.gupaoedu.service.impl,com.gupaoedu.dao.impl" use-default-filters="true" >
    <!-- 排除掉某个注解 -->
    <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller" />
</context:component-scan>
```

#### 基于Java配置类的方式

我们需要在Java配置类中通过@ComponentScan注解来指定扫描的路径

默认的情况下扫描的是当前路径及其子路径下的所有的被@Componenet @Controller @Service @Repository标注的类型

```java
@Configuration
@ComponentScans({
    //在controller package下，只扫描@Controller标注的bean
        @ComponentScan(value = {"com.gupaoedu.controller"}
                ,useDefaultFilters = false
                ,includeFilters = {@ComponentScan.Filter(Controller.class)})
    //在service，dao package下，不扫描@Controller标注的bean
        ,@ComponentScan(value = {"com.gupaoedu.service","com.gupaoedu.dao"}
                ,useDefaultFilters = true
                ,excludeFilters = {@ComponentScan.Filter(Controller.class)})
})
public class JavaConfig {
}
```

### 多环境下的解决方案之Profile

Profile本质就是Conditional的实现

```java
@Bean
    @Profile("pro") // 其实Profile注解本质上就是Conditional的一种实现
    public GpDataSource proDataSource(){
        GpDataSource ds = new GpDataSource("root","123","192.168.11.190");
        return ds;
    }

    @Bean
    @Profile("dev")
    public GpDataSource devDataSource(){
        GpDataSource ds = new GpDataSource("admin","456","192.168.12.190");
        return ds;
    }

public static void main(String[] args) {
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext();
        ac.getEnvironment().setActiveProfiles("dev");
        ac.register(JavaConfig.class);
        ac.refresh();
        System.out.println(ac.getBean(GpDataSource.class));
    }
```

## Spring AOP及其应用

AOP为Aspect Oriented Programming的缩写，意为：面向切面编程

我们将记录日志功能解耦为日志切面，它的目标是解耦。进而引出AOP的理念：就是将分散在各个业务逻辑代码中相同的代码通过**横向切割**的方式抽取到一个独立的模块中！

![](.\spring-framework-aop-4.png)

OOP面向对象编程，针对业务处理过程的实体及其属性和行为进行抽象封装，以获得更加清晰高效的逻辑单元划分。而AOP则是针对业务处理过程中的切面进行提取，它所面对的是处理过程的某个步骤或阶段，以获得逻辑过程的中各部分之间低耦合的隔离效果。这两种设计思想在目标上有着本质的差异。

![](.\spring-framework-aop-2.png)

AOP可以拦截指定的方法并且对方法增强，而且无需侵入到业务代码中，使业务与非业务处理逻辑分离，比如Spring的事务，通过事务的注解配置，Spring会自动在业务方法中开启、提交业务，并且在业务处理失败时，执行相应的回滚策略。

### AOP术语

![](.\aop.png)

- **连接点（Jointpoint）**：A joinpoint is a *candidate* point in the **Program Execution** of the application where an aspect can be plugged in. This point could be a method being called, an exception being thrown, or even a field being modified. These are the points where your aspect’s code can be inserted into the normal flow of your application to add new behavior.
- **切入点（Pointcut）**： A pointcut defines at what joinpoints, the associated Advice should be applied. Advice can be applied at any joinpoint supported by the AOP framework. Of course, you don’t want to apply all of your aspects at all of the possible joinpoints. Pointcuts allow you to specify where you want your advice to be applied. Often you specify these pointcuts using explicit class and method names or through regular expressions that define matching class and method name patterns. Some AOP frameworks allow you to create dynamic pointcuts that determine whether to apply advice based on runtime decisions, such as the value of method parameters.
- **通知（Advice）**：This is an object which includes API invocations to the system wide concerns representing the action to perform at a joinpoint specified by a point.
- **方面/切面（Aspect）**：横切关注点的模块化，比如上边提到的日志组件。可以认为是通知、引入和切入点的组合；在Spring中可以使用Schema和@AspectJ方式进行组织实现；在AOP中表示为**在哪干和干什么集合**；
- **引入（introduction）**：Declaring additional methods or fields on behalf of a type. Spring AOP lets you introduce new interfaces (and a corresponding implementation) to any advised object. For example, you could use an introduction to make a bean implement an `IsModified` interface, to simplify caching. (An introduction is known as an inter-type declaration in the AspectJ community.)
- **目标对象（Target Object）**：An object being advised by one or more aspects. Also referred to as the "advised object". Since Spring AOP is implemented by using runtime proxies, this object is always a proxied object.
- **织入（Weaving）**：linking aspects with other application types or objects to create an advised object. This can be done at compile time (using the AspectJ compiler, for example), load time, or at runtime. Spring AOP, like other pure Java AOP frameworks, performs weaving at runtime.
- **AOP代理（AOP Proxy）**：AOP框架使用代理模式创建的对象，从而实现在连接点处插入通知（即应用切面），就是通过代理来对目标对象应用切面。在Spring中，AOP代理可以用JDK动态代理或CGLIB代理实现，而通过拦截器模型应用切面。在AOP中表示为**怎么实现的一种典型方式**；

#### 通知类型

- **前置通知（Before advice）**：在某连接点之前执行的通知，但这个通知不能阻止连接点之前的执行流程（除非它抛出一个异常）。
- **后置通知（After returning advice）**：在某连接点正常完成后执行的通知：例如，一个方法没有抛出任何异常，正常返回。
- **异常通知（After throwing advice）**：在方法抛出异常退出时执行的通知。
- **最终通知（After (finally) advice）**：当某连接点退出的时候执行的通知（不论是正常返回还是异常退出）。
- **环绕通知（Around Advice）**：包围一个连接点的通知，如方法调用。这是最强大的一种通知类型。环绕通知可以在方法调用前后完成自定义的行为。它也会选择是否继续执行连接点或直接返回它自己的返回值或抛出异常来结束执行。

环绕通知是最常用的通知类型。和AspectJ一样，Spring提供所有类型的通知，我们推荐你使用尽可能简单的通知类型来实现需要的功能。例如，如果你只是需要一个方法的返回值来更新缓存，最好使用后置通知而不是环绕通知，尽管环绕通知也能完成同样的事情。用最合适的通知类型可以使得编程模型变得简单，并且能够避免很多潜在的错误。比如，你不需要在JoinPoint上调用用于环绕通知的proceed()方法，就不会有调用的问题。

### AspectJ

AspectJ是一个java实现的AOP框架，它能够对java代码进行AOP编译（一般在编译期进行），让java代码具有AspectJ的AOP功能（当然需要特殊的编译器）

ApectJ采用的是静态织入的方式。ApectJ主要采用的是编译期织入，在这个期间使用AspectJ的acj编译器(类似javac)把aspect类编译成class字节码后，在java目标类编译时织入，即先编译aspect类再编译目标类。

![](.\spring-framework-aop-6.png)

### AOP的配置方式

#### XML Schema配置方式

Spring提供了使用"aop"命名空间来定义一个切面, 我们来看个例子

**定义目标类**

```java
public class AopDemoServiceImpl {

    public void doMethod1() {
        System.out.println("AopDemoServiceImpl.doMethod1()");
    }

    public String doMethod2() {
        System.out.println("AopDemoServiceImpl.doMethod2()");
        return "hello world";
    }

    public String doMethod3() throws Exception {
        System.out.println("AopDemoServiceImpl.doMethod3()");
        throw new Exception("some exception");
    }
}
```

**定义切面类**

```java
public class LogAspect {

    /**
     * 环绕通知.
     *
     * @param pjp pjp
     * @return obj
     * @throws Throwable exception
     */
    public Object doAround(ProceedingJoinPoint pjp) throws Throwable {
        System.out.println("-----------------------");
        System.out.println("环绕通知: 进入方法");
        Object o = pjp.proceed();
        System.out.println("环绕通知: 退出方法");
        return o;
    }

    /**
     * 前置通知.
     */
    public void doBefore() {
        System.out.println("前置通知");
    }

    /**
     * 后置通知.
     *
     * @param result return val
     */
    public void doAfterReturning(String result) {
        System.out.println("后置通知, 返回值: " + result);
    }

    /**
     * 异常通知.
     *
     * @param e exception
     */
    public void doAfterThrowing(Exception e) {
        System.out.println("异常通知, 异常: " + e.getMessage());
    }

    /**
     * 最终通知.
     */
    public void doAfter() {
        System.out.println("最终通知");
    }

}
```

**XML配置AOP**

```xml
...
<context:component-scan base-package="tech.pdai.springframework" />

    <aop:aspectj-autoproxy/>

    <!-- 目标类 -->
    <bean id="demoService" class="tech.pdai.springframework.service.AopDemoServiceImpl">
        <!-- configure properties of bean here as normal -->
    </bean>

    <!-- 切面 -->
    <bean id="logAspect" class="tech.pdai.springframework.aspect.LogAspect">
        <!-- configure properties of aspect here as normal -->
    </bean>

    <aop:config>
        <!-- 配置切面 -->
        <aop:aspect ref="logAspect">
            <!-- 配置切入点 -->
            <aop:pointcut id="pointCutMethod" expression="execution(* tech.pdai.springframework.service.*.*(..))"/>
            <!-- 环绕通知 -->
            <aop:around method="doAround" pointcut-ref="pointCutMethod"/>
            <!-- 前置通知 -->
            <aop:before method="doBefore" pointcut-ref="pointCutMethod"/>
            <!-- 后置通知；returning属性：用于设置后置通知的第二个参数的名称，类型是Object -->
            <aop:after-returning method="doAfterReturning" pointcut-ref="pointCutMethod" returning="result"/>
            <!-- 异常通知：如果没有异常，将不会执行增强；throwing属性：用于设置通知第二个参数的的名称、类型-->
            <aop:after-throwing method="doAfterThrowing" pointcut-ref="pointCutMethod" throwing="e"/>
            <!-- 最终通知 -->
            <aop:after method="doAfter" pointcut-ref="pointCutMethod"/>
        </aop:aspect>
    </aop:config>

    <!-- more bean definitions for data access objects go here -->
</beans>
```

**测试类**

```java
public static void main(String[] args) {
    // create and configure beans
    ApplicationContext context = new ClassPathXmlApplicationContext("aspects.xml");

    // retrieve configured instance
    AopDemoServiceImpl service = context.getBean("demoService", AopDemoServiceImpl.class);

    // use configured instance
    service.doMethod1();
    service.doMethod2();
    try {
        service.doMethod3();
    } catch (Exception e) {
        // e.printStackTrace();
    }
}
```

**输出结果**

```java
-----------------------
环绕通知: 进入方法
前置通知
AopDemoServiceImpl.doMethod1()
环绕通知: 退出方法
最终通知
-----------------------
环绕通知: 进入方法
前置通知
AopDemoServiceImpl.doMethod2()
环绕通知: 退出方法
最终通知
后置通知, 返回值: hello world
-----------------------
环绕通知: 进入方法
前置通知
AopDemoServiceImpl.doMethod3()
最终通知
异常通知, 异常: some exception
```

#### AspectJ注解方式

基于XML的声明式AspectJ存在一些不足，需要在Spring配置文件配置大量的代码信息，为了解决这个问题，Spring 使用了@AspectJ框架为AOP的实现提供了一套注解。

| 注解名称        | 解释                                                         |
| --------------- | ------------------------------------------------------------ |
| @Aspect         | 用来定义一个切面。                                           |
| @pointcut       | 用于定义切入点表达式。在使用时还需要定义一个包含名字和任意参数的方法签名来表示切入点名称，这个方法签名就是一个返回值为void，且方法体为空的普通方法。 |
| @Before         | 用于定义前置通知，相当于BeforeAdvice。在使用时，通常需要指定一个value属性值，该属性值用于指定一个切入点表达式(可以是已有的切入点，也可以直接定义切入点表达式)。 |
| @AfterReturning | 用于定义后置通知，相当于AfterReturningAdvice。在使用时可以指定pointcut / value和returning属性，其中pointcut / value这两个属性的作用一样，都用于指定切入点表达式。 |
| @Around         | 用于定义环绕通知，相当于MethodInterceptor。在使用时需要指定一个value属性，该属性用于指定该通知被植入的切入点。 |
| @After-Throwing | 用于定义异常通知来处理程序中未处理的异常，相当于ThrowAdvice。在使用时可指定pointcut / value和throwing属性。其中pointcut/value用于指定切入点表达式，而throwing属性值用于指定-一个形参名来表示Advice方法中可定义与此同名的形参，该形参可用于访问目标方法抛出的异常。 |
| @After          | 用于定义最终final 通知，不管是否异常，该通知都会执行。使用时需要指定一个value属性，该属性用于指定该通知被植入的切入点。 |
| @DeclareParents | 用于定义引介通知，相当于IntroductionInterceptor (不要求掌握)。 |

Spring AOP的实现方式是动态织入，动态织入的方式是在运行时动态将要增强的代码织入到目标类中，这样往往是通过动态代理技术完成的；**如Java JDK的动态代理(Proxy，底层通过反射实现)或者CGLIB的动态代理(底层通过继承实现)**，Spring AOP采用的就是基于运行时增强的代理技术。

如果目标对象有实现接口，默认使用JDK代理模式

**定义切面**

```java
@EnableAspectJAutoProxy //@EnableAspectJAutoProxy(proxyTargetClass = true) -> 更改为cglib代理
@Component
@Aspect
public class LogAspect {

    /**
     * define point cut.
     */
    @Pointcut("execution(* tech.pdai.springframework.service.*.*(..))")
    private void pointCutMethod() {
    }


    /**
     * 环绕通知.
     *
     * @param pjp pjp
     * @return obj
     * @throws Throwable exception
     */
    @Around("pointCutMethod()")
    public Object doAround(ProceedingJoinPoint pjp) throws Throwable {
        System.out.println("-----------------------");
        System.out.println("环绕通知: 进入方法");
        Object o = pjp.proceed();
        System.out.println("环绕通知: 退出方法");
        return o;
    }

    /**
     * 前置通知.
     */
    @Before("pointCutMethod()")
    public void doBefore() {
        System.out.println("前置通知");
    }


    /**
     * 后置通知.
     *
     * @param result return val
     */
    @AfterReturning(pointcut = "pointCutMethod()", returning = "result")
    public void doAfterReturning(String result) {
        System.out.println("后置通知, 返回值: " + result);
    }

    /**
     * 异常通知.
     *
     * @param e exception
     */
    @AfterThrowing(pointcut = "pointCutMethod()", throwing = "e")
    public void doAfterThrowing(Exception e) {
        System.out.println("异常通知, 异常: " + e.getMessage());
    }

    /**
     * 最终通知.
     */
    @After("pointCutMethod()")
    public void doAfter() {
        System.out.println("最终通知");
    }

}
```

其他同上

#### 切入点（pointcut）的申明规则

| 表达式类型  | 说明                     |
| ----------- | ------------------------ |
| execution   | 定位到目标对象的方法上   |
| within      | 定位到具体的类型上       |
| this        | 代理对象的类型           |
| target      | 目标对象的类型           |
| args        | 参数的类型               |
| @args       | 传入的参数有被该注解修饰 |
| @within     | 类型修饰的注解           |
| @annotation | 方法修饰的注解           |

execution表达式

语法： execution([访问权限类型] 返回值类型 [全限定类名] 方法名(参数名) [抛出的异常类型])

| 符合 | 含有                                                  |
| ---- | ----------------------------------------------------- |
| *    | 0到多个符合                                           |
| ..   | 方法参数中表示任意个参数,用在报名后表示当前包及其子包 |
| +    | 用在类名后表示当前类及其子类,用在接口后表接口及其实现 |

实例：

```java
// 指定切入点为：任意公共方法。
execution(public * *(. .))


// 指定切入点为：任何一个以“set”开始的方法。    
execution(* set *(. .))

// 指定切入点为：定义在service包里的任意类的任意方法。
execution(* com.xyz.service.*.*(. .))

// 指定切入点为：定义在service包或者子包里的任意类的任意方法。“..”出现在类名中时，
// 后面必须跟“*”，表示包、子包下的所有类。
execution(* com.xyz.service. .*.*(. .))

// 指定只有一级包下的serivce子包下所有类(接口)中的所有方法为切入点
execution(* *.service.*.*(. .))

// 指定所有包下的serivce子包下所有类(接口)中的所有方法为切入点
execution(* *. .service.*.*(. .))

// 在service包中的任意连接点（在Spring AOP中只是方法执行）：
within（com.xyz.service.*）

// 在service包或其子包中的任意连接点（在Spring AOP中只是方法执行）：
within（com.xyz.service..*）

// 实现了AccountService接口的代理对象的任意连接点 （在Spring AOP中只是方法执行）：
this（com.xyz.service.AccountService）// 'this'在绑定表单中更加常用

// 实现AccountService接口的目标对象的任意连接点 （在Spring AOP中只是方法执行）：
target（com.xyz.service.AccountService） // 'target'在绑定表单中更加常用

// 任何一个只接受一个参数，并且运行时所传入的参数是Serializable 接口的连接点（在Spring AOP中只是方法执行）
args（java.io.Serializable） // 'args'在绑定表单中更加常用; 请注意在例子中给出的切入点不同于 execution(* *(java.io.Serializable))： args版本只有在动态运行时候传入参数是Serializable时才匹配，而execution版本在方法签名中声明只有一个 Serializable类型的参数时候匹配。

// 目标对象中有一个 @Transactional 注解的任意连接点 （在Spring AOP中只是方法执行）
@target（org.springframework.transaction.annotation.Transactional）// '@target'在绑定表单中更加常用

// 任何一个目标对象声明的类型有一个 @Transactional 注解的连接点 （在Spring AOP中只是方法执行）：
@within（org.springframework.transaction.annotation.Transactional） // '@within'在绑定表单中更加常用

// 任何一个执行的方法有一个 @Transactional 注解的连接点 （在Spring AOP中只是方法执行）
@annotation（org.springframework.transaction.annotation.Transactional） // '@annotation'在绑定表单中更加常用

// 任何一个只接受一个参数，并且运行时所传入的参数类型具有@Classified 注解的连接点（在Spring AOP中只是方法执行）
@args（com.xyz.security.Classified） // '@args'在绑定表单中更加常用

// 任何一个在名为'tradeService'的Spring bean之上的连接点 （在Spring AOP中只是方法执行）
bean（tradeService）

// 任何一个在名字匹配通配符表达式'*Service'的Spring bean之上的连接点 （在Spring AOP中只是方法执行）
bean（*Service）
```

此外Spring 支持如下三个逻辑运算符来组合切入点表达式

```java
&& ：要求连接点同时匹配两个切入点表达式
|| ：要求连接点匹配任意个切入点表达式
!: ：要求连接点不匹配指定的切入点表达式
```

### Spring AOP 和 AspectJ 之间的关键区别？

AspectJ可以做Spring AOP干不了的事情，**它是AOP编程的完全解决方案，Spring AOP则致力于解决企业级开发中最普遍的AOP**（方法织入）。

| Spring AOP                                       | AspectJ                                                      |
| ------------------------------------------------ | ------------------------------------------------------------ |
| 在纯 Java 中实现                                 | 使用 Java 编程语言的扩展实现                                 |
| 不需要单独的编译过程                             | 除非设置 LTW，否则需要 AspectJ 编译器 (ajc)                  |
| 只能使用运行时织入                               | 运行时织入不可用。支持编译时、编译后和加载时织入             |
| 功能不强-仅支持方法级编织                        | 更强大 - 可以编织字段、方法、构造函数、静态初始值设定项、最终类/方法等......。 |
| 只能在由 Spring 容器管理的 bean 上实现           | 可以在所有域对象上实现                                       |
| 仅支持方法执行切入点                             | 支持所有切入点                                               |
| 代理是由目标对象创建的, 并且切面应用在这些代理上 | 在执行应用程序之前 (在运行时) 前, 各方面直接在代码中进行织入 |
| 比 AspectJ 慢多了                                | 更好的性能                                                   |
| 易于学习和应用                                   | 相对于 Spring AOP 来说更复杂                                 |

## Spring事务管理

### JdbcTemplate

Jdbc模板，基于Jdbc封装的组件

- The Spring JDBC template allows to clean-up the resources automatically, e.g. release the database connections.
- The Spring JDBC template converts the standard JDBC SQLExceptions into RuntimeExceptions. This allows the programmer to react more flexible to the errors. The Spring JDBC template converts also the vendor specific error messages into better understandable error messages.

创建对应的业务类

```java
public class User {

    private Integer id;

    private String name;

    private Integer age;
    //getter, setters
}
```

dao

```java
public interface IUserDao {

    public Integer addUser(User user);

    public List<User> query();
}
```

```java
@Repository
public class UserDaoImpl implements IUserDao {
    @Autowired
    private JdbcTemplate template;
    private String sql;

    public Integer addUser(User user) {
        sql = "insert into users(name,age)values(?,?)";
        return template.update(sql,user.getName(),user.getAge());
    }

    public List<User> query() {
        sql = "select * from users";
        List<User> users = template.query(sql, new RowMapper<User>() {
            /**
         * 我们自己手动将查询出来的结果集和Java对象中的属性做映射
         * @param resultSet
         * @param i
         * @return
         */
            public User mapRow(ResultSet resultSet, int i) {
                User user = new User();
                try {
                    user.setId(resultSet.getInt("id"));
                    user.setName(resultSet.getString("name"));
                    user.setAge(resultSet.getInt("age"));
                } catch (SQLException e) {
                    e.printStackTrace();
                }
                return user;
            }
        });
        return users;
    }
}
```
service

```java
public interface IUserService {

    public Integer addUser(User user);

    public List<User> query();
}
```

```java
@Service
public class UserServiceImpl implements IUserService {

    @Autowired
    private IUserDao dao;

    public Integer addUser(User user) {
        return dao.addUser(user);
    }

    public List<User> query() {
        return dao.query();
    }
}
```

Java配置及测试

```java
@Configuration
@ComponentScan
public class JavaConfig {


    @Bean
    public DataSource dataSource(){
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/gp?serverTimezone=UTC&characterEncoding=utf8&useUnicode=true&useSSL=false");
        dataSource.setUsername("root");
        dataSource.setPassword("123456");
        return dataSource;
    }

    @Bean
    @DependsOn("dataSource")
    public JdbcTemplate template(DataSource dataSource){
        return new JdbcTemplate(dataSource);
    }

    public static void main(String[] args) {
        ApplicationContext ac = new AnnotationConfigApplicationContext(JavaConfig.class);
        IUserService bean = ac.getBean(IUserService.class);
        //插入数据库
        User user  = new User();
        user.setName("Mic");
        user.setAge(18);
        bean.addUser(user);
        //从数据库查询
        System.out.println(bean.query());
    }
}
```

### 事务管理

事务必须满足ACID原则：

![](.\ACID.png)

#### 基于Jdk动态代理实现事务管理

公共代码管理

```java
public class DbUtils {
    private static final String DRIVERNAME = "com.mysql.cj.jdbc.Driver";
    private static final String URL="jdbc:mysql://localhost:3306/gp?serverTimezone=UTC&characterEncoding=utf8&useUnicode=true&useSSL=false";
    private static final String USERNAME = "root";
    private static final String PASSWORD = "123456";

    private static Connection conn = null;

    public static Connection getConnection(){
        if(conn == null){
            try {
                Class.forName(DRIVERNAME);
                conn = DriverManager.getConnection(URL,USERNAME,PASSWORD);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
        return conn;
    }
}
```

Dao代码新增

```java
...
public Integer addUser1(String userName, String password) throws Exception {
        /*sql = "insert into t_user(username,password1)values(?,?)";
        return template.update(sql,userName,password);*/
        int count = 0;
        sql = "insert into t_user(username1,password)values(?,?)";
        ps = DbUtils.getConnection().prepareStatement(sql);
        ps.setString(1,userName);
        ps.setString(2,password);
        count = ps.executeUpdate();
        return count;
    }
...
```



业务实现管理新增

```java
...
    public Integer business(User user, String userName, String password) throws Exception{
        dao.addUser(user);
        dao.addUser1(userName,password);
        return null;
    }
...
```

代理模式实现

```java
public class TestMain {
    public static void main(String[] args) {

        // 目标对象
        final IUserService target = new UserServiceImpl();
        // 获取代理对象
        IUserService proxy = (IUserService) Proxy.newProxyInstance(JavaConfig.class.getClassLoader(), target.getClass().getInterfaces(), new InvocationHandler() {
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                Connection conn = null;
                try{
                    conn = DbUtils.getConnection();
                    conn.setAutoCommit(false);
                    method.invoke(target,args[0],args[1],args[2]); // 目标对象的执行
                    System.out.println("提交成功...");
                    conn.commit();
                }catch (Exception e){
                    System.out.println("执行失败，数据回滚");
                    conn.rollback();
                }finally {
                    conn.close();
                }

                return null;
            }
        });
        User user  = new User();
        user.setName("波波2");
        user.setAge(18);

        try {
            proxy.business(user,"root2","11111");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

#### 基于AspectJ来自定义实现事务的处理

定义自定义注解

```java
@Retention(RetentionPolicy.RUNTIME)
public @interface GupaoTx {
}
```

在需要AOP支持的方法上添加该注解

```java
@GupaoTx
public Integer business(User user, String userName, String password) throws Exception{
    dao.addUser(user);
    dao.addUser1(userName,password);
    return null;
}
```

自定义切面类

```java
@Aspect
@Component
public class GupaoAspect {

    /**
     * 事务处理的前置方法
     */
    @Before(value = "@annotation(com.gupaoedu.annotation.GupaoTx)")
    public void beforeTx() throws SQLException {
        Connection conn = DbUtils.getConnection();
        conn.setAutoCommit(false);
    }

    /**
     * 后置通知
     * @throws SQLException
     */
    @AfterReturning("@annotation(com.gupaoedu.annotation.GupaoTx)")
    public void afterReturningTx() throws SQLException {
        Connection conn = DbUtils.getConnection();
        System.out.println("提交成功...");
        conn.commit();
    }

    /**
     * 最终通知
     * @throws SQLException
     */
    @After("@annotation(com.gupaoedu.annotation.GupaoTx)")
    public void afterTx() throws SQLException {
        Connection conn = DbUtils.getConnection();
        //conn.close();
    }

    @AfterThrowing("@annotation(com.gupaoedu.annotation.GupaoTx)")
    public void afterThrowing() throws SQLException {
        Connection conn = DbUtils.getConnection();
        System.out.println("失败回滚....");
        conn.rollback();
    }

}
```

测试代码

```java
public class JavaConfigMain {

    public static void main(String[] args) throws Exception {
        ApplicationContext ac = new AnnotationConfigApplicationContext(JavaConfig.class);
        IUserService bean = ac.getBean(IUserService.class);
        User user = new User();
        user.setName("James2");
        user.setAge(22);
        bean.business(user,"abc2","123321");
    }
}
```

### Spring中事务管理

#### 基于xml文件的形式

```xml
 <context:component-scan base-package="com.gupaoedu.*" />

    <context:property-placeholder location="db.properties"/>
    <!-- 配置数据源 -->
    <bean class="org.springframework.jdbc.datasource.DriverManagerDataSource" id="dataSource">
        <property name="url" value="${jdbc_url}"/>
        <property name="driverClassName" value="${jdbc_driver}"/>
        <property name="username" value="${jdbc_username}"/>
        <property name="password" value="${jdbc_password}"/>
    </bean>

    <!-- 配置JdbcTemplate -->
    <bean class="org.springframework.jdbc.core.JdbcTemplate" >
        <constructor-arg name="dataSource" ref="dataSource"/>
    </bean>

    <bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <tx:advice id="txAdvice" transaction-manager="txManager">
        <!-- the transactional semantics... -->
        <tx:attributes>
            <!-- all methods starting with 'get' are read-only -->
            <tx:method name="bus*" propagation="REQUIRED"/>
            <!-- other methods use the default transaction settings (see below) -->
            <tx:method name="*"/>
        </tx:attributes>
    </tx:advice>

    <aop:config>
        <!-- insert around every method under the service class -->
        <aop:pointcut id="tx" expression="execution(* com.gupaoedu.service..*.*(..))"/>
        <aop:advisor advice-ref="txAdvice" pointcut-ref="tx"/>
    </aop:config>

</beans>
```

#### 基于注解的方式

```java
@Configuration
@ComponentScan
@EnableTransactionManagement
public class JavaConfig {


    @Bean
    public DataSource dataSource(){
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/gp?serverTimezone=UTC&characterEncoding=utf8&useUnicode=true&useSSL=false");
        dataSource.setUsername("root");
        dataSource.setPassword("123456");
        return dataSource;
    }

    @Bean
    @DependsOn("dataSource")
    public JdbcTemplate template(DataSource dataSource){
        return new JdbcTemplate(dataSource);
    }
    
    @Bean
    @DependsOn("dataSource")
     public PlatformTransactionManager txManager() {
         return new DataSourceTransactionManager(dataSource());
     }
}
```

在需要使用注解的方法获取类的头部添加对应的注解 @Transactional

```java
@Transactional
public void business() throws Exception{
    User user = new User("lisi",24);
    addUser(user);
    user.setId(13);
    user.setUserName("bbbb");
    updateUser(user);
}
```

### 事务的传播行为

| 事务行为                  | 说明                                                         |
| :------------------------ | :----------------------------------------------------------- |
| PROPAGATION_REQUIRED      | 支持当前事务，假设当前没有事务。就新建一个事务               |
| PROPAGATION_SUPPORTS      | 支持当前事务，假设当前没有事务，就以非事务方式运行           |
| PROPAGATION_MANDATORY     | 支持当前事务，假设当前没有事务，就抛出异常                   |
| PROPAGATION_REQUIRES_NEW  | 新建事务，假设当前存在事务。把当前事务挂起                   |
| PROPAGATION_NOT_SUPPORTED | 以非事务方式运行操作。假设当前存在事务，就把当前事务挂起     |
| PROPAGATION_NEVER         | 以非事务方式运行，假设当前存在事务，则抛出异常               |
| PROPAGATION_NESTED        | 如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与PROPAGATION_REQUIRED类似的操作。 |

**举例说明**

ServiceA

```java
ServiceA {   
     void methodA() {
         ServiceB.methodB();
     }
}
```

ServiceB

```java
ServiceB { 
     void methodB() {
     }      
}
```

#### PROPAGATION_REQUIRED

假如当前正要运行的事务不在另外一个事务里，那么就起一个新的事务 比方说，ServiceB.methodB的事务级别定义PROPAGATION_REQUIRED, 那么因为执行ServiceA.methodA的时候，ServiceA.methodA已经起了事务。这时调用ServiceB.methodB，ServiceB.methodB看到自己已经执行在ServiceA.methodA的事务内部，就不再起新的事务。

而假如ServiceA.methodA执行的时候发现自己没有在事务中，他就会为自己分配一个事务。这样，在ServiceA.methodA或者在ServiceB.methodB内的不论什么地方出现异常。事务都会被回滚。即使ServiceB.methodB的事务已经被提交，可是ServiceA.methodA在接下来fail要回滚，ServiceB.methodB也要回滚

![](.\Propagation_Requiired.png)

#### PROPAGATION_SUPPORTS

假设当前在事务中。即以事务的形式执行。假设当前不在一个事务中，那么就以非事务的形式执行

#### PROPAGATION_MANDATORY

必须在一个事务中执行。也就是说，他仅仅能被一个父事务调用。否则，他就要抛出异常

#### PROPAGATION_REQUIRES_NEW

这个就比较绕口了。 比方我们设计ServiceA.methodA的事务级别为PROPAGATION_REQUIRED，ServiceB.methodB的事务级别为PROPAGATION_REQUIRES_NEW。那么当运行到ServiceB.methodB的时候，ServiceA.methodA所在的事务就会挂起。ServiceB.methodB会起一个新的事务。等待ServiceB.methodB的事务完毕以后，他才继续运行。

他与PROPAGATION_REQUIRED 的事务差别在于事务的回滚程度了。由于ServiceB.methodB是新起一个事务，那么就是存在两个不同的事务。假设ServiceB.methodB已经提交，那么ServiceA.methodA失败回滚。ServiceB.methodB是不会回滚的。假设ServiceB.methodB失败回滚，假设他抛出的异常被ServiceA.methodA捕获，ServiceA.methodA事务仍然可能提交。

![](.\Propagation_Required_New.png)

#### PROPAGATION_NOT_SUPPORTED

当前不支持事务。比方ServiceA.methodA的事务级别是PROPAGATION_REQUIRED 。而ServiceB.methodB的事务级别是PROPAGATION_NOT_SUPPORTED ，那么当执行到ServiceB.methodB时。ServiceA.methodA的事务挂起。而他以非事务的状态执行完，再继续ServiceA.methodA的事务。

#### PROPAGATION_NEVER

不能在事务中执行。
如果ServiceA.methodA的事务级别是PROPAGATION_REQUIRED。 而ServiceB.methodB的事务级别是PROPAGATION_NEVER ，那么ServiceB.methodB就要抛出异常了。 

#### PROPAGATION_NESTED

如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与PROPAGATION_REQUIRED类似的操作。

### 设置传播行为

#### xml

```java
<tx:advice id="txAdvice" transaction-manager="txManager"> 
      <tx:attributes>  
      <!--设置所有匹配的方法，然后设置传播级别和事务隔离-->
           <tx:method name="save*" propagation="REQUIRED" /> 
           <tx:method name="add*" propagation="REQUIRED" /> 
           <tx:method name="create*" propagation="REQUIRED" /> 
           <tx:method name="insert*" propagation="REQUIRED" /> 
           <tx:method name="update*" propagation="REQUIRED" /> 
           <tx:method name="merge*" propagation="REQUIRED" /> 
           <tx:method name="del*" propagation="REQUIRED" /> 
           <tx:method name="remove*" propagation="REQUIRED" /> 
           <tx:method name="put*" propagation="REQUIRED" /> 
           <tx:method name="get*" propagation="SUPPORTS" read-only="true" /> 
           <tx:method name="count*" propagation="SUPPORTS" read-only="true" /> 
          <tx:method name="find*" propagation="SUPPORTS" read-only="true" /> 
          <tx:method name="list*" propagation="SUPPORTS" read-only="true" /> 
          <tx:method name="*" propagation="SUPPORTS" read-only="true" /> 
     </tx:attributes> 
</tx:advice> 
```

#### 注解

```java
<!--开启注解的方式--> 
<tx:annotation-driven transaction-manager="transactioManager" />
```

### Spring支持的隔离级别

事务隔离级别指的是一个事务对数据的修改与另一个并行的事务的隔离程度，当多个事务同时访问相同数据时，如果没有采取必要的隔离机制，就可能发生以下问题：

| 问题       | 描述                                                         |
| ---------- | :----------------------------------------------------------- |
| 脏读       | **A *dirty read* occurs when a transaction reads data that has not yet been committed.** For example, consider two transactions T1 and T2. T1 modifies a row in a table and starts a transaction, but does not commit it. Meanwhile, T2 tries to read the same row before T1 commits its changes. If T2 is allowed to read the uncommitted data, it will result in a dirty read, potentially leading to incorrect or inconsistent results. |
| 幻读       | **A *phantom* is a row that matches the search criteria but is not initially seen.** For example, suppose transaction 1 reads a set of rows that satisfy some search criteria. Transaction 2 generates a new row (through either an update or an insert) that matches the search criteria for transaction 1. If transaction 1 reexecutes the statement that reads the rows, it gets a different set of rows. |
| 不可重复读 | **A *nonrepeatable read* occurs when a transaction reads the same row twice but gets different data each time.** For example, suppose transaction 1 reads a row. Transaction 2 updates or deletes that row and commits the update or delete. If transaction 1 rereads the row, it retrieves different row values or discovers that the row has been deleted. |

#### 隔离级别

| 隔离级别        | 描述                                                         |
| :-------------- | :----------------------------------------------------------- |
| DEFAULT         | 使用数据库本身使用的隔离级别<br> ORACLE（读已提交） MySQL（可重复读） |
| READ_UNCOMITTED | Transactions are not isolated from each other. If the DBMS supports other transaction isolation levels, it ignores whatever mechanism it uses to implement those levels. So that they do not adversely affect other transactions, transactions running at the Read Uncommitted level are usually read-only. |
| READ_COMMITED   | The transaction waits until rows write-locked by other transactions are unlocked; this prevents it from reading any "dirty" data. <br>The transaction holds a read lock (if it only reads the row) or write lock (if it updates or deletes the row) on the current row to prevent other transactions from updating or deleting it. The transaction releases read locks when it moves off the current row. It holds write locks until it is committed or rolled back. |
| REPEATABLE_READ | The transaction waits until rows write-locked by other transactions are unlocked; this prevents it from reading any "dirty" data.<br>The transaction holds read locks on all rows it returns to the application and write locks on all rows it inserts, updates, or deletes. For example, if the transaction includes the SQL statement **SELECT \* FROM Orders**, the transaction read-locks rows as the application fetches them. If the transaction includes the SQL statement **DELETE FROM Orders WHERE Status = 'CLOSED'**, the transaction write-locks rows as it deletes them.<br>Because other transactions cannot update or delete these rows, the current transaction avoids any nonrepeatable reads. The transaction releases its locks when it is committed or rolled back. |
| SERLALIZABLE    | The transaction waits until rows write-locked by other transactions are unlocked; this prevents it from reading any "dirty" data.<br>Because other transactions cannot update or delete the rows in the range, the current transaction avoids any nonrepeatable reads. Because other transactions cannot insert any rows in the range, the current transaction avoids any phantoms. The transaction releases its lock when it is committed or rolled back. |

| Transaction isolation level | Dirty reads | Nonrepeatable reads | Phantoms |
| :-------------------------- | :---------- | :------------------ | :------- |
| Read uncommitted            | X           | X                   | X        |
| Read committed              | --          | X                   | X        |
| Repeatable read             | --          | --                  | X        |
| Serializable                | --          | --                  | --       |

再必须强调一遍，不是事务隔离级别设置得越高越好，事务隔离级别设置得越高，意味着势必要花手段去加锁用以保证事务的正确性，那么效率就要降低，因此实际开发中往往要在效率和并发正确性之间做一个取舍，一般情况下会设置为READ_COMMITED，此时避免了脏读，并发性也还不错，之后再通过一些别的手段去解决不可重复读和幻读的问题就好了。

## Spring MVC

**控制层框架【接收请求，响应请求】**

Spring MVC 是 Spring 提供的一个基于 MVC 设计模式的轻量级 Web 开发框架，本质上相当于 Servlet。

Spring MVC 是结构最清晰的 Servlet+JSP+JavaBean 的实现，是一个典型的教科书式的 MVC 构架。在 Spring MVC 框架中，Controller 替换 Servlet 来担负控制器的职责，用于接收请求，调用相应的 Model 进行处理，处理器完成业务处理后返回处理结果。Controller 调用相应的 View 并对处理结果进行视图渲染，最终客户端得到响应信息。

### Servlet 简介

#### Servlet 是什么？

Java Servlet 是运行在 Web 服务器或应用服务器上的程序，它是作为来自 Web 浏览器或其他 HTTP 客户端的请求和 HTTP 服务器上的数据库或应用程序之间的中间层。

![](.Boot\servlet-arch.jpg)

#### Servlet 任务

- 读取客户端（浏览器）发送的显式的数据。这包括网页上的 HTML 表单，或者也可以是来自 applet 或自定义的 HTTP 客户端程序的表单。
- 读取客户端（浏览器）发送的隐式的 HTTP 请求数据。这包括 cookies、媒体类型和浏览器能理解的压缩格式等等。
- 处理数据并生成结果。这个过程可能需要访问数据库，执行 RMI 或 CORBA 调用，调用 Web 服务，或者直接计算得出对应的响应。
- 发送显式的数据（即文档）到客户端（浏览器）。该文档的格式可以是多种多样的，包括文本文件（HTML 或 XML）、二进制文件（GIF 图像）、Excel 等。
- 发送隐式的 HTTP 响应到客户端（浏览器）。这包括告诉浏览器或其他客户端被返回的文档类型（例如 HTML），设置 cookies 和缓存参数，以及其他类似的任务。

### 示例

#### 通过maven构建一个web项目

#### 添加对应的依赖及Tomcat插件

```xml
<dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.1.17.RELEASE</version>
    </dependency>
  </dependencies>

  <build>
    <finalName>gp_springmvc_01_hello</finalName>
    <plugins>
      <!-- tomcat插件 -->
      <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.2</version>
        <configuration>
          <!-- 端口号 -->
          <port>8082</port>
          <!-- /表示访问路径 省略项目名 -->
          <path>/</path>
          <!-- 设置编码方式 -->
          <uriEncoding>utf-8</uriEncoding>
        </configuration>
      </plugin>
    </plugins>
```

#### 创建SpringMVC的配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 处理器映射器 -->
    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping" />

    <!-- 处理器适配器 -->
    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter" />

</beans>
```

#### 在web.xml中注册DispatchServlet

```xml
<web-app>
  <display-name>Archetype Created Web Application</display-name>
  <!--  注册前端控制器-->
  <servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <!-- 关联配置文件 -->
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:spring-mvc.xml</param-value>
    </init-param>
  </servlet>
  <servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>
```

#### 创建自定义的Controller

```java
public class UserController implements Controller {

    /**
     * 处理请求的方法
     * @param httpServletRequest
     * @param httpServletResponse
     * @return
     * @throws Exception
     */
    @Override
    public ModelAndView handleRequest(HttpServletRequest httpServletRequest
            , HttpServletResponse httpServletResponse) throws Exception {
        System.out.println("controller 执行了....");
        ModelAndView view = new ModelAndView();
        view.setViewName("/index.jsp");
        return view;
    }
}
```

#### 在Springmvc配置文件中注册

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 处理器映射器 -->
    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping" />

    <!-- 处理器适配器 -->
    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter" />

    <!-- 注册自定义的Controller name必须加 / 前缀-->
    <bean class="com.gupaoedu.UserController" name="/user"/>
</beans>
```

### SpringMVC基于注解的使用方式

#### SpringMVC配置文件修改

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc.xsd
">

    <!-- 开启扫描 -->
    <context:component-scan base-package="com.gupaoedu.controller" />
    <!-- 开启SpringMVC注解的使用 -->
    <mvc:annotation-driven />

</beans>
```

#### 自定义控制器

```java
![SpringMVC](.\SpringMVC.png)@Controller // 将UserController对象交给IoC容器管理
@RequestMapping("/user") // 类头部的可以省略
public class UserController  {


    /**
     * 具体处理请求的方法  【头部的mapping+方法的mapping】
     * http://localhost:8082/user/query
     * @return
     */
    @RequestMapping("/query")
    public String query(){
        System.out.println("query ..... ");
        return "/index.jsp";
    }

    @RequestMapping("/save")
    public String add(){
        System.out.println("save ..... ");
        return "/index.jsp";
    }

    @RequestMapping("/delete")
    public String delete(){
        System.out.println("delete ..... ");
        return "/index.jsp";
    }

    @RequestMapping("/update")
    public String update(){
        System.out.println("update ..... ");
        return "/index.jsp";
    }
}
```

### SpringMVC工作原理图解

![](.\SpringMVC.png)

### SpringMVC中响应请求

#### 响应返回字符串

我们可以在处理方法的最后返回一个要跳转的页面地址”/“不要漏了

```java
@RequestMapping("/query")
public String query(){
    System.out.println("query ..... ");
    return "/index.jsp";
}
```

#### 不响应

如果用户提交了请求后服务端不需要给客户端一个响应，那么我们可以指定返回类型为`void`同时在方法头部添加`@ResponseBody`注解即可

```java
@RequestMapping("/save")
@ResponseBody
public void add(){
    System.out.println("save ..... ");
}
```

#### 直接返回一个ModelAndView对象

```java
@RequestMapping("/delete")
public ModelAndView delete(){
    System.out.println("delete ..... ");
    ModelAndView mm = new ModelAndView();
    mm.setViewName("/index.jsp");
    return mm;
}
```

#### 重定向跳转

```java
@RequestMapping("/update")
public String update(){
    System.out.println("update ..... ");
    return "redirect:/user/query";
}
```

#### 视图解析器添加前后缀

```java
<!-- 配置视图解析器 -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" >
    <property name="suffix" value=".jsp"/>
    <property name="prefix" value="/" />
</bean>
```

那这样的话响应的页面就会自动添加对应的前后缀信息

```java
@RequestMapping("/query")
public String query(){
    System.out.println("query ..... ");
    return "index";
}
```

#### 通过HttpServletResponse响应

仅仅只需要在方法的形参中声明这两个变量即可~

```java
@RequestMapping("/fun1")
public void fun1(HttpServletRequest request
                 ,HttpServletResponse response) throws Exception {
    // response.sendRedirect("/index.jsp");
    System.out.println("fun1 ...");
    request.getRequestDispatcher("/index.jsp").forward(request,response);
}
```

### SpringMVC接收请求数据

#### 基本数据类型

直接在形参中声明要接收的数据，默认情况下形参必须和传过来的数据参数名一致

```java
@RequestMapping("/query")
//id传过来的名字可以是ids，默认为123
public String query(@RequestParam(value = "ids",required = true,defaultValue = "123") Integer id, String name){
    System.out.println("query ..... " + id + ":" + name);
    return "/index.jsp";
}
```

#### 对象接收

如果传递过来的数据比较多，那么我们可以通过一个自定义的对象来接收

```java
@RequestMapping("/save")
public String add(User user){
    System.out.println("save ..... " + user);
    return "/index.jsp";
}
```

#### 通过数组接收

如果有多个名称相同的数据提交，我们可以使用数组的方式来接收

```java
@RequestMapping("/delete")
public String delete(String[] loves){
    System.out.println("delete ..... ");
    if(loves !=null){
        for(String l : loves){
            System.out.println(l);
        }
    }
    return "/index.jsp";
}
```

#### 自定义转换器

有时候客户端传递过来特殊类型的数据，SpringMVC中提供的默认的转换器不能支持该转换，此时我们就需要自定义转换器

```java
public class DateConvert implements Converter<String,Date> {
    @Override
    public Date convert(String s) {
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
        try {
            return sdf.parse(s);
        } catch (ParseException e) {
            e.printStackTrace();
        }
        return null;
    }
}
```

配置文件中注册

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc.xsd
">

    <!-- 开启扫描 -->
    <context:component-scan base-package="com.gupaoedu.controller" />
    <!-- 开启SpringMVC注解的使用 -->
    <mvc:annotation-driven conversion-service="formattingConversionServiceFactoryBean"/>

    <!-- 配置转换器 -->
    <bean class="org.springframework.format.support.FormattingConversionServiceFactoryBean"
    id="formattingConversionServiceFactoryBean">
        <property name="converters">
            <set>
                <bean class="com.gupaoedu.convert.DateConvert"/>
            </set>
        </property>
    </bean>
</beans>
```

使用

```java
@RequestMapping("/date")
public String delete(Date date){
    System.out.println(date);
    return "/index.jsp";
}
```

### 响应数据

#### ModelAndView传递

```java
@RequestMapping("/query")
public ModelAndView query(){
    System.out.println("query ..... ");
    ModelAndView mm = new ModelAndView();
    mm.setViewName("/user.jsp");
    mm.addObject("msg","msg.....");
    return mm;
}
```

然后在jsp页面中通过EL表达式获取传递的信息

```html
<%--
  Created by IntelliJ IDEA.
  User: admin
  Date: 2020/8/26
  Time: 16:29
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
  hello<br>
  ${msg}
</body>
</html>
```

#### 通过Map对象传值

```java
@RequestMapping("/save")
public String add(Map<String,Object> map){
    System.out.println("save ..... ");
    map.put("msg","map ...msg");
    return "/index.jsp";
}
```

jsp页面同上

#### 通过Model来接收

```java
@RequestMapping("/delete")
public String delete(Model model){
    System.out.println("delete ..... ");
    model.addAttribute("msg","model ...msg");
    return "/index.jsp";
}
```

#### 通过ModelMap响应数据

```java
@RequestMapping("/update")
public String update(ModelMap mm){
    System.out.println("update ..... ");
    mm.put("msg","ModelMap ... msg");
    return "/index.jsp";
}
```

前面介绍的多种方式的数据都是会被保存在request作用域中，如果我们同时需要将数据保存在Session对象中，我们只需要在类的头部添加一个@SessionAttributes注解即可

```java
@RequestMapping("/update")
@SessionAttributes({"msg"})
public String update(ModelMap mm){
    System.out.println("update ..... ");
    mm.put("msg","ModelMap ... msg");
    return "/index.jsp";
}
```

#### 静态资源处理

默认的情况下在SpringMVC中只能访问jsp页面，其他的都会被`DispatchServlet`拦截，原因是`DispatchServlet`配置的时候用的`/` 覆盖掉了`default`servlet所做的工作，所以我们只需要重新制定即可

```xml
  <filter-mapping>
    <filter-name>encodeFiletr</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
  <servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>*.png</url-pattern>
  </servlet-mapping>
  <servlet-mapping>
    <servlet-name>default</servlet-name>
    <url-pattern>*.html</url-pattern>
  </servlet-mapping>
```

第二种方式就是在SpringMVC中指定映射的规则

```java
<mvc:resources mapping="/img/**" location="/img/"  />
```

### SpringMVC服务端验证

> 最早的校验，就是服务端校验。早期的网站，用户输入一个邮箱地址，校验邮箱地址需要将地址发送到服务端，服务端进行校验，校验成功后，给前端一个响应。有了JavaScript，校验工作可以放在前端去执行。那么为什么还需要服务端校验呢？ 因为前端传来的数据不可信。前端很容易获取都后端的数据接口，如果有人绕过页面，就会出现非法数据，所以服务端也要数据校验，总的来说：
> 1.前端校验要做，目的是为了提高用户体验
> 2.后端校验也要做，目的是为了数据安全 

SpringMVC本身是没有提供校验框架的，我们需要使用Hibernate提供的校验框架

```xml
<dependency>
  	<groupId>org.hibernate</groupId>
  	<artifactId>hibernate-validator</artifactId>
  	<version>5.3.0.Alpha1</version>
</dependency>
```

在配置文件中注册对应的校验框架

```xml
<beans><!-- 省略schema -->
<!-- 开启扫描 -->
    <context:component-scan base-package="com.gupaoedu.controller" />
    <!-- 开启SpringMVC注解的使用 -->
    <mvc:annotation-driven validator="localValidatorFactoryBean" />
    <mvc:resources mapping="/img/**" location="/img/"  />
    <!--  文件上传的解析器-->
    <bean class="org.springframework.web.multipart.commons.CommonsMultipartResolver" id="multipartResolver">
        <property name="maxUploadSize">
            <value>5242880</value>
        </property>
    </bean>

    <!-- 配置验证框架 -->
    <bean class="org.springframework.validation.beanvalidation.LocalValidatorFactoryBean" id="localValidatorFactoryBean">
        <property name="providerClass" value="org.hibernate.validator.HibernateValidator"/>
        <property name="validationMessageSource" ref="messageSource" />
    </bean>

    <bean class="org.springframework.context.support.ReloadableResourceBundleMessageSource" id="messageSource">
        <property name="basename" value="classpath:volidata.properties"/>
        <property name="fileEncodings" value="utf-8"/>
        <property name="cacheSeconds" value="120"/>
    </bean>

</beans>
```

#### 验证规则

| 注解                        | 说明                                                     |
| :-------------------------- | :------------------------------------------------------- |
| @Null                       | 被注解的元素必须为 null                                  |
| @NotNull                    | 被注解的元素必须不为 null                                |
| @AssertTrue                 | 被注解的元素必须为 true                                  |
| @AssertFalse                | 被注解的元素必须为 false                                 |
| @Min(value)                 | 被注解的元素必须是一个数字，其值必须大于等于指定的最小值 |
| @Max(value)                 | 被注解的元素必须是一个数字，其值必须小于等于指定的最大值 |
| @DecimalMin(value)          | 被注解的元素必须是一个数字，其值必须大于等于指定的最小值 |
| @DecimalMax(value)          | 被注解的元素必须是一个数字，其值必须小于等于指定的最大值 |
| @Size(max=, min=)           | 被注解的元素的大小必须在指定的范围内                     |
| @Digits (integer, fraction) | 被注解的元素必须是一个数字，其值必须在可接受的范围内     |
| @Past                       | 被注解的元素必须是一个过去的日期                         |
| @Future                     | 被注解的元素必须是一个将来的日期                         |
| @Pattern(regex=,flag=)      | 被注解的元素必须符合指定的正则表达式                     |
| @NotBlank(message =)        | 验证字符串非null，且长度必须大于0                        |
| @Email                      | 被注解的元素必须是电子邮箱地址                           |
| @Length(min=,max=)          | 被注解的字符串的大小必须在指定的范围内                   |
| @NotEmpty                   | 被注解的字符串的必须非空                                 |
| @Range(min=,max=,message=)  | 被注解的元素必须在合适的范围内                           |

在自定义的对象中指定验证规则

```java
public class User {


    private Integer id;

    @NotNull(message = "账号不能为空")
    @Length(message = "账号的长度必须在3~6位" ,max = 6,min = 3)
    private String userName;

    @Max(message = "age最大值是120",value = 120)
    @Min(message = "age必须大于0" ,value = 0)
    private Integer age;

    //getters and setters
}
```

在控制层中使用校验规则

```java
@RequestMapping("/save")
public String add(@Validated User user, BindingResult br) throws IOException {
    System.out.println("save ....");
    List<ObjectError> allErrors = br.getAllErrors();
    for (ObjectError error:allErrors){
        System.out.println(error.getDefaultMessage());
    }
    return "/index.jsp";
}
```

#### 分组验证

分组验证解决的是不同的同一个POJO对象在不同的场景用适用不同的验证规则

定义分组

![](.\SpringMVC_Validator_Grouping.png)

验证规则和分组绑定

```java
public class User {


    @NotBlank(message = "ID不能为空" ,groups = {GroupInterface1.class})
    private Integer id;

    @NotBlank(message = "{user.username.empty}" ,groups = {GroupInterface1.class,GroupInterface2.class})
    @Length(message = "账号的长度必须在3~6位" ,max = 6,min = 3,groups = {GroupInterface1.class,GroupInterface2.class})
    private String userName;

    @Max(message = "age最大值是120",value = 120,groups = {GroupInterface1.class,GroupInterface2.class})
    @Min(message = "age必须大于0" ,value = 0,groups = {GroupInterface1.class,GroupInterface2.class})
    private Integer age;
    //getters, setters
}
```

应用

```java
@RequestMapping("/save")
public String add(@Validated({GroupInterface2.class}) User user, BindingResult br, Model model) throws IOException {
    System.out.println("save ....");
    List<ObjectError> allErrors = br.getAllErrors();
    for (ObjectError error:allErrors){
        System.out.println(error.getDefaultMessage());
    }
    return "/index.jsp";
}

@RequestMapping("/update")
public String update(@Validated({GroupInterface1.class}) User user, BindingResult br, Model model) throws IOException {
    System.out.println("update ....");
    List<ObjectError> allErrors = br.getAllErrors();
    for (ObjectError error:allErrors){
        System.out.println(error.getDefaultMessage());
    }
    return "/index.jsp";
}
```

### Restful风格

Restful是一种设计风格，是一个规范，不是一个技术。

| 提交方式   | 地址                         | 说明                  |
| ---------- | ---------------------------- | --------------------- |
| GET(查)    | http://localhost:8080/book/1 | 查询id为1的书         |
| POST(增)   | http://localhost:8080/book/1 | 添加一本书，书的id为1 |
| DELETE(删) | http://localhost:8080/book/1 | 删除id为1的书         |
| PUT(改)    | http://localhost:8080/book/1 | 修改id为1的书         |

控制器处理

```java
@RestController
public class UserController  {

    /**
     * 具体处理请求的方法  【头部的mapping+方法的mapping】
     * http://localhost:8082/user/query
     * @return
     */
    @GetMapping("/user/{id}/{name}")
    public List<User> query(@PathVariable Integer id,@PathVariable String name){
        System.out.println("query ..... " + id + " " + name);
        return Arrays.asList(new User(1,"zhangsan1",18)
        ,new User(2,"zhangsan2",19)
        ,new User(3,"zhangsan3",12));
    }

    @PostMapping("/user")
    public String add(@RequestBody User user){
        System.out.println("save ..... " + user);
        return "/index.jsp";
    }

    @DeleteMapping("/user")
    public String delete(){
        System.out.println("delete ..... ");
        return "/index.jsp";
    }

    @PutMapping("/user")
    public String update(){
        System.out.println("update ..... ");
        return "/index.jsp";
    }
}
```



# Spring框架核心原理

## IoC设计思想与Spring IoC实现

![](.\spring-framework-ioc-source-7.png)

### BeanFactory和BeanRegistry：IOC容器功能规范和Bean的注册

> Spring Bean的创建是典型的工厂模式，这一系列的Bean工厂，也即IOC容器为开发者管理对象间的依赖关系提供了很多便利和基础服务，在Spring中有许多的IOC容器的实现供用户选择和使用，这是IOC容器的基础；在顶层的结构设计主要围绕着BeanFactory和xxxRegistry进行：
>
> - **BeanFactory： 工厂模式定义了IOC容器的基本功能规范**
> - **BeanRegistry： 向IOC容器手工注册 BeanDefinition 对象的方法**

####  BeanFactory定义了IOC 容器基本功能规范？

**BeanFactory作为最顶层的一个接口类，它定义了IOC容器的基本功能规范**，BeanFactory 有三个子类：ListableBeanFactory、HierarchicalBeanFactory 和AutowireCapableBeanFactory。

```java
public interface BeanFactory {    
      
    //用于取消引用实例并将其与FactoryBean创建的bean区分开来。例如，如果命名的bean是FactoryBean，则获取将返回Factory，而不是Factory返回的实例。
    String FACTORY_BEAN_PREFIX = "&"; 
        
    //根据bean的名字和Class类型等来得到bean实例    
    Object getBean(String name) throws BeansException;    
    Object getBean(String name, Class requiredType) throws BeansException;    
    Object getBean(String name, Object... args) throws BeansException;
    <T> T getBean(Class<T> requiredType) throws BeansException;
    <T> T getBean(Class<T> requiredType, Object... args) throws BeansException;

    //返回指定bean的Provider
    <T> ObjectProvider<T> getBeanProvider(Class<T> requiredType);
    <T> ObjectProvider<T> getBeanProvider(ResolvableType requiredType);

    //检查工厂中是否包含给定name的bean，或者外部注册的bean
    boolean containsBean(String name);

    //检查所给定name的bean是否为单例/原型
    boolean isSingleton(String name) throws NoSuchBeanDefinitionException;
    boolean isPrototype(String name) throws NoSuchBeanDefinitionException;

    //判断所给name的类型与type是否匹配
    boolean isTypeMatch(String name, ResolvableType typeToMatch) throws NoSuchBeanDefinitionException;
    boolean isTypeMatch(String name, Class<?> typeToMatch) throws NoSuchBeanDefinitionException;

    //获取给定name的bean的类型
    @Nullable
    Class<?> getType(String name) throws NoSuchBeanDefinitionException;

    //返回给定name的bean的别名
    String[] getAliases(String name);
     
}
```

####  BeanFactory为何要定义这么多层次的接口？定义了哪些接口？

主要是为了**区分在 Spring 内部在操作过程中对象的传递和转化过程中，对对象的数据访问所做的限制**。

有哪些接口呢？

- **ListableBeanFactory**：该接口定义了访问容器中 Bean 基本信息的若干方法，如查看Bean 的个数、获取某一类型 Bean 的配置名、查看容器中是否包括某一 Bean 等方法；
- **HierarchicalBeanFactory**：父子级联 IoC 容器的接口，子容器可以通过接口方法访问父容器； 通过 HierarchicalBeanFactory 接口， Spring 的 IoC 容器可以建立父子层级关联的容器体系，子容器可以访问父容器中的 Bean，但父容器不能访问子容器的 Bean。Spring 使用父子容器实现了很多功能，比如在 Spring MVC 中，展现层 Bean 位于一个子容器中，而业务层和持久层的 Bean 位于父容器中。这样，展现层 Bean 就可以引用业务层和持久层的 Bean，而业务层和持久层的 Bean 则看不到展现层的 Bean。
- **ConfigurableBeanFactory**：是一个重要的接口，增强了 IoC 容器的可定制性，它定义了设置类装载器、属性编辑器、容器初始化后置处理器等方法；
- **ConfigurableListableBeanFactory**: ListableBeanFactory 和 ConfigurableBeanFactory的融合；
- **AutowireCapableBeanFactory**：定义了将容器中的 Bean 按某种规则（如按名字匹配、按类型匹配等）进行自动装配的方法；

#### 如何将Bean注册到BeanFactory中？BeanRegistry

Spring 配置文件中每一个`<bean>`节点元素在 Spring 容器里都通过一个 BeanDefinition 对象表示，它描述了 Bean 的配置信息。而 BeanDefinitionRegistry 接口提供了向容器手工注册 BeanDefinition 对象的方法。

#### BeanDefinition：各种Bean对象及其相互的关系

> Bean对象存在依赖嵌套等关系，所以设计者设计了BeanDefinition，它用来对Bean对象及关系定义；我们在理解时只需要抓住如下三个要点：
>
> - **BeanDefinition 定义了各种Bean对象及其相互的关系**
> - **BeanDefinitionReader 这是BeanDefinition的解析器**
> - **BeanDefinitionHolder 这是BeanDefination的包装类，用来存储BeanDefinition，name以及aliases等。**

### ApplicationContext：IOC接口设计和实现

> IoC容器的接口类是ApplicationContext，很显然它必然继承BeanFactory对Bean规范（最基本的ioc容器的实现）进行定义。而ApplicationContext表示的是应用的上下文，除了对Bean的管理外，还至少应该包含了
>
> - **访问资源**： 对不同方式的Bean配置（即资源）进行加载。(实现ResourcePatternResolver接口)
> - **国际化**: 支持信息源，可以实现国际化。（实现MessageSource接口）
> - **应用事件**: 支持应用事件。(实现ApplicationEventPublisher接口)

#### ApplicationContext接口的设计

![](.\spring-framework-ioc-source-51.png)

- **HierarchicalBeanFactory 和 ListableBeanFactory**： ApplicationContext 继承了 HierarchicalBeanFactory 和 ListableBeanFactory 接口，在此基础上，还通过多个其他的接口扩展了 BeanFactory 的功能：
- **ApplicationEventPublisher**：让容器拥有发布应用上下文事件的功能，包括容器启动事件、关闭事件等。实现了 ApplicationListener 事件监听接口的 Bean 可以接收到容器事件 ， 并对事件进行响应处理 。 在 ApplicationContext 抽象实现类AbstractApplicationContext 中，我们可以发现存在一个 ApplicationEventMulticaster，它负责保存所有监听器，以便在容器产生上下文事件时通知这些事件监听者。
- **MessageSource**：为应用提供 i18n 国际化消息访问的功能；
- **ResourcePatternResolver** ： 所 有 ApplicationContext 实现类都实现了类似于PathMatchingResourcePatternResolver 的功能，可以通过带前缀的 Ant 风格的资源文件路径装载 Spring 的配置文件。
- **LifeCycle**：该接口是 Spring 2.0 加入的，该接口提供了 start()和 stop()两个方法，主要用于控制异步处理过程。在具体使用时，该接口同时被 ApplicationContext 实现及具体 Bean 实现， ApplicationContext 会将 start/stop 的信息传递给容器中所有实现了该接口的 Bean，以达到管理和控制 JMX、任务调度等目的。

#### ApplicationContext接口的实现

在考虑ApplicationContext接口的实现时，关键的点在于，不同Bean的配置方式（比如xml,groovy,annotation等）有着不同的资源加载方式，这便衍生除了众多ApplicationContext的实现类。

![](.\spring-framework-ioc-source-61.png)

**第一，从类结构设计上看， 围绕着是否需要Refresh容器衍生出两个抽象类**：

- **GenericApplicationContext**： 是初始化的时候就创建容器，往后的每次refresh都不会更改
- **AbstractRefreshableApplicationContext**： AbstractRefreshableApplicationContext及子类的每次refresh都是先清除已有(如果不存在就创建)的容器，然后再重新创建；AbstractRefreshableApplicationContext及子类无法做到GenericApplicationContext**混合搭配从不同源头获取bean的定义信息**

**第二， 从加载的源来看（比如xml,groovy,annotation等）， 衍生出众多类型的ApplicationContext, 典型比如**:

- **FileSystemXmlApplicationContext**： 从文件系统下的一个或多个xml配置文件中加载上下文定义，也就是说系统盘符中加载xml配置文件。
- **ClassPathXmlApplicationContext**： 从类路径下的一个或多个xml配置文件中加载上下文定义，适用于xml配置的方式。
- **AnnotationConfigApplicationContext**： 从一个或多个基于java的配置类中加载上下文定义，适用于java注解的方式。
- **ConfigurableApplicationContext**： 扩展于 ApplicationContext，它新增加了两个主要的方法： refresh()和 close()，让 ApplicationContext 具有启动、刷新和关闭应用上下文的能力。在应用上下文关闭的情况下调用 refresh()即可启动应用上下文，在已经启动的状态下，调用 refresh()则清除缓存并重新装载配置信息，而调用close()则可关闭应用上下文。这些接口方法为容器的控制管理带来了便利，但作为开发者，我们并不需要过多关心这些方法。

**第三， 更进一步理解**：

***设计者在设计时AnnotationConfigApplicationContext为什么是继承GenericApplicationContext***？ 因为基于注解的配置，是不太会被运行时修改的，这意味着不需要进行动态Bean配置和刷新容器，所以只需要GenericApplicationContext。

而基于XML这种配置文件，这种文件是容易修改的，需要动态性刷新Bean的支持，所以XML相关的配置必然继承AbstractRefreshableApplicationContext； 且存在多种xml的加载方式（位置不同的设计），所以必然会设计出AbstractXmlApplicationContext, 其中包含对XML配置解析成BeanDefination的过程。

## IOC初始化流程

### 如何将Bean从XML配置中解析后放到IoC容器中的？

#### 初始化的入口

对于xml配置的Spring应用，在main()方法中实例化ClasspathXmlApplicationContext即可创建一个IoC容器。我们可以从这个构造方法开始，探究一下IoC容器的初始化过程。

```java
ApplicationContext context = new ClassPathXmlApplicationContext("aspects.xml", "daos.xml", "services.xml");
```

```java
public ClassPathXmlApplicationContext(String... configLocations) throws BeansException {
    this(configLocations, true, (ApplicationContext)null);
}

public ClassPathXmlApplicationContext(String[] configLocations, boolean refresh, @Nullable ApplicationContext parent) throws BeansException {
    // 设置Bean资源加载器
    super(parent);

    // 设置配置路径
    this.setConfigLocations(configLocations);

    // 初始化容器
    if (refresh) {
        this.refresh();
    }
}
```

####  设置资源解析器和环境

调用父类容器AbstractApplicationContext的构造方法(`super(parent)`方法)为容器设置好Bean资源加载器

```java
public AbstractApplicationContext(@Nullable ApplicationContext parent) {
    // 默认构造函数初始化容器id, name, 状态 以及 资源解析器
    this();

    // 将父容器的Environment合并到当前容器
    this.setParent(parent);
}
```

#### 设置配置路径

在设置容器的资源加载器之后，接下来`FileSystemXmlApplicationContext`执行`setConfigLocations`方法通过调用其父类`AbstractRefreshableConfigApplicationContext`的方法进行对Bean定义资源文件的定位

```java
public void setConfigLocations(@Nullable String... locations) {
    if (locations != null) {
        Assert.noNullElements(locations, "Config locations must not be null");
        this.configLocations = new String[locations.length];

        for(int i = 0; i < locations.length; ++i) {
            // 解析配置路径
            this.configLocations[i] = this.resolvePath(locations[i]).trim();
        }
    } else {
        this.configLocations = null;
    }
}
protected String resolvePath(String path) {
    // Environment中包括之前的设置和profiles
    return this.getEnvironment().resolveRequiredPlaceholders(path);
}
```

### 初始化的主体流程

Spring IoC容器对Bean定义资源的载入是从refresh()函数开始的，refresh()是一个模板方法，refresh()方法的作用是：在创建IoC容器前，如果已经有容器存在，则需要把已有的容器销毁和关闭，以保证在refresh之后使用的是新建立起来的IoC容器。refresh的作用类似于对IoC容器的重启，在新建立好的容器中对容器进行初始化，对Bean定义资源进行载入。

![](.\spring-framework-ioc-source-8.png)

AbstractApplicationContext类中只抽象定义了refreshBeanFactory()方法，容器真正调用的是其子类AbstractRefreshableApplicationContext实现的refreshBeanFactory()方法; 在创建IoC容器前，如果已经有容器存在，则需要把已有的容器销毁和关闭，以保证在refresh之后使用的是新建立起来的IoC容器。方法的源码如下：

```java
protected final void refreshBeanFactory() throws BeansException {
    // 如果已经有容器存在，则需要把已有的容器销毁和关闭，以保证在refresh之后使用的是新建立起来的IoC容器
    if (hasBeanFactory()) {
        destroyBeans();
        closeBeanFactory();
    }
    try {
        // 创建DefaultListableBeanFactory，并调用loadBeanDefinitions(beanFactory)装载bean定义
        DefaultListableBeanFactory beanFactory = createBeanFactory();
        beanFactory.setSerializationId(getId());
        customizeBeanFactory(beanFactory); // 对IoC容器进行定制化，如设置启动参数，开启注解的自动装配等 
        loadBeanDefinitions(beanFactory); // 调用载入Bean定义的方法，主要这里又使用了一个委派模式，在当前类中只定义了抽象的loadBeanDefinitions方法，具体的实现调用子类容器  
        this.beanFactory = beanFactory;
    }
    catch (IOException ex) {
        throw new ApplicationContextException("I/O error parsing bean definition source for " + getDisplayName(), ex);
    }
}
```

#### 初始化BeanFactory之loadBeanDefinitions

```java
protected void loadBeanDefinitions(DefaultListableBeanFactory beanFactory) throws BeansException, IOException {
    // 创建XmlBeanDefinitionReader，即创建Bean读取器，并通过回调设置到容器中去，容器使用该读取器读取Bean定义资源  
    XmlBeanDefinitionReader beanDefinitionReader = new XmlBeanDefinitionReader(beanFactory);

    // 配置上下文的环境，资源加载器、解析器
    beanDefinitionReader.setEnvironment(this.getEnvironment());
    beanDefinitionReader.setResourceLoader(this);
    beanDefinitionReader.setEntityResolver(new ResourceEntityResolver(this)); // 为Bean读取器设置SAX xml解析器

    // 允许子类自行初始化（比如校验机制），并提供真正的加载方法
    initBeanDefinitionReader(beanDefinitionReader); // 当Bean读取器读取Bean定义的Xml资源文件时，启用Xml的校验机制  
    loadBeanDefinitions(beanDefinitionReader);
}

protected void loadBeanDefinitions(XmlBeanDefinitionReader reader) throws BeansException, IOException {
    // 加载XML配置方式里的Bean定义的资源
    Resource[] configResources = getConfigResources();
    if (configResources != null) {
        reader.loadBeanDefinitions(configResources);
    }
    // 加载构造函数里配置的Bean配置文件，即{"aspects.xml", "daos.xml", "services.xml"}
    String[] configLocations = getConfigLocations();
    if (configLocations != null) {
        reader.loadBeanDefinitions(configLocations);
    }
}
```

#### AbstractBeanDefinitionReader读取Bean定义资源

- 首先，调用资源加载器的获取资源方法resourceLoader.getResource(location)，获取到要加载的资源。
- 其次，真正执行加载功能是其子类XmlBeanDefinitionReader的loadBeanDefinitions方法。

####  XmlBeanDefinitionReader加载Bean定义资源

```java
/**
    * 本质上是加载XML配置的Bean。
    * @param inputSource the SAX InputSource to read from
    * @param resource the resource descriptor for the XML file
    */
protected int doLoadBeanDefinitions(InputSource inputSource, Resource resource)
        throws BeanDefinitionStoreException {

    try {
        Document doc = doLoadDocument(inputSource, resource); // 用documentLoader将Bean定义资源(xml)转换成Document对象
        int count = registerBeanDefinitions(doc, resource);
        return count;
    }
    // catch...
}
```

#### DocumentLoader将Bean定义资源转换为Document对象

```java
// 使用标准的JAXP将载入的Bean定义资源转换成document对象
@Override
public Document loadDocument(InputSource inputSource, EntityResolver entityResolver,
                             ErrorHandler errorHandler, int validationMode, boolean namespaceAware) throws Exception {

    // 创建文件解析器工厂
    DocumentBuilderFactory factory = createDocumentBuilderFactory(validationMode, namespaceAware);

    // 创建文档解析器
    DocumentBuilder builder = createDocumentBuilder(factory, entityResolver, errorHandler);
    return builder.parse(inputSource); // 解析
}
```

####  XmlBeanDefinitionReader解析载入的Bean定义资源文件

Bean定义资源的载入解析分为以下两个过程：

- 首先，通过调用XML解析器将Bean定义资源文件转换得到Document对象，但是这些Document对象并没有按照Spring的Bean规则进行解析。这一步是载入的过程
- 其次，在完成通用的XML解析之后，按照Spring的Bean规则对Document对象进行解析。

按照Spring的Bean规则对Document对象解析的过程是在接口`BeanDefinitionDocumentReader`的实现类`DefaultBeanDefinitionDocumentReader`中实现的。

BeanDefinitionDocumentReader接口通过registerBeanDefinitions方法调用其实现类DefaultBeanDefinitionDocumentReader对Document对象进行解析

```java
// 注册<beans/>配置的Beans
protected void doRegisterBeanDefinitions(Element root) {
    // Any nested <beans> elements will cause recursion in this method. In
    // order to propagate and preserve <beans> default-* attributes correctly,
    // keep track of the current (parent) delegate, which may be null. Create
    // the new (child) delegate with a reference to the parent for fallback purposes,
    // then ultimately reset this.delegate back to its original (parent) reference.
    // this behavior emulates a stack of delegates without actually necessitating one.
    BeanDefinitionParserDelegate parent = this.delegate;
    this.delegate = createDelegate(getReaderContext(), root, parent);

    if (this.delegate.isDefaultNamespace(root)) {
        String profileSpec = root.getAttribute(PROFILE_ATTRIBUTE);
        if (StringUtils.hasText(profileSpec)) {
            String[] specifiedProfiles = StringUtils.tokenizeToStringArray(
                profileSpec, BeanDefinitionParserDelegate.MULTI_VALUE_ATTRIBUTE_DELIMITERS);
        }
    }
    preProcessXml(root);
    parseBeanDefinitions(root, this.delegate); // 从Document的根元素开始进行Bean定义的Document对象  
    postProcessXml(root);

    this.delegate = parent;
}
```

#### BeanDefinitionParserDelegate解析Bean定义资源文件生成BeanDefinition

```java
/**
    * Parse the elements at the root level in the document:
    * "import", "alias", "bean".
    * @param root the DOM root element of the document
    */
protected void parseBeanDefinitions(Element root, BeanDefinitionParserDelegate delegate) {
    if (delegate.isDefaultNamespace(root)) {
        NodeList nl = root.getChildNodes();
        for (int i = 0; i < nl.getLength(); i++) {
            Node node = nl.item(i);
            if (node instanceof Element) {
                Element ele = (Element) node;
                if (delegate.isDefaultNamespace(ele)) {
                    parseDefaultElement(ele, delegate);
                }
                else {
                    delegate.parseCustomElement(ele);
                }
            }
        }
    }
    else {
        delegate.parseCustomElement(root);
    }
}

private void parseDefaultElement(Element ele, BeanDefinitionParserDelegate delegate) {
      
    // 如果元素节点是<Import>导入元素，进行导入解析
    if (delegate.nodeNameEquals(ele, IMPORT_ELEMENT)) {
        importBeanDefinitionResource(ele);
    }
    // 如果元素节点是<Alias>别名元素，进行别名解析 
    else if (delegate.nodeNameEquals(ele, ALIAS_ELEMENT)) {
        processAliasRegistration(ele);
    }
    // 如果元素节点<Bean>元素, 按照Spring的Bean规则解析元素  
    else if (delegate.nodeNameEquals(ele, BEAN_ELEMENT)) {
        processBeanDefinition(ele, delegate);
    }
    // 如果元素节点<Beans>元素，即它是嵌套类型的
    else if (delegate.nodeNameEquals(ele, NESTED_BEANS_ELEMENT)) {
        // 递归解析
        doRegisterBeanDefinitions(ele);
    }
}
```

解析Bean生成BeanDefinitionHolder的方法

```java
/**
    * Process the given bean element, parsing the bean definition
    * and registering it with the registry.
    */
protected void processBeanDefinition(Element ele, BeanDefinitionParserDelegate delegate) {
    BeanDefinitionHolder bdHolder = delegate.parseBeanDefinitionElement(ele);
    if (bdHolder != null) {
        bdHolder = delegate.decorateBeanDefinitionIfRequired(ele, bdHolder);
        try {
            // 注册最终的装饰实例
            BeanDefinitionReaderUtils.registerBeanDefinition(bdHolder, getReaderContext().getRegistry());
        }
        catch //...
        // Send registration event.
        getReaderContext().fireComponentRegistered(new BeanComponentDefinition(bdHolder));
    }
}
```

`BeanDefinitionHolder`: Holder for a BeanDefinition with name and aliases.

```java
public class BeanDefinitionHolder implements BeanMetadataElement {

    private final BeanDefinition beanDefinition;

    private final String beanName;

    @Nullable
    private final String[] aliases;
    //getters and setters
}
```

#### 解析过后的BeanDefinition在IoC容器中的注册

Document对象的解析后得到封装`BeanDefinition`的`BeanDefinitionHold`对象，然后调用`BeanDefinitionReaderUtils`的`registerBeanDefinition`方法向IoC容器注册解析的Bean。 当调用`BeanDefinitionReaderUtils`向IoC容器注册解析的`BeanDefinition`时，真正完成注册功能的是`DefaultListableBeanFactory`。

IOC容器本质上就是一个beanDefinitionMap， 注册即将BeanDefinition put到map中

```java
/** Map of bean definition objects, keyed by bean name. */
private final Map<String, BeanDefinition> beanDefinitionMap = new ConcurrentHashMap<>(256);

/** Map from bean name to merged BeanDefinitionHolder. */
private final Map<String, BeanDefinitionHolder> mergedBeanDefinitionHolders = new ConcurrentHashMap<>(256);


@Override
public void registerBeanDefinition(String beanName, BeanDefinition beanDefinition)
        throws BeanDefinitionStoreException {

    Assert.hasText(beanName, "Bean name must not be empty");
    Assert.notNull(beanDefinition, "BeanDefinition must not be null");

    if (beanDefinition instanceof AbstractBeanDefinition) {
        try {
            ((AbstractBeanDefinition) beanDefinition).validate();
        }
        catch (BeanDefinitionValidationException ex) {
            throw new BeanDefinitionStoreException(beanDefinition.getResourceDescription(), beanName,
                    "Validation of bean definition failed", ex);
        }
    }

    BeanDefinition existingDefinition = this.beanDefinitionMap.get(beanName);
    // 如果已经注册
    if (existingDefinition != null) {
        // 检查是否可以覆盖
        if (!isAllowBeanDefinitionOverriding()) {
            throw new BeanDefinitionOverrideException(beanName, beanDefinition, existingDefinition);
        }
        // 覆盖
        this.beanDefinitionMap.put(beanName, beanDefinition);
    }
    else {
        if (hasBeanCreationStarted()) {
            // Cannot modify startup-time collection elements anymore (for stable iteration)
            synchronized (this.beanDefinitionMap) {
                this.beanDefinitionMap.put(beanName, beanDefinition);
                List<String> updatedDefinitions = new ArrayList<>(this.beanDefinitionNames.size() + 1);
                updatedDefinitions.addAll(this.beanDefinitionNames);
                updatedDefinitions.add(beanName);
                this.beanDefinitionNames = updatedDefinitions;
                removeManualSingletonName(beanName);
            }
        }
        else {
            // Still in startup registration phase
            this.beanDefinitionMap.put(beanName, beanDefinition);
            this.beanDefinitionNames.add(beanName);
            removeManualSingletonName(beanName);
        }
        //重置所有已经注册过的BeanDefinition的缓存  
        this.frozenBeanDefinitionNames = null;
    }

    if (existingDefinition != null || containsSingleton(beanName)) {
        resetBeanDefinition(beanName);
    }
    else if (isConfigurationFrozen()) {
        clearByTypeCache();
    }
}
```

至此，Bean定义资源文件中配置的Bean被解析过后，已经注册到IoC容器中，被容器管理起来，真正完成了IoC容器初始化所做的全部工作。现 在IoC容器中已经建立了整个Bean的配置信息，这些BeanDefinition信息已经可以使用，并且可以被检索，IoC容器的作用就是对这些注册的Bean定义信息进行处理和维护。这些的注册的Bean定义信息是IoC容器控制反转的基础，正是有了这些注册的数据，容器才可以进行依赖注入。

### 从注解中解析Bean并注入AnnotationConfigApplicationContext

```java
public class AnnotationConfigApplicationContext extends GenericApplicationContext implements AnnotationConfigRegistry {

    private final AnnotatedBeanDefinitionReader reader;

    private final ClassPathBeanDefinitionScanner scanner;

    /**
	 * Create a new AnnotationConfigApplicationContext, deriving bean definitions
	 * from the given component classes and automatically refreshing the context.
	 * @param componentClasses one or more component classes &mdash; for example,
	 * {@link Configuration @Configuration} classes
	 */
    public AnnotationConfigApplicationContext(Class<?>... componentClasses) {
        this();
        register(componentClasses);
        refresh();
    }

    /**
	 * Create a new AnnotationConfigApplicationContext, scanning for components
	 * in the given packages, registering bean definitions for those components,
	 * and automatically refreshing the context.
	 * @param basePackages the packages to scan for component classes
	 */
    public AnnotationConfigApplicationContext(String... basePackages) {
        this();
        scan(basePackages);
        refresh();
    }
}
```

调用`AnnotatedBeanDefinitionReader`中的`doRegisterBean`注册配置类

```java
/**
 * Register a bean from the given bean class, deriving its metadata from
 * class-declared annotations.
 * @param beanClass the class of the bean
 * @param name an explicit name for the bean
 * @param qualifiers specific qualifier annotations to consider, if any,
 * in addition to qualifiers at the bean class level
 * @param supplier a callback for creating an instance of the bean
 * (may be {@code null})
 * @param customizers one or more callbacks for customizing the factory's
 * {@link BeanDefinition}, e.g. setting a lazy-init or primary flag
 * @since 5.0
 */
private <T> void doRegisterBean(Class<T> beanClass, @Nullable String name,
       @Nullable Class<? extends Annotation>[] qualifiers, @Nullable Supplier<T> supplier,
       @Nullable BeanDefinitionCustomizer[] customizers) {

    //Create a new AnnotatedGenericBeanDefinition for the given bean class.
    //this.beanClass = beanClass;
    AnnotatedGenericBeanDefinition abd = new AnnotatedGenericBeanDefinition(beanClass);
    if (this.conditionEvaluator.shouldSkip(abd.getMetadata())) {
       return;
    }

    //读取注解，并设置在BeanDefinition中
    abd.setAttribute(ConfigurationClassUtils.CANDIDATE_ATTRIBUTE, Boolean.TRUE);
    abd.setInstanceSupplier(supplier);
    ScopeMetadata scopeMetadata = this.scopeMetadataResolver.resolveScopeMetadata(abd);
    abd.setScope(scopeMetadata.getScopeName());
    String beanName = (name != null ? name : this.beanNameGenerator.generateBeanName(abd, this.registry));

    AnnotationConfigUtils.processCommonDefinitionAnnotations(abd);
    if (qualifiers != null) {
       for (Class<? extends Annotation> qualifier : qualifiers) {
          if (Primary.class == qualifier) {
             abd.setPrimary(true);
          }
          else if (Lazy.class == qualifier) {
             abd.setLazyInit(true);
          }
          else {
             abd.addQualifier(new AutowireCandidateQualifier(qualifier));
          }
       }
    }
    if (customizers != null) {
       for (BeanDefinitionCustomizer customizer : customizers) {
          customizer.customize(abd);
       }
    }

    BeanDefinitionHolder definitionHolder = new BeanDefinitionHolder(abd, beanName);
    definitionHolder = AnnotationConfigUtils.applyScopedProxyMode(scopeMetadata, definitionHolder, this.registry);
    //将beanDefinition注册到map里面，跟前面一样
    BeanDefinitionReaderUtils.registerBeanDefinition(definitionHolder, this.registry);
}
```

## Bean实例化

如何从IOC容器已有的BeanDefinition信息，实例化出Bean对象

### BeanFactory中getBean的主体思路

上文我们已经分析了IoC初始化的流程，最终的将Bean的定义即BeanDefinition放到beanDefinitionMap中，本质上是一个`ConcurrentHashMap<String, Object>`；并且BeanDefinition接口中包含了这个类的Class信息以及是否是单例等；

![](.\spring-framework-ioc-source-100.png)

这样我们初步有了实现`Object getBean(String name)`这个方法的思路：

- 从beanDefinitionMap通过beanName获得BeanDefinition
- 从BeanDefinition中获得beanClassName
- 通过反射初始化beanClassName的实例instance
  - 构造函数从BeanDefinition的getConstructorArgumentValues()方法获取
  - 属性值从BeanDefinition的getPropertyValues()方法获取
- 返回beanName的实例instance

由于BeanDefinition还有单例的信息，如果是无参构造函数的实例还可以放在一个缓存中，这样下次获取这个单例的实例时只需要从缓存中获取，如果获取不到再通过上述步骤获取。

### Spring中getBean的主体思路

BeanFactory实现getBean方法在AbstractBeanFactory中

- 解析bean的真正name，如果bean是工厂类，name前缀会加&，需要去掉
- 无参单例先从缓存中尝试获取
- 如果bean实例还在创建中，则直接抛出异常
- 如果bean definition 存在于父的bean工厂中，委派给父Bean工厂获取
- 标记这个beanName的实例正在创建
- 确保它的依赖也被初始化
- 真正创建
  - 单例时
  - 原型时
  - 根据bean的scope创建

### createBean()

在AbstractBeanFactory类的doGetBean()中，都是调用AbstractAutowireCapableBeanFactory类的createBean()来创建Bean实例

```java
protected Object createBean(String beanName, RootBeanDefinition mbd, @Nullable Object[] args)
```

第一个参数为beanName，第二个为beanName对应的RootBeanDefinition，第三个参数为构造器的参数列表，实例化的时候需要参数列表推断使用哪一个构造器来进行实例化

#### 加载beanClass

在扫描指定包下面的class文件生成BeanDefinition的时候，beanDefinition中的beanClass属性暂时存放的是类的名称(全称)，而不是加载后的Class对象，但是在实例化Bean对象之前，JVM需要加载将class文件加载进来，并用beanDefinition中的beanClass存储Class对象

```java
RootBeanDefinition mbdToUse = mbd;
// 马上就要实例化Bean了，确保beanClass被加载了
Class<?> resolvedClass = resolveBeanClass(mbd, beanName);
if (resolvedClass != null && !mbd.hasBeanClass() && mbd.getBeanClassName() != null) {
    mbdToUse = new RootBeanDefinition(mbd);
    mbdToUse.setBeanClass(resolvedClass);
}
```

下面看resolveBeanClass(mbd, beanName)是如何获取Class对象的，如果该RootBeanDefinition对应的Class对象已经加载，则直接返回该对象，否则调用doResolveBeanClass(mbd, typesToMatch)方法来加载Class对象

```java
protected Class<?> resolveBeanClass(RootBeanDefinition mbd, String beanName, Class<?>... typesToMatch)
    throws CannotLoadBeanClassException {

    try {
        // 如果beanClass被加载了
        if (mbd.hasBeanClass()) {
            return mbd.getBeanClass();
        }
        ……		
            // 如果beanClass没有被加载
            return doResolveBeanClass(mbd, typesToMatch);
    }
}
```

看doResolveBeanClass是如何实现的

```java
private Class<?> doResolveBeanClass(RootBeanDefinition mbd, Class<?>... typesToMatch)
    throws ClassNotFoundException {

    // 获取类加载器
    ClassLoader beanClassLoader = getBeanClassLoader();
    ClassLoader dynamicLoader = beanClassLoader;
    boolean freshResolve = false;
    if (!ObjectUtils.isEmpty(typesToMatch)) {
        // When just doing type checks (i.e. not creating an actual instance yet),
        // use the specified temporary class loader (e.g. in a weaving scenario).
        ClassLoader tempClassLoader = getTempClassLoader();
        if (tempClassLoader != null) {
            dynamicLoader = tempClassLoader;
            freshResolve = true;
            if (tempClassLoader instanceof DecoratingClassLoader) {
                DecoratingClassLoader dcl = (DecoratingClassLoader) tempClassLoader;
                for (Class<?> typeToMatch : typesToMatch) {
                    dcl.excludeClass(typeToMatch.getName());
                }
            }
        }
    }
```

**首先获取类加载器，getBeanClassLoader()会调用ClassUtils类中的getDefaultClassLoader()获取类加载器，获取类加载的代码如下：**

首先获取当前线程指定上下文加载器，可以在Spring容器启动之前，设置当前线程(主线程)上下文的类加载器

```java
Thread.currentThread().setContextClassLoader(ClassLoader.getSystemClassLoader());
```

如果没有指定，则获取当前类ClassUtils的类加载器，如果当前类是通过bootstrap加载的，则获取系统类加载器

```java
public static ClassLoader getDefaultClassLoader() {
    ClassLoader cl = null;

    // 优先获取线程中的类加载器
    cl = Thread.currentThread().getContextClassLoader();

    // 线程中类加载器为null的情况下，获取加载ClassUtils类的类加载器
    if (cl == null) {
        // No thread context class loader -> use class loader of this class.
        cl = ClassUtils.class.getClassLoader();
        if (cl == null) {
            // getClassLoader() returning null indicates the bootstrap ClassLoader
            // 假如ClassUtils是被Bootstrap类加载器加载的，则获取系统类加载器
            cl = ClassLoader.getSystemClassLoader();
        }
    }
    return cl;
}
```

**判断beanClassName是否是EL表达式**

getBeanClassName()方法返回的可能是类加载的名字，也可能只是类名，如果beanClass是一个Spring的EL表达式，则evaluateBeanDefinitionString可以将其解析，返回一个Class对象或类名

```java
String className = mbd.getBeanClassName();
if (className != null) {
    // 解析Spring表达式，有可能直接返回了一个Class对象
    Object evaluated = evaluateBeanDefinitionString(className, mbd);
    if (!className.equals(evaluated)) {
        // A dynamically resolved expression, supported as of 4.2...
        if (evaluated instanceof Class) {
            return (Class<?>) evaluated;
        }
        else if (evaluated instanceof String) {
            className = (String) evaluated;
            freshResolve = true;
        }
    }
    if (freshResolve) {
        if (dynamicLoader != null) {
           return dynamicLoader.loadClass(className);
        }
        return ClassUtils.forName(className, dynamicLoader);
    }
}
```

**解析生成beanClass**

如果前面的条件都无法得到Class对象，则调用RootBeanDefinition的resolveBeanClass方法

```java
@Nullable
private Class<?> doResolveBeanClass(RootBeanDefinition mbd, Class<?>... typesToMatch)
    throws ClassNotFoundException {
    ……
    return mbd.resolveBeanClass(beanClassLoader);
}
```

resolveBeanClass()中调用 ClassUtils.forName生成对应Class对象，然后将Class对象存储在RootBeanDefinition的beanClass属性中

```java
public Class<?> resolveBeanClass(@Nullable ClassLoader classLoader) throws ClassNotFoundException {
    String className = getBeanClassName();
    if (className == null) {
        return null;
    }
    Class<?> resolvedClass = ClassUtils.forName(className, classLoader);
    this.beanClass = resolvedClass;
    return resolvedClass;
}
```

#### 实例化前

在实例化前操作的前面，会调用prepareMethodOverrides()处理带有@Lookup注解的方法，然后再进行实例化前的操作

```java
protected Object createBean(String beanName, RootBeanDefinition mbd, @Nullable Object[] args)
    throws BeanCreationException {
    ……
    mbdToUse.prepareMethodOverrides();
    ……
}
```

在对Bean进行实例化之前，调用resolveBeforeInstantiation()执行初始化前的操作，如果初始化前的操作直接就生成了Bean对象，则直接返回，没必要再执行后面的实例化操作

```java
protected Object createBean(String beanName, RootBeanDefinition mbd, @Nullable Object[] args)
    throws BeanCreationException {
    ……
    // 实例化前
    Object bean = resolveBeforeInstantiation(beanName, mbdToUse);
    if (bean != null) {
        return bean;
    }
    ……
}
```

resolveBeforeInstantiation()中，hasInstantiationAwareBeanPostProcessors()会判断当前Spring容器的Bean中是否有实现了InstantiationAwareBeanPostProcessor接口的Bean

```java
protected Object resolveBeforeInstantiation(String beanName, RootBeanDefinition mbd) {
    Object bean = null;
    if (!Boolean.FALSE.equals(mbd.beforeInstantiationResolved)) {
        // Make sure bean class is actually resolved at this point.
        // synthetic表示合成，如果某些Bean式合成的，那么则不会经过BeanPostProcessor的处理
        // 判断是否有Bean实现了InstantiationAwareBeanPostProcessor接口
        if (!mbd.isSynthetic() && hasInstantiationAwareBeanPostProcessors()) {
            Class<?> targetType = determineTargetType(beanName, mbd);
            if (targetType != null) {
                bean = applyBeanPostProcessorsBeforeInstantiation(targetType, beanName);
                if (bean != null) {
                    bean = applyBeanPostProcessorsAfterInitialization(bean, beanName);
                }
            }
        }
        mbd.beforeInstantiationResolved = (bean != null);
    }
    return bean;
}
```

在讲清楚InstantiationAwareBeanPostProcessor之前，需要先了解一下AbstractBeanFactory中的静态内部类BeanPostProcessorCache，源码如下：

```java
static class BeanPostProcessorCache {

    final List<InstantiationAwareBeanPostProcessor> instantiationAware = new ArrayList<>();

    final List<SmartInstantiationAwareBeanPostProcessor> smartInstantiationAware = new ArrayList<>();

    final List<DestructionAwareBeanPostProcessor> destructionAware = new ArrayList<>();

    final List<MergedBeanDefinitionPostProcessor> mergedDefinition = new ArrayList<>();
}
```

这四个接口都继承自BeanPostProcessor接口，BeanPostProcessor接口定义了postProcessBeforeInitialization()和postProcessAfterInitialization两个方法，即初始化前和初始化需要调用的方法，在这两个方法的基础上，上述四个接口也定义各自的方法，源码如下

```java
public interface InstantiationAwareBeanPostProcessor extends BeanPostProcessor {
    // 实例化前调用
    default Object postProcessBeforeInstantiation(Class<?> beanClass, String beanName) throws BeansException {
        return null;
    }

    // 实例化后调用
    default boolean postProcessAfterInstantiation(Object bean, String beanName) throws BeansException {
        return true;
    }

    //属性填充
    default PropertyValues postProcessProperties(PropertyValues pvs, Object bean, String beanName)
        throws BeansException {
        return null;
    }
}
```

**调用实例化前方法**

调用`applyBeanPostProcessorsBeforeInstantiation()`执行实例化前方法，beanPostProcessorCache可能有多个实现了InstantiationAwareBeanPostProcessor接口的Bean对象，则遍历调用这些对象的`postProcessBeforeInstantiation()`方法，如果某个对象调用该方法时返回了一个对象，则直接返回该对象

```java
protected Object applyBeanPostProcessorsBeforeInstantiation(Class<?> beanClass, String beanName) {
   for (InstantiationAwareBeanPostProcessor bp : getBeanPostProcessorCache().instantiationAware) {
      Object result = bp.postProcessBeforeInstantiation(beanClass, beanName);
      if (result != null) {
         return result;
      }
   }
   return null;
}
```

**如果调用实例化前方法产生了对象，则调用初始化后方法**

```java
public Object applyBeanPostProcessorsAfterInitialization(Object existingBean, String beanName)
    throws BeansException {

    Object result = existingBean;

    for (BeanPostProcessor processor : getBeanPostProcessors()) {
        Object current = processor.postProcessAfterInitialization(result, beanName);
        if (current == null) {
            return result;
        }
        result = current;
    }
    return result;
}
```

#### 实例化

经历了实例化前操作，如果返回的对象不是null，则直接将返回的对象作为Bean实例，如果返回的对象等于null，则调用`doCreateBean()`方法创建Bean实例

`doCreateBean()`中调用`createBeanInstance()`来完成实例化

```java
protected Object doCreateBean(String beanName, RootBeanDefinition mbd, @Nullable Object[] args)
    throws BeanCreationException {
    BeanWrapper instanceWrapper = null;
    if (mbd.isSingleton()) {
        // 有可能在本Bean创建之前，就有其他Bean把当前Bean给创建出来了（比如依赖注入过程中）
        instanceWrapper = this.factoryBeanInstanceCache.remove(beanName);
    }
    if (instanceWrapper == null) {
        // 创建Bean实例
        instanceWrapper = createBeanInstance(beanName, mbd, args);
    }
}
```

#### 实例化后

在doCreateBean()中调用createBeanInstance()完成Bean的实例化以后，还需要进行实例化后的一系列的操作

**BeanDefinition后置处理**

完成实例化之后，遍历BeanPostProcessorCache中缓存的实现了MergedBeanDefinitionPostProcessor接口的Bean对象，然后调用对象的postProcessMergedBeanDefinition()方法，对合并的BeanDefinition进行操作，包括设置初始化方法名、属性赋值等等操作

```java
protected Object doCreateBean(String beanName, RootBeanDefinition mbd, @Nullable Object[] args)
    throws BeanCreationException {
    ……
        synchronized (mbd.postProcessingLock) {
        if (!mbd.postProcessed) {
            applyMergedBeanDefinitionPostProcessors(mbd, beanType, beanName);
            mbd.postProcessed = true;
        }
    } 
    ……
}

protected void applyMergedBeanDefinitionPostProcessors(RootBeanDefinition mbd, Class<?> beanType, String beanName) {
    for (MergedBeanDefinitionPostProcessor processor : getBeanPostProcessorCache().mergedDefinition) {
        processor.postProcessMergedBeanDefinition(mbd, beanType, beanName);
    }
}
```

**属性填充**

执行完BeanDefinition的后置处理后，将进行属性填充，而属性填充又涉及到一系列的动作

```java
protected Object doCreateBean(String beanName, RootBeanDefinition mbd, @Nullable Object[] args)
    throws BeanCreationException {
    ……
    // 属性填充
    populateBean(beanName, mbd, instanceWrapper);
    ……
}
```

**调用实例化后方法：**

这个地方同实例化前操作相似，不过这里调用的是实现了InstantiationAwareBeanPostProcessor接口的Bean对象的postProcessAfterInstantiation()实例化后方法

```java
protected void populateBean(String beanName, RootBeanDefinition mbd, @Nullable BeanWrapper bw) {
    ……
    if (!mbd.isSynthetic() && hasInstantiationAwareBeanPostProcessors()) {
        for (InstantiationAwareBeanPostProcessor bp : getBeanPostProcessorCache().instantiationAware) {
            if (!bp.postProcessAfterInstantiation(bw.getWrappedInstance(), beanName)) {
                return;
            }
        }
    }
}
```

**属性填充：**

AutowiredAnnotationBeanPostProcessor和CommonAnnotationBeanPostProcessor都继承了InstantiationAwareBeanPostProcessor接口，而这两个类会在postProcessProperties()方法中解决@Autowired和@Resource的属性注入，代码中的pvs得到的是在BeanDefinition后置处理中设置属性值，如果设置了属性值，AutowiredAnnotationBeanPostProcessor将不在处理这些属性，直接返回

```java
protected void populateBean(String beanName, RootBeanDefinition mbd, @Nullable BeanWrapper bw) {
    ……
        boolean hasInstAwareBpps = hasInstantiationAwareBeanPostProcessors();
    boolean needsDepCheck = (mbd.getDependencyCheck() != AbstractBeanDefinition.DEPENDENCY_CHECK_NONE);
    PropertyDescriptor[] filteredPds = null;
    if (hasInstAwareBpps) {
        if (pvs == null) {
            pvs = mbd.getPropertyValues();
        }
        for (InstantiationAwareBeanPostProcessor bp : getBeanPostProcessorCache().instantiationAware) {
            // 这里会调用AutowiredAnnotationBeanPostProcessor的postProcessProperties()方法，会直接给对象中的属性赋值
            // AutowiredAnnotationBeanPostProcessor内部并不会处理pvs，直接返回了
            PropertyValues pvsToUse = bp.postProcessProperties(pvs, bw.getWrappedInstance(), beanName);
            if (pvsToUse == null) {
                if (filteredPds == null) {
                    filteredPds = filterPropertyDescriptorsForDependencyCheck(bw, mbd.allowCaching);
                }
            }
            pvs = pvsToUse;
        }
    }
}
```

**初始化**

属性填充完成之后，进行初始化的操作

```java
protected Object doCreateBean(String beanName, RootBeanDefinition mbd, @Nullable Object[] args)
    throws BeanCreationException {
    ……
    // 初始化
	exposedObject = initializeBean(beanName, exposedObject, mbd);
    ……
}
```

**调用Aware方法：**

在进行初始化的时候，首先判断当前Bean实现了哪些Aware接口，然后设置对应的属性

```java
protected Object initializeBean(String beanName, Object bean, @Nullable RootBeanDefinition mbd) {
    ……
    invokeAwareMethods(beanName, bean);
    ……
}

private void invokeAwareMethods(String beanName, Object bean) {
    if (bean instanceof Aware) {
        if (bean instanceof BeanNameAware) {
            ((BeanNameAware) bean).setBeanName(beanName);
        }
        if (bean instanceof BeanClassLoaderAware) {
            ClassLoader bcl = getBeanClassLoader();
            if (bcl != null) {
                ((BeanClassLoaderAware) bean).setBeanClassLoader(bcl);
            }
        }
        if (bean instanceof BeanFactoryAware) {
            ((BeanFactoryAware) bean).setBeanFactory(AbstractAutowireCapableBeanFactory.this);
        }
    }
}
```

**调用初始化前方法：**

Aware方法调用完成之后，调用初始化前方法，执行所有BeanPostProcessor的postProcessBeforeInitialization()初始化前方法，如果某个Bean对象调用该方法返回null，则直接返回，不再执行后面Bean对象的方法

```java
protected Object initializeBean(String beanName, Object bean, @Nullable RootBeanDefinition mbd) {
    ……
    if (mbd == null || !mbd.isSynthetic()) {
        // 初始化前
        wrappedBean = applyBeanPostProcessorsBeforeInitialization(wrappedBean, beanName);
    }
    ……
}
public Object applyBeanPostProcessorsBeforeInitialization(Object existingBean, String beanName)
    throws BeansException {

    Object result = existingBean;
    for (BeanPostProcessor processor : getBeanPostProcessors()) {
        Object current = processor.postProcessBeforeInitialization(result, beanName);
        if (current == null) {
            return result;
        }
        result = current;
    }
    return result;
}
```

**调用初始化方法：**

初始化的过程中，会判断当前Bean实例是否实现了InitializingBean接口，如果实现了该接口，则会调用Bean对象的afterPropertiesSet()方法，如果在BeanDifinition的后置处理中设置了初始化方法名并且该对象中有此方法，放回通过反射调用指定的初始化方法

```java
protected Object initializeBean(String beanName, Object bean, @Nullable RootBeanDefinition mbd) {
    ……
    // 初始化
    invokeInitMethods(beanName, wrappedBean, mbd);
    ……
}
protected void invokeInitMethods(String beanName, Object bean, @Nullable RootBeanDefinition mbd)
    throws Throwable {

    boolean isInitializingBean = (bean instanceof InitializingBean);
    if (isInitializingBean && (mbd == null || !mbd.isExternallyManagedInitMethod("afterPropertiesSet"))) {
        ((InitializingBean) bean).afterPropertiesSet();
    }

    if (mbd != null && bean.getClass() != NullBean.class) {
        String initMethodName = mbd.getInitMethodName();
        if (StringUtils.hasLength(initMethodName) &&
            !(isInitializingBean && "afterPropertiesSet".equals(initMethodName)) &&
            !mbd.isExternallyManagedInitMethod(initMethodName)) {
            invokeCustomInitMethod(beanName, bean, mbd);
        }
    }
}
```

**调用初始化后：**

初始化完成之后，调用初始化后方法，执行所有BeanPostProcessor的postProcessAfterInitialization()初始化后方法，如果某个Bean对象调用该方法返回null，则直接返回，不再执行后面Bean对象的方法

```java
protected Object initializeBean(String beanName, Object bean, @Nullable RootBeanDefinition mbd) {
    ……
    // 初始化后
    if (mbd == null || !mbd.isSynthetic()) {
        wrappedBean = applyBeanPostProcessorsAfterInitialization(wrappedBean, beanName);
    }
    ……
}
public Object applyBeanPostProcessorsAfterInitialization(Object existingBean, String beanName)
    throws BeansException {

    Object result = existingBean;

    for (BeanPostProcessor processor : getBeanPostProcessors()) {
        Object current = processor.postProcessAfterInitialization(result, beanName);
        if (current == null) {
            return result;
        }
        result = current;
    }
    return result;
}
```

## Spring 循环依赖原理

### 什么是循环依赖？

从字面上来理解就是A依赖B的同时B也依赖了A，就像下面这样

```java
@Component
public class A {
    // A中注入了B
    @Autowired
    private B b;
}

@Component
public class B {
    // B中也注入了A
    @Autowired
    private A a;
}
```

### 什么情况下循环依赖可以被处理？

1. 出现循环依赖的Bean必须要是单例
2. 依赖注入的方式不能全是构造器注入的方式（很多博客上说，只能解决setter方法的循环依赖，这是错误的）

其中第一点应该很好理解，第二点：不能全是构造器注入是什么意思呢？我们还是用代码说话

```java
@Component
public class A {
    //    @Autowired
    //    private B b;
    public A(B b) {

    }
}

@Component
public class B {

    //    @Autowired
    //    private A a;

    public B(A a){

    }
}
```

在上面的例子中，A中注入B的方式是通过构造器，B中注入A的方式也是通过构造器，这个时候循环依赖是无法被解决，如果你的项目中有两个这样相互依赖的Bean，在启动时就会报出以下错误：

```
Caused by: org.springframework.beans.factory.BeanCurrentlyInCreationException: Error creating bean with name 'a': Requested bean is currently in creation: Is there an unresolvable circular reference?
```

### Spring是如何解决的循环依赖？

Spring为了解决单例的循环依赖问题，使用了三级缓存。

```java
/** Cache of singleton objects: bean name --> bean instance */
private final Map<String, Object> singletonObjects = new ConcurrentHashMap<String, Object>(256);
 
/** Cache of early singleton objects: bean name --> bean instance */
private final Map<String, Object> earlySingletonObjects = new HashMap<String, Object>(16);

/** Cache of singleton factories: bean name --> ObjectFactory */
private final Map<String, ObjectFactory<?>> singletonFactories = new HashMap<String, ObjectFactory<?>>(16);
```

- **第一层缓存（singletonObjects）**：单例对象缓存池，已经实例化并且属性赋值，这里的对象是**成熟对象**；
- **第二层缓存（earlySingletonObjects）**：单例对象缓存池，已经实例化但尚未属性赋值，这里的对象是**半成品对象**；
- **第三层缓存（singletonFactories）**: 单例工厂的缓存

分析getSingleton()的整个过程，Spring首先从一级缓存singletonObjects中获取。若是获取不到，而且对象正在建立中，就再从二级缓存earlySingletonObjects中获取。若是仍是获取不到且容许singletonFactories经过getObject()获取，就从三级缓存singletonFactory.getObject()(三级缓存)获取，若是获取到了则从三级缓存移动到了二级缓存。

当A完成了实例化并添加进了三级缓存后，就要开始为A进行属性注入了，在注入时发现A依赖了B，那么这个时候Spring又会去`getBean(b)`，然后反射调用setter方法完成属性注入。

![](.\SpringDependency.png)

因为B需要注入A，所以在创建B的时候，又会去调用`getBean(a)`，这个时候就又回到之前的流程了，但是不同的是，之前的`getBean`是为了创建Bean，而此时再调用`getBean`不是为了创建了，而是要从缓存中获取，因为之前A在实例化后已经将其放入了三级缓存`singletonFactories`中，所以此时`getBean(a)`的流程就是这样子了

![](.\SpringDependency1.png)

从这里我们可以看出，注入到B中的A是通过`getEarlyBeanReference`方法提前暴露出去的一个对象，还不是一个完整的Bean，那么`getEarlyBeanReference`到底干了啥了，我们看下它的源码

```java
protected Object getEarlyBeanReference(String beanName, RootBeanDefinition mbd, Object bean) {
    Object exposedObject = bean;
    //AOP stuff
    if (!mbd.isSynthetic() && hasInstantiationAwareBeanPostProcessors()) {
        for (BeanPostProcessor bp : getBeanPostProcessors()) {
            if (bp instanceof SmartInstantiationAwareBeanPostProcessor) {
                SmartInstantiationAwareBeanPostProcessor ibp = (SmartInstantiationAwareBeanPostProcessor) bp;
                exposedObject = ibp.getEarlyBeanReference(exposedObject, beanName);
            }
        }
    }
    return exposedObject;
}
```

在普通的循环依赖的情况下，三级缓存没有任何作用。三级缓存实际上跟Spring中的`AOP`相关

![](.\SpringDependency2.png)

从上图中我们可以看到，虽然在创建B时会提前给B注入了一个还未初始化的A对象，但是在创建A的流程中一直使用的是注入到B中的A对象的引用，之后会根据这个引用对A进行初始化，所以这是没有问题的。

### 结合了AOP的循环依赖

之前我们已经说过了，在普通的循环依赖的情况下，三级缓存没有任何作用。三级缓存实际上跟Spring中的`AOP`相关，我们再来看一看`getEarlyBeanReference`的代码：

```java
protected Object getEarlyBeanReference(String beanName, RootBeanDefinition mbd, Object bean) {
    Object exposedObject = bean;
    if (!mbd.isSynthetic() && hasInstantiationAwareBeanPostProcessors()) {
        for (BeanPostProcessor bp : getBeanPostProcessors()) {
            if (bp instanceof SmartInstantiationAwareBeanPostProcessor) {
                SmartInstantiationAwareBeanPostProcessor ibp = (SmartInstantiationAwareBeanPostProcessor) bp;
                exposedObject = ibp.getEarlyBeanReference(exposedObject, beanName);
            }
        }
    }
    return exposedObject;
}
```

如果在开启`AOP`的情况下，那么就是调用到`AnnotationAwareAspectJAutoProxyCreator`的`getEarlyBeanReference`方法，对应的源码如下：

```java
public Object getEarlyBeanReference(Object bean, String beanName) {
    Object cacheKey = getCacheKey(bean.getClass(), beanName);
    this.earlyProxyReferences.put(cacheKey, bean);
    // 如果需要代理，返回一个代理对象，不需要代理，直接返回当前传入的这个bean对象
    return wrapIfNecessary(bean, beanName, cacheKey);
}
```

回到上面的例子，我们对A进行了`AOP`代理的话，那么此时`getEarlyBeanReference`将返回一个代理后的对象，而不是实例化阶段创建的对象，这样就意味着B中注入的A将是一个代理对象而不是A的实例化阶段创建后的对象。

**三级缓存为什么要使用工厂而不是直接使用引用？换而言之，为什么需要这个三级缓存，直接通过二级缓存暴露一个引用不行吗？**

这个工厂的目的在于延迟对实例化阶段生成的对象的代理，只有真正发生循环依赖的时候，才去提前生成代理对象，否则只会创建一个工厂并将其放入到三级缓存中，但是不会去通过这个工厂去真正创建对象

即使没有循环依赖，也会将其添加到三级缓存中，而且是不得不添加到三级缓存中，因为到目前为止Spring也不能确定这个Bean有没有跟别的Bean出现循环依赖。

Spring结合`AOP`跟Bean的生命周期本身就是通过`AnnotationAwareAspectJAutoProxyCreator`这个后置处理器来完成的，在这个后置处理的`postProcessAfterInitialization`方法中对初始化后的Bean完成`AOP`代理。如果出现了循环依赖，那没有办法，只有给Bean先创建代理，但是没有出现循环依赖的情况下，设计之初就是让Bean在生命周期的最后一步完成代理而不是在实例化后就立马完成代理。

###  Spring为何不能解决非单例属性之外的循环依赖

**Spring为什么不能解决构造器的循环依赖？**

构造器注入形成的循环依赖： 也就是beanB需要在beanA的构造函数中完成初始化，beanA也需要在beanB的构造函数中完成初始化，这种情况的结果就是两个bean都不能完成初始化，循环依赖难以解决。

Spring解决循环依赖主要是依赖三级缓存，但是的**在调用构造方法之前还未将其放入三级缓存之中**，因此后续的依赖调用构造方法的时候并不能从三级缓存中获取到依赖的Bean，因此不能解决。

**Spring为什么不能解决prototype作用域循环依赖？**

这种循环依赖同样无法解决，因为spring不会缓存‘prototype’作用域的bean，而spring中循环依赖的解决正是通过缓存来实现的。

**Spring为什么不能解决多例的循环依赖？**

多实例Bean是每次调用一次getBean都会执行一次构造方法并且给属性赋值，根本没有三级缓存，因此不能解决循环依赖。

### 那么其它循环依赖如何解决？

- **生成代理对象产生的循环依赖**

这类循环依赖问题解决方法很多，主要有：

1. 使用@Lazy注解，延迟加载
2. 使用@DependsOn注解，指定加载先后关系
3. 修改文件名称，改变循环依赖类的加载顺序

- **使用@DependsOn产生的循环依赖**

这类循环依赖问题要找到@DependsOn注解循环依赖的地方，迫使它不循环依赖就可以解决问题。

- **多例循环依赖**

这类循环依赖问题可以通过把bean改成单例的解决。

- **构造器循环依赖**

这类循环依赖问题可以通过使用@Lazy注解解决。

## 深入理解Bean生命周期

> Spring 只帮我们管理单例模式 Bean 的**完整**生命周期，对于 prototype 的 bean ，Spring 在创建好交给使用者之后则不会再管理后续的生命周期。

Spring 容器可以管理 singleton 作用域 Bean 的生命周期，在此作用域下，Spring 能够精确地知道该 Bean 何时被创建，何时初始化完成，以及何时被销毁。

而对于 prototype 作用域的 Bean，Spring 只负责创建，当容器创建了 Bean 的实例后，Bean 的实例就交给客户端代码管理，Spring 容器将不再跟踪其生命周期。每次客户端请求 prototype 作用域的 Bean 时，Spring 容器都会创建一个新的实例，并且不会管那些被配置成 prototype 作用域的 Bean 的生命周期。

了解 Spring 生命周期的意义就在于，**可以利用 Bean 在其存活期间的指定时刻完成一些相关操作**。这种时刻可能有很多，但一般情况下，会在 Bean 被初始化后和被销毁前执行一些相关操作。

### Spring Bean生命周期流程

![](.\spring-framework-ioc-source-102.png)

- 如果 BeanFactoryPostProcessor 和 Bean 关联, 则调用postProcessBeanFactory方法.(即首**先尝试从Bean工厂中获取Bean**)

- 如果 InstantiationAwareBeanPostProcessor 和 Bean 关联，则调用postProcessBeforeInstantiation方法

- 根据配置情况调用 Bean 构造方法**实例化 Bean**。

- 利用依赖注入完成 Bean 中所有**属性值的配置注入**。

- 如果 InstantiationAwareBeanPostProcessor 和 Bean 关联，则调用postProcessAfterInstantiation方法和postProcessProperties

- 调用xxxAware接口

   

  (上图只是给了几个例子)

  - 第一类Aware接口
    - 如果 Bean 实现了 BeanNameAware 接口，则 Spring 调用 Bean 的 setBeanName() 方法传入当前 Bean 的 id 值。
    - 如果 Bean 实现了 BeanClassLoaderAware 接口，则 Spring 调用 setBeanClassLoader() 方法传入classLoader的引用。
    - 如果 Bean 实现了 BeanFactoryAware 接口，则 Spring 调用 setBeanFactory() 方法传入当前工厂实例的引用。
  - 第二类Aware接口
    - 如果 Bean 实现了 EnvironmentAware 接口，则 Spring 调用 setEnvironment() 方法传入当前 Environment 实例的引用。
    - 如果 Bean 实现了 EmbeddedValueResolverAware 接口，则 Spring 调用 setEmbeddedValueResolver() 方法传入当前 StringValueResolver 实例的引用。
    - 如果 Bean 实现了 ApplicationContextAware 接口，则 Spring 调用 setApplicationContext() 方法传入当前 ApplicationContext 实例的引用。
    - ...

- 如果 BeanPostProcessor 和 Bean 关联，则 Spring 将调用该接口的预初始化方法 postProcessBeforeInitialzation() 对 Bean 进行加工操作，此处非常重要，Spring 的 AOP 就是利用它实现的。

- 如果 Bean 实现了 InitializingBean 接口，则 Spring 将调用 afterPropertiesSet() 方法。(或者有执行@PostConstruct注解的方法)

- 如果在配置文件中通过 **init-method** 属性指定了初始化方法，则调用该初始化方法。

- 如果 BeanPostProcessor 和 Bean 关联，则 Spring 将调用该接口的初始化方法 postProcessAfterInitialization()。此时，Bean 已经可以被应用系统使用了。

- 如果在 `<bean>` 中指定了该 Bean 的作用范围为 scope="singleton"，则将该 Bean 放入 Spring IoC 的缓存池中，将触发 Spring 对该 Bean 的生命周期管理；如果在 `<bean>` 中指定了该 Bean 的作用范围为 scope="prototype"，则将该 Bean 交给调用者，调用者管理该 Bean 的生命周期，Spring 不再管理该 Bean。

- 如果 Bean 实现了 DisposableBean 接口，则 Spring 会调用 destory() 方法将 Spring 中的 Bean 销毁；(或者有执行@PreDestroy注解的方法)

- 如果在配置文件中通过 **destory-method** 属性指定了 Bean 的销毁方法，则 Spring 将调用该方法对 Bean 进行销毁。

**Bean的完整生命周期经历了各种方法调用，这些方法可以划分为以下几类**：(结合上图，需要有如下顶层思维)

- **Bean自身的方法**： 这个包括了Bean本身调用的方法和通过配置文件中`<bean>`的init-method和destroy-method指定的方法
- **Bean级生命周期接口方法**： 这个包括了BeanNameAware、BeanFactoryAware、ApplicationContextAware；当然也包括InitializingBean和DiposableBean这些接口的方法（可以被@PostConstruct和@PreDestroy注解替代)
- **容器级生命周期接口方法**： 这个包括了InstantiationAwareBeanPostProcessor 和 BeanPostProcessor 这两个接口实现，一般称它们的实现类为“后处理器”。
- **工厂后处理器接口方法**： 这个包括了AspectJWeavingEnabler, ConfigurationClassPostProcessor, CustomAutowireConfigurer等等非常有用的工厂后处理器接口的方法。工厂后处理器也是容器级的。在应用上下文装配配置文件之后立即调用。

## Spring MVC核心流程

## Spring AOP原理分析

### aop配置标签的解析

AOP是基于IOC的Bean加载来实现的, 在`parseCustomElement`方法找到parse `aop:aspectj-autoproxy`的handler(`org.springframework.aop.config.AopNamespaceHandler`)

```java
public class AopNamespaceHandler extends NamespaceHandlerSupport {

	/**
	 * Register the {@link BeanDefinitionParser BeanDefinitionParsers} for the
	 * '{@code config}', '{@code spring-configured}', '{@code aspectj-autoproxy}'
	 * and '{@code scoped-proxy}' tags.
	 */
	@Override
	public void init() {
		// In 2.0 XSD as well as in 2.5+ XSDs
        // 注册解析<aop:config> 配置
		registerBeanDefinitionParser("config", new ConfigBeanDefinitionParser());
        // 注册解析<aop:aspectj-autoproxy> 配置
		registerBeanDefinitionParser("aspectj-autoproxy", new AspectJAutoProxyBeanDefinitionParser());
		registerBeanDefinitionDecorator("scoped-proxy", new ScopedProxyBeanDefinitionDecorator());

		// Only in 2.0 XSD: moved to context namespace in 2.5+
		registerBeanDefinitionParser("spring-configured", new SpringConfiguredBeanDefinitionParser());
	}
}
```

### config配置标签的解析

`<aop:config/>`由ConfigBeanDefinitionParser这个类处理，作为parser类最重要的就是parse方法

```java
@Override
@Nullable
public BeanDefinition parse(Element element, ParserContext parserContext) {
    CompositeComponentDefinition compositeDef =
            new CompositeComponentDefinition(element.getTagName(), parserContext.extractSource(element));
    parserContext.pushContainingComponent(compositeDef);

    configureAutoProxyCreator(parserContext, element);

    List<Element> childElts = DomUtils.getChildElements(element);
    for (Element elt: childElts) {
        String localName = parserContext.getDelegate().getLocalName(elt);
        if (POINTCUT.equals(localName)) {
            parsePointcut(elt, parserContext);
        }
        else if (ADVISOR.equals(localName)) {
            parseAdvisor(elt, parserContext);
        }
        else if (ASPECT.equals(localName)) {
            parseAspect(elt, parserContext);
        }
    }

    parserContext.popAndRegisterContainingComponent();
    return null;
}
```

parseAspect的方法如下

```java
private void parseAspect(Element aspectElement, ParserContext parserContext) {
    String aspectId = aspectElement.getAttribute(ID);
    String aspectName = aspectElement.getAttribute(REF);

    try {
        this.parseState.push(new AspectEntry(aspectId, aspectName));
        List<BeanDefinition> beanDefinitions = new ArrayList<>();
        List<BeanReference> beanReferences = new ArrayList<>();

        List<Element> declareParents = DomUtils.getChildElementsByTagName(aspectElement, DECLARE_PARENTS);
        for (int i = METHOD_INDEX; i < declareParents.size(); i++) {
            Element declareParentsElement = declareParents.get(i);
            beanDefinitions.add(parseDeclareParents(declareParentsElement, parserContext));
        }

        // We have to parse "advice" and all the advice kinds in one loop, to get the
        // ordering semantics right.
        NodeList nodeList = aspectElement.getChildNodes();
        boolean adviceFoundAlready = false;
        for (int i = 0; i < nodeList.getLength(); i++) {
            Node node = nodeList.item(i);
            if (isAdviceNode(node, parserContext)) {
                if (!adviceFoundAlready) {
                    adviceFoundAlready = true;
                    if (!StringUtils.hasText(aspectName)) {
                        parserContext.getReaderContext().error(
                                "<aspect> tag needs aspect bean reference via 'ref' attribute when declaring advices.",
                                aspectElement, this.parseState.snapshot());
                        return;
                    }
                    beanReferences.add(new RuntimeBeanReference(aspectName));
                }
                AbstractBeanDefinition advisorDefinition = parseAdvice(
                        aspectName, i, aspectElement, (Element) node, parserContext, beanDefinitions, beanReferences);
                beanDefinitions.add(advisorDefinition);
            }
        }

        AspectComponentDefinition aspectComponentDefinition = createAspectComponentDefinition(
                aspectElement, aspectId, beanDefinitions, beanReferences, parserContext);
        parserContext.pushContainingComponent(aspectComponentDefinition);

        List<Element> pointcuts = DomUtils.getChildElementsByTagName(aspectElement, POINTCUT);
        for (Element pointcutElement : pointcuts) {
            parsePointcut(pointcutElement, parserContext);
        }

        parserContext.popAndRegisterContainingComponent();
    }
    finally {
        this.parseState.pop();
    }
}
```

### aspectj-autoproxy配置标签的解析

`<aop:aspectj-autoproxy/>`则由AspectJAutoProxyBeanDefinitionParser这个类处理的，我们看下parse 方法

```java
@Override
@Nullable
public BeanDefinition parse(Element element, ParserContext parserContext) {
    // 注册AspectJAnnotationAutoProxyCreator
    AopNamespaceUtils.registerAspectJAnnotationAutoProxyCreatorIfNecessary(parserContext, element);
    // 拓展BeanDefinition
    extendBeanDefinition(element, parserContext);
    return null;
}
```

AOP的创建工作是交给AnnotationAwareAspectJAutoProxyCreator来完成的。

### 注解切面代理创建类(AnnotationAwareAspectJAutoProxyCreator)

![](.\spring-springframework-aop-4.png)

#### postProcessBeforeInstantiation

```java
@Override
public Object postProcessBeforeInstantiation(Class<?> beanClass, String beanName) {
    Object cacheKey = getCacheKey(beanClass, beanName);

    if (!StringUtils.hasLength(beanName) || !this.targetSourcedBeans.contains(beanName)) {
        // 如果已经在缓存中，则忽略
        if (this.advisedBeans.containsKey(cacheKey)) {
            return null;
        }
        // 是否是aop基础类？是否跳过？
        if (isInfrastructureClass(beanClass) || shouldSkip(beanClass, beanName)) {
            this.advisedBeans.put(cacheKey, Boolean.FALSE);
            return null;
        }
    }

    // Create proxy here if we have a custom TargetSource.
    // Suppresses unnecessary default instantiation of the target bean:
    // The TargetSource will handle target instances in a custom fashion.
    TargetSource targetSource = getCustomTargetSource(beanClass, beanName);
    if (targetSource != null) {
        if (StringUtils.hasLength(beanName)) {
            this.targetSourcedBeans.add(beanName);
        }
        Object[] specificInterceptors = getAdvicesAndAdvisorsForBean(beanClass, beanName, targetSource);
        Object proxy = createProxy(beanClass, beanName, specificInterceptors, targetSource);
        this.proxyTypes.put(cacheKey, proxy.getClass());
        return proxy;
    }

    return null;
}
```

#### 切面方法转成Advisor

findCandidateAdvisors方法如下：

```java
@Override
protected List<Advisor> findCandidateAdvisors() {
    // 在父类中找到所有的advisor：基于xml配置的<aop:before/>生成的
    List<Advisor> advisors = super.findCandidateAdvisors();
    // 为bean Factory中AspectJ切面构建advistor：通过AspectJ注解的方式生成Advisor类
    if (this.aspectJAdvisorsBuilder != null) {
        advisors.addAll(this.aspectJAdvisorsBuilder.buildAspectJAdvisors());
    }
    return advisors;
}
```

在当前的bean Factory中通过AspectJ注解的方式生成Advisor类，buildAspectJAdvisors方法如下

```java
/**
    * Look for AspectJ-annotated aspect beans in the current bean factory,
    * and return to a list of Spring AOP Advisors representing them.
    * <p>Creates a Spring Advisor for each AspectJ advice method.
    * @return the list of {@link org.springframework.aop.Advisor} beans
    * @see #isEligibleBean
    */
public List<Advisor> buildAspectJAdvisors() {
    List<String> aspectNames = this.aspectBeanNames;

    if (aspectNames == null) {
        synchronized (this) {
            aspectNames = this.aspectBeanNames;
            if (aspectNames == null) {
                List<Advisor> advisors = new ArrayList<>();
                aspectNames = new ArrayList<>();
                String[] beanNames = BeanFactoryUtils.beanNamesForTypeIncludingAncestors(
                    this.beanFactory, Object.class, true, false);
                for (String beanName : beanNames) {
                    if (!isEligibleBean(beanName)) {
                        continue;
                    }
                    // We must be careful not to instantiate beans eagerly as in this case they
                    // would be cached by the Spring container but would not have been weaved.
                    Class<?> beanType = this.beanFactory.getType(beanName, false);
                    if (beanType == null) {
                        continue;
                    }
                    if (this.advisorFactory.isAspect(beanType)) {
                        aspectNames.add(beanName);
                        AspectMetadata amd = new AspectMetadata(beanType, beanName);
                        if (amd.getAjType().getPerClause().getKind() == PerClauseKind.SINGLETON) {
                            MetadataAwareAspectInstanceFactory factory =
                                new BeanFactoryAspectInstanceFactory(this.beanFactory, beanName);
                            List<Advisor> classAdvisors = this.advisorFactory.getAdvisors(factory);
                            // 单例加到advisorsCache, 非单例加到aspectFactoryCache
                            if (this.beanFactory.isSingleton(beanName)) {
                                this.advisorsCache.put(beanName, classAdvisors);
                            }
                            else {
                                this.aspectFactoryCache.put(beanName, factory);
                            }
                            advisors.addAll(classAdvisors);
                        }
                        else {
                            // Per target or per this.
                            if (this.beanFactory.isSingleton(beanName)) {
                                throw new IllegalArgumentException("Bean with name '" + beanName +
                                                                   "' is a singleton, but aspect instantiation model is not singleton");
                            }
                            MetadataAwareAspectInstanceFactory factory =
                                new PrototypeAspectInstanceFactory(this.beanFactory, beanName);
                            this.aspectFactoryCache.put(beanName, factory);
                            // advisorFactory工厂获取advisors
                            advisors.addAll(this.advisorFactory.getAdvisors(factory));
                        }
                    }
                }
                this.aspectBeanNames = aspectNames;
                return advisors;
            }
        }
    }

    if (aspectNames.isEmpty()) {
        return Collections.emptyList();
    }
    List<Advisor> advisors = new ArrayList<>();
    for (String aspectName : aspectNames) {
        List<Advisor> cachedAdvisors = this.advisorsCache.get(aspectName);
        if (cachedAdvisors != null) {
            advisors.addAll(cachedAdvisors);
        }
        else {
            MetadataAwareAspectInstanceFactory factory = this.aspectFactoryCache.get(aspectName);
            advisors.addAll(this.advisorFactory.getAdvisors(factory));
        }
    }
    return advisors;
}
```

上述方法本质上的思路是：用DCL双重锁的单例实现方式，拿到切面类里的切面方法，将其转换成advisor（并放入缓存中）。

转换的成advisor的方法是：this.advisorFactory.getAdvisor

```java
@Override
@Nullable
public Advisor getAdvisor(Method candidateAdviceMethod, MetadataAwareAspectInstanceFactory aspectInstanceFactory,
                          int declarationOrderInAspect, String aspectName) {

    validate(aspectInstanceFactory.getAspectMetadata().getAspectClass());

    AspectJExpressionPointcut expressionPointcut = getPointcut(
        candidateAdviceMethod, aspectInstanceFactory.getAspectMetadata().getAspectClass());
    if (expressionPointcut == null) {
        return null;
    }

    // 封装成advisor
    return new InstantiationModelAwarePointcutAdvisorImpl(expressionPointcut, candidateAdviceMethod,
                                        this, aspectInstanceFactory, declarationOrderInAspect, aspectName);
}
```

#### 获取表达式的切点

获取表达式的切点的方法getPointcut如下：

```java
@Nullable
private AspectJExpressionPointcut getPointcut(Method candidateAdviceMethod, Class<?> candidateAspectClass) {
    AspectJAnnotation<?> aspectJAnnotation =
            AbstractAspectJAdvisorFactory.findAspectJAnnotationOnMethod(candidateAdviceMethod);
    if (aspectJAnnotation == null) {
        return null;
    }

    AspectJExpressionPointcut ajexp =
            new AspectJExpressionPointcut(candidateAspectClass, new String[0], new Class<?>[0]);
    ajexp.setExpression(aspectJAnnotation.getPointcutExpression());
    if (this.beanFactory != null) {
        ajexp.setBeanFactory(this.beanFactory);
    }
    return ajexp;
}
```

AbstractAspectJAdvisorFactory.findAspectJAnnotationOnMethod的方法如下

```java
private static final Class<?>[] ASPECTJ_ANNOTATION_CLASSES = new Class<?>[] {
        Pointcut.class, Around.class, Before.class, After.class, AfterReturning.class, AfterThrowing.class};

/**
    * Find and return the first AspectJ annotation on the given method
    * (there <i>should</i> only be one anyway...).
    */
@SuppressWarnings("unchecked")
@Nullable
protected static AspectJAnnotation<?> findAspectJAnnotationOnMethod(Method method) {
    for (Class<?> clazz : ASPECTJ_ANNOTATION_CLASSES) {
        AspectJAnnotation<?> foundAnnotation = findAnnotation(method, (Class<Annotation>) clazz);
        if (foundAnnotation != null) {
            return foundAnnotation;
        }
    }
    return null;
}
```

findAnnotation方法如下

```java
@Nullable
private static <A extends Annotation> AspectJAnnotation<A> findAnnotation(Method method, Class<A> toLookFor) {
    A result = AnnotationUtils.findAnnotation(method, toLookFor);
    if (result != null) {
        return new AspectJAnnotation<>(result);
    }
    else {
        return null;
    }
}
```

AnnotationUtils.findAnnotation 获取注解方法如下

```java
/**
    * Find a single {@link Annotation} of {@code annotationType} on the supplied
    * {@link Method}, traversing its super methods (i.e. from superclasses and
    * interfaces) if the annotation is not <em>directly present</em> on the given
    * method itself.
    * <p>Correctly handles bridge {@link Method Methods} generated by the compiler.
    * <p>Meta-annotations will be searched if the annotation is not
    * <em>directly present</em> on the method.
    * <p>Annotations on methods are not inherited by default, so we need to handle
    * this explicitly.
    * @param method the method to look for annotations on
    * @param annotationType the annotation type to look for
    * @return the first matching annotation, or {@code null} if not found
    * @see #getAnnotation(Method, Class)
    */
@Nullable
public static <A extends Annotation> A findAnnotation(Method method, @Nullable Class<A> annotationType) {
    if (annotationType == null) {
        return null;
    }

    // Shortcut: directly present on the element, with no merging needed?
    if (AnnotationFilter.PLAIN.matches(annotationType) ||
            AnnotationsScanner.hasPlainJavaAnnotationsOnly(method)) {
        return method.getDeclaredAnnotation(annotationType);
    }

    // Exhaustive retrieval of merged annotations...
    return MergedAnnotations.from(method, SearchStrategy.TYPE_HIERARCHY, RepeatableContainers.none())
            .get(annotationType).withNonMergedAttributes()
            .synthesize(MergedAnnotation::isPresent).orElse(null);
}
```

#### 封装成Advisor

注：Advisor 是 advice的包装器，包含了advice及其它信息

由InstantiationModelAwarePointcutAdvisorImpl构造完成

```java
public InstantiationModelAwarePointcutAdvisorImpl(AspectJExpressionPointcut declaredPointcut,
        Method aspectJAdviceMethod, AspectJAdvisorFactory aspectJAdvisorFactory,
        MetadataAwareAspectInstanceFactory aspectInstanceFactory, int declarationOrder, String aspectName) {

    this.declaredPointcut = declaredPointcut;
    this.declaringClass = aspectJAdviceMethod.getDeclaringClass();
    this.methodName = aspectJAdviceMethod.getName();
    this.parameterTypes = aspectJAdviceMethod.getParameterTypes();
    this.aspectJAdviceMethod = aspectJAdviceMethod;
    this.aspectJAdvisorFactory = aspectJAdvisorFactory;
    this.aspectInstanceFactory = aspectInstanceFactory;
    this.declarationOrder = declarationOrder;
    this.aspectName = aspectName;

    if (aspectInstanceFactory.getAspectMetadata().isLazilyInstantiated()) {
        // Static part of the pointcut is a lazy type.
        Pointcut preInstantiationPointcut = Pointcuts.union(
                aspectInstanceFactory.getAspectMetadata().getPerClausePointcut(), this.declaredPointcut);

        // Make it dynamic: must mutate from pre-instantiation to post-instantiation state.
        // If it's not a dynamic pointcut, it may be optimized out
        // by the Spring AOP infrastructure after the first evaluation.
        this.pointcut = new PerTargetInstantiationModelPointcut(
                this.declaredPointcut, preInstantiationPointcut, aspectInstanceFactory);
        this.lazy = true;
    }
    else {
        // A singleton aspect.
        this.pointcut = this.declaredPointcut;
        this.lazy = false;
        this.instantiatedAdvice = instantiateAdvice(this.declaredPointcut);
    }
}
```

通过pointcut获取advice

```java
private Advice instantiateAdvice(AspectJExpressionPointcut pointcut) {
    Advice advice = this.aspectJAdvisorFactory.getAdvice(this.aspectJAdviceMethod, pointcut,
            this.aspectInstanceFactory, this.declarationOrder, this.aspectName);
    return (advice != null ? advice : EMPTY_ADVICE);
}
```

交给aspectJAdvisorFactory获取

```java
@Override
@Nullable
public Advice getAdvice(Method candidateAdviceMethod, AspectJExpressionPointcut expressionPointcut,
        MetadataAwareAspectInstanceFactory aspectInstanceFactory, int declarationOrder, String aspectName) {

    // 获取切面类
    Class<?> candidateAspectClass = aspectInstanceFactory.getAspectMetadata().getAspectClass();
    validate(candidateAspectClass);

    // 获取切面注解
    AspectJAnnotation<?> aspectJAnnotation =
            AbstractAspectJAdvisorFactory.findAspectJAnnotationOnMethod(candidateAdviceMethod);
    if (aspectJAnnotation == null) {
        return null;
    }

    // If we get here, we know we have an AspectJ method.
    // Check that it's an AspectJ-annotated class
    if (!isAspect(candidateAspectClass)) {
        throw new AopConfigException("Advice must be declared inside an aspect type: " +
                "Offending method '" + candidateAdviceMethod + "' in class [" +
                candidateAspectClass.getName() + "]");
    }

    if (logger.isDebugEnabled()) {
        logger.debug("Found AspectJ method: " + candidateAdviceMethod);
    }

    // 切面注解转换成advice
    AbstractAspectJAdvice springAdvice;

    switch (aspectJAnnotation.getAnnotationType()) {
        case AtPointcut: // AtPointcut忽略
            if (logger.isDebugEnabled()) {
                logger.debug("Processing pointcut '" + candidateAdviceMethod.getName() + "'");
            }
            return null;
        case AtAround:
            springAdvice = new AspectJAroundAdvice(
                    candidateAdviceMethod, expressionPointcut, aspectInstanceFactory);
            break;
        case AtBefore:
            springAdvice = new AspectJMethodBeforeAdvice(
                    candidateAdviceMethod, expressionPointcut, aspectInstanceFactory);
            break;
        case AtAfter:
            springAdvice = new AspectJAfterAdvice(
                    candidateAdviceMethod, expressionPointcut, aspectInstanceFactory);
            break;
        case AtAfterReturning:
            springAdvice = new AspectJAfterReturningAdvice(
                    candidateAdviceMethod, expressionPointcut, aspectInstanceFactory);
            AfterReturning afterReturningAnnotation = (AfterReturning) aspectJAnnotation.getAnnotation();
            if (StringUtils.hasText(afterReturningAnnotation.returning())) {
                springAdvice.setReturningName(afterReturningAnnotation.returning());
            }
            break;
        case AtAfterThrowing:
            springAdvice = new AspectJAfterThrowingAdvice(
                    candidateAdviceMethod, expressionPointcut, aspectInstanceFactory);
            AfterThrowing afterThrowingAnnotation = (AfterThrowing) aspectJAnnotation.getAnnotation();
            if (StringUtils.hasText(afterThrowingAnnotation.throwing())) {
                springAdvice.setThrowingName(afterThrowingAnnotation.throwing());
            }
            break;
        default:
            throw new UnsupportedOperationException(
                    "Unsupported advice type on method: " + candidateAdviceMethod);
    }

    // 最后将其它切面信息配置到advice
    springAdvice.setAspectName(aspectName);
    springAdvice.setDeclarationOrder(declarationOrder);
    String[] argNames = this.parameterNameDiscoverer.getParameterNames(candidateAdviceMethod);
    if (argNames != null) {
        springAdvice.setArgumentNamesFromStringArray(argNames);
    }
    springAdvice.calculateArgumentBindings();

    return springAdvice;
}
```

### postProcessAfterInitialization

有了Adisor, 注入到合适的位置并交给代理（cglib和jdk)实现了。有了Adisor, 注入到合适的位置并交给代理（cglib和jdk)实现了。

```java
/**
* Create a proxy with the configured interceptors if the bean is
* identified as one to proxy by the subclass.
* @see #getAdvicesAndAdvisorsForBean
*/
@Override
public Object postProcessAfterInitialization(@Nullable Object bean, String beanName) {
    if (bean != null) {
        Object cacheKey = getCacheKey(bean.getClass(), beanName);
        // 如果不是提前暴露的代理
        if (this.earlyProxyReferences.remove(cacheKey) != bean) {
            return wrapIfNecessary(bean, beanName, cacheKey);
        }
    }
    return bean;
}
```

wrapIfNecessary方法主要用于判断是否需要创建代理，如果Bean能够获取到advisor才需要创建代理

```java
/**
  * Wrap the given bean if necessary, i.e. if it is eligible for being proxied.
  * @param bean the raw bean instance
  * @param beanName the name of the bean
  * @param cacheKey the cache key for metadata access
  * @return a proxy wrapping the bean, or the raw bean instance as-is
  */
protected Object wrapIfNecessary(Object bean, String beanName, Object cacheKey) {
   // 如果bean是通过TargetSource接口获取
   if (beanName != null && this.targetSourcedBeans.contains(beanName)) {
      return bean;
   }
   // 如果bean是切面类
   if (Boolean.FALSE.equals(this.advisedBeans.get(cacheKey))) {
      return bean;
   }
   // 如果是aop基础类？是否跳过？
   if (isInfrastructureClass(bean.getClass()) || shouldSkip(bean.getClass(), beanName)) {
      this.advisedBeans.put(cacheKey, Boolean.FALSE);
      return bean;
   }

  // 重点：获取所有advisor，如果没有获取到，那说明不要进行增强，也就不需要代理了。
  Object[] specificInterceptors = getAdvicesAndAdvisorsForBean(bean.getClass(), beanName, null);
  if (specificInterceptors != DO_NOT_PROXY) {
    this.advisedBeans.put(cacheKey, Boolean.TRUE);
    // 重点：创建代理
    Object proxy = createProxy(
        bean.getClass(), beanName, specificInterceptors, new SingletonTargetSource(bean));
    this.proxyTypes.put(cacheKey, proxy.getClass());
    return proxy;
  }

  this.advisedBeans.put(cacheKey, Boolean.FALSE);
  return bean;
}
```

#### 获取所有的Advisor

我们看下获取所有advisor的方法getAdvicesAndAdvisorsForBean

```java
@Override
@Nullable
protected Object[] getAdvicesAndAdvisorsForBean(
    Class<?> beanClass, String beanName, @Nullable TargetSource targetSource) {

  List<Advisor> advisors = findEligibleAdvisors(beanClass, beanName);
  if (advisors.isEmpty()) {
    return DO_NOT_PROXY;
  }
  return advisors.toArray();
}
```

通过findEligibleAdvisors方法获取advisor， 如果获取不到返回DO_NOT_PROXY（不需要创建代理），findEligibleAdvisors方法如下

```java
/**
  * Find all eligible Advisors for auto-proxying this class.
  * @param beanClass the clazz to find advisors for
  * @param beanName the name of the currently proxied bean
  * @return the empty List, not {@code null},
  * if there are no pointcuts or interceptors
  * @see #findCandidateAdvisors
  * @see #sortAdvisors
  * @see #extendAdvisors
  */
protected List<Advisor> findEligibleAdvisors(Class<?> beanClass, String beanName) {
  // 和上文一样，获取所有切面类的切面方法生成Advisor
  List<Advisor> candidateAdvisors = findCandidateAdvisors();
  // 找到这些Advisor中能够应用于beanClass的Advisor
  List<Advisor> eligibleAdvisors = findAdvisorsThatCanApply(candidateAdvisors, beanClass, beanName);
  // 如果需要，交给子类拓展
  extendAdvisors(eligibleAdvisors);
  // 对Advisor排序
  if (!eligibleAdvisors.isEmpty()) {
    eligibleAdvisors = sortAdvisors(eligibleAdvisors);
  }
  return eligibleAdvisors;
}
```

获取所有切面类的切面方法生成Advisor

```java
/**
  * Find all candidate Advisors to use in auto-proxying.
  * @return the List of candidate Advisors
  */
protected List<Advisor> findCandidateAdvisors() {
  Assert.state(this.advisorRetrievalHelper != null, "No BeanFactoryAdvisorRetrievalHelper available");
  return this.advisorRetrievalHelper.findAdvisorBeans();
}
```

找到这些Advisor中能够应用于beanClass的Advisor

```java
/**
  * Determine the sublist of the {@code candidateAdvisors} list
  * that is applicable to the given class.
  * @param candidateAdvisors the Advisors to evaluate
  * @param clazz the target class
  * @return sublist of Advisors that can apply to an object of the given class
  * (may be the incoming List as-is)
  */
public static List<Advisor> findAdvisorsThatCanApply(List<Advisor> candidateAdvisors, Class<?> clazz) {
  if (candidateAdvisors.isEmpty()) {
    return candidateAdvisors;
  }
  List<Advisor> eligibleAdvisors = new ArrayList<>();
  for (Advisor candidate : candidateAdvisors) {
    // 通过Introduction实现的advice
    if (candidate instanceof IntroductionAdvisor && canApply(candidate, clazz)) {
      eligibleAdvisors.add(candidate);
    }
  }
  boolean hasIntroductions = !eligibleAdvisors.isEmpty();
  for (Advisor candidate : candidateAdvisors) {
    if (candidate instanceof IntroductionAdvisor) {
      // already processed
      continue;
    }
    // 是否能够应用于clazz的Advice
    if (canApply(candidate, clazz, hasIntroductions)) {
      eligibleAdvisors.add(candidate);
    }
  }
  return eligibleAdvisors;
}
```

#### 创建代理的入口方法

获取所有advisor后，如果有advisor，则说明需要增强，即需要创建代理，创建代理的方法如下：

```java
/**
  * Create an AOP proxy for the given bean.
  * @param beanClass the class of the bean
  * @param beanName the name of the bean
  * @param specificInterceptors the set of interceptors that is
  * specific to this bean (may be empty, but not null)
  * @param targetSource the TargetSource for the proxy,
  * already pre-configured to access the bean
  * @return the AOP proxy for the bean
  * @see #buildAdvisors
  */
protected Object createProxy(Class<?> beanClass, @Nullable String beanName,
    @Nullable Object[] specificInterceptors, TargetSource targetSource) {

  if (this.beanFactory instanceof ConfigurableListableBeanFactory) {
    AutoProxyUtils.exposeTargetClass((ConfigurableListableBeanFactory) this.beanFactory, beanName, beanClass);
  }

  ProxyFactory proxyFactory = new ProxyFactory();
  proxyFactory.copyFrom(this);

  if (proxyFactory.isProxyTargetClass()) {
    // Explicit handling of JDK proxy targets (for introduction advice scenarios)
    if (Proxy.isProxyClass(beanClass)) {
      // Must allow for introductions; can't just set interfaces to the proxy's interfaces only.
      for (Class<?> ifc : beanClass.getInterfaces()) {
        proxyFactory.addInterface(ifc);
      }
    }
  }
  else {
    // No proxyTargetClass flag enforced, let's apply our default checks...
    if (shouldProxyTargetClass(beanClass, beanName)) {
      proxyFactory.setProxyTargetClass(true);
    }
    else {
      evaluateProxyInterfaces(beanClass, proxyFactory);
    }
  }

  Advisor[] advisors = buildAdvisors(beanName, specificInterceptors);
  proxyFactory.addAdvisors(advisors);
  proxyFactory.setTargetSource(targetSource);
  customizeProxyFactory(proxyFactory);

  proxyFactory.setFrozen(this.freezeProxy);
  if (advisorsPreFiltered()) {
    proxyFactory.setPreFiltered(true);
  }

  // Use original ClassLoader if bean class not locally loaded in overriding class loader
  ClassLoader classLoader = getProxyClassLoader();
  if (classLoader instanceof SmartClassLoader && classLoader != beanClass.getClassLoader()) {
    classLoader = ((SmartClassLoader) classLoader).getOriginalClassLoader();
  }
  return proxyFactory.getProxy(classLoader);
}
```

#### 依据条件创建代理(jdk或cglib)

DefaultAopProxyFactory.createAopProxy

```java
@Override
public AopProxy createAopProxy(AdvisedSupport config) throws AopConfigException {
  if (!NativeDetector.inNativeImage() &&
      (config.isOptimize() || config.isProxyTargetClass() || hasNoUserSuppliedProxyInterfaces(config))) {
    Class<?> targetClass = config.getTargetClass();
    if (targetClass == null) {
      throw new AopConfigException("TargetSource cannot determine target class: " +
          "Either an interface or a target is required for proxy creation.");
    }
    if (targetClass.isInterface() || Proxy.isProxyClass(targetClass)) {
      return new JdkDynamicAopProxy(config);
    }
    return new ObjenesisCglibAopProxy(config);
  }
  else {
    return new JdkDynamicAopProxy(config);
  }
}
```

几个要点

- config.isOptimize() 是通过optimize设置，表示配置是自定义的，默认是false；
- config.isProxyTargetClass()是通过`<aop:config proxy-target-class="true" /> `来配置的，表示优先使用cglib代理，默认是false；
- hasNoUserSuppliedProxyInterfaces(config) 表示是否目标类实现了接口

由此我们可以知道：

Spring默认在目标类实现接口时是通过JDK代理实现的，只有非接口的是通过Cglib代理实现的。当设置proxy-target-class为true时在目标类不是接口或者代理类时优先使用cglib代理实现。

### SpringAOP中Cglib代理的实现

我们看下CglibAopProxy的getProxy方法

```java
@Override
public Object getProxy() {
  return getProxy(null);
}

@Override
public Object getProxy(@Nullable ClassLoader classLoader) {
  if (logger.isTraceEnabled()) {
    logger.trace("Creating CGLIB proxy: " + this.advised.getTargetSource());
  }

  try {
    Class<?> rootClass = this.advised.getTargetClass();
    Assert.state(rootClass != null, "Target class must be available for creating a CGLIB proxy");

    // 上面流程图中的目标类
    Class<?> proxySuperClass = rootClass;
    if (rootClass.getName().contains(ClassUtils.CGLIB_CLASS_SEPARATOR)) {
      proxySuperClass = rootClass.getSuperclass();
      Class<?>[] additionalInterfaces = rootClass.getInterfaces();
      for (Class<?> additionalInterface : additionalInterfaces) {
        this.advised.addInterface(additionalInterface);
      }
    }

    // Validate the class, writing log messages as necessary.
    validateClassIfNecessary(proxySuperClass, classLoader);

    // 重点看这里，就是上图的enhancer，设置各种参数来构建
    Enhancer enhancer = createEnhancer();
    if (classLoader != null) {
      enhancer.setClassLoader(classLoader);
      if (classLoader instanceof SmartClassLoader &&
          ((SmartClassLoader) classLoader).isClassReloadable(proxySuperClass)) {
        enhancer.setUseCache(false);
      }
    }
    enhancer.setSuperclass(proxySuperClass);
    enhancer.setInterfaces(AopProxyUtils.completeProxiedInterfaces(this.advised));
    enhancer.setNamingPolicy(SpringNamingPolicy.INSTANCE);
    enhancer.setStrategy(new ClassLoaderAwareGeneratorStrategy(classLoader));

    // 设置callback回调接口，即方法的增强点
    Callback[] callbacks = getCallbacks(rootClass);
    Class<?>[] types = new Class<?>[callbacks.length];
    for (int x = 0; x < types.length; x++) {
      types[x] = callbacks[x].getClass();
    }
    // 上节说到的filter
    enhancer.setCallbackFilter(new ProxyCallbackFilter(
        this.advised.getConfigurationOnlyCopy(), this.fixedInterceptorMap, this.fixedInterceptorOffset));
    enhancer.setCallbackTypes(types);

    // 重点：创建proxy和其实例
    return createProxyClassAndInstance(enhancer, callbacks);
  }
  catch (CodeGenerationException | IllegalArgumentException ex) {
    throw new AopConfigException("Could not generate CGLIB subclass of " + this.advised.getTargetClass() +
        ": Common causes of this problem include using a final class or a non-visible class",
        ex);
  }
  catch (Throwable ex) {
    // TargetSource.getTarget() failed
    throw new AopConfigException("Unexpected AOP exception", ex);
  }
}
```

- `rootClass`: 即目标代理类
- `advised`: 包含上文中我们获取到的advisor增强器的集合
- `exposeProxy`: 在xml配置文件中配置的，背景就是如果在事务A中使用了代理，事务A调用了目标类的的方法a，在方法a中又调用目标类的方法b，方法a，b同时都是要被增强的方法，如果不配置exposeProxy属性，方法b的增强将会失效，如果配置exposeProxy，方法b在方法a的执行中也会被增强了
- `DynamicAdvisedInterceptor`: 拦截器将advised(包含上文中我们获取到的advisor增强器)构建配置的AOP的callback（第一个callback)
- `targetInterceptor`: xml配置的optimize属性使用的（第二个callback)
- 最后连同其它5个默认的Interceptor 返回作为cglib的拦截器链，之后通过CallbackFilter的accpet方法返回的索引从这个集合中返回对应的拦截增强器执行增强操作。

```java
private Callback[] getCallbacks(Class<?> rootClass) throws Exception {
  // Parameters used for optimization choices...
  boolean exposeProxy = this.advised.isExposeProxy();
  boolean isFrozen = this.advised.isFrozen();
  boolean isStatic = this.advised.getTargetSource().isStatic();

  // Choose an "aop" interceptor (used for AOP calls).
  Callback aopInterceptor = new DynamicAdvisedInterceptor(this.advised);

  // Choose a "straight to target" interceptor. (used for calls that are
  // unadvised but can return this). May be required to expose the proxy.
  Callback targetInterceptor;
  if (exposeProxy) {
    targetInterceptor = (isStatic ?
        new StaticUnadvisedExposedInterceptor(this.advised.getTargetSource().getTarget()) :
        new DynamicUnadvisedExposedInterceptor(this.advised.getTargetSource()));
  }
  else {
    targetInterceptor = (isStatic ?
        new StaticUnadvisedInterceptor(this.advised.getTargetSource().getTarget()) :
        new DynamicUnadvisedInterceptor(this.advised.getTargetSource()));
  }

  // Choose a "direct to target" dispatcher (used for
  // unadvised calls to static targets that cannot return this).
  Callback targetDispatcher = (isStatic ?
      new StaticDispatcher(this.advised.getTargetSource().getTarget()) : new SerializableNoOp());

  Callback[] mainCallbacks = new Callback[] {
      aopInterceptor,  // 
      targetInterceptor,  // invoke target without considering advice, if optimized
      new SerializableNoOp(),  // no override for methods mapped to this
      targetDispatcher, this.advisedDispatcher,
      new EqualsInterceptor(this.advised),
      new HashCodeInterceptor(this.advised)
  };

  Callback[] callbacks;

  // If the target is a static one and the advice chain is frozen,
  // then we can make some optimizations by sending the AOP calls
  // direct to the target using the fixed chain for that method.
  if (isStatic && isFrozen) {
    Method[] methods = rootClass.getMethods();
    Callback[] fixedCallbacks = new Callback[methods.length];
    this.fixedInterceptorMap = CollectionUtils.newHashMap(methods.length);

    // TODO: small memory optimization here (can skip creation for methods with no advice)
    for (int x = 0; x < methods.length; x++) {
      Method method = methods[x];
      List<Object> chain = this.advised.getInterceptorsAndDynamicInterceptionAdvice(method, rootClass);
      fixedCallbacks[x] = new FixedChainStaticTargetInterceptor(
          chain, this.advised.getTargetSource().getTarget(), this.advised.getTargetClass());
      this.fixedInterceptorMap.put(method, x);
    }

    // Now copy both the callbacks from mainCallbacks
    // and fixedCallbacks into the callbacks array.
    callbacks = new Callback[mainCallbacks.length + fixedCallbacks.length];
    System.arraycopy(mainCallbacks, 0, callbacks, 0, mainCallbacks.length);
    System.arraycopy(fixedCallbacks, 0, callbacks, mainCallbacks.length, fixedCallbacks.length);
    this.fixedInterceptorOffset = mainCallbacks.length;
  }
  else {
    callbacks = mainCallbacks;
  }
  return callbacks;
}
```

### JDK代理的流程

JDK代理自动生成的class是由sun.misc.ProxyGenerator来生成的。我们看下sun.misc.ProxyGenerator生成代码的逻辑：

```java
/**
    * Generate a proxy class given a name and a list of proxy interfaces.
    *
    * @param name        the class name of the proxy class
    * @param interfaces  proxy interfaces
    * @param accessFlags access flags of the proxy class
*/
public static byte[] generateProxyClass(final String name,
                                        Class<?>[] interfaces,
                                        int accessFlags)
{
    ProxyGenerator gen = new ProxyGenerator(name, interfaces, accessFlags);
    final byte[] classFile = gen.generateClassFile();
    ...
}
```

generateClassFile方法如下：

```java
/**
    * Generate a class file for the proxy class.  This method drives the
    * class file generation process.
    */
private byte[] generateClassFile() {

    /* 第一步：将所有方法包装成ProxyMethod对象 */
    
    // 将Object类中hashCode、equals、toString方法包装成ProxyMethod对象
    addProxyMethod(hashCodeMethod, Object.class);
    addProxyMethod(equalsMethod, Object.class);
    addProxyMethod(toStringMethod, Object.class);

    // 将代理类接口方法包装成ProxyMethod对象
    for (Class<?> intf : interfaces) {
        for (Method m : intf.getMethods()) {
            addProxyMethod(m, intf);
        }
    }

    // 校验返回类型
    for (List<ProxyMethod> sigmethods : proxyMethods.values()) {
        checkReturnTypes(sigmethods);
    }

    /* 第二步：为代理类组装字段，构造函数，方法，static初始化块等 */
    try {
        // 添加构造函数，参数是InvocationHandler
        methods.add(generateConstructor());

        // 代理方法
        for (List<ProxyMethod> sigmethods : proxyMethods.values()) {
            for (ProxyMethod pm : sigmethods) {

                // 字段
                fields.add(new FieldInfo(pm.methodFieldName,
                    "Ljava/lang/reflect/Method;",
                        ACC_PRIVATE | ACC_STATIC));

                // 上述ProxyMethod中的方法
                methods.add(pm.generateMethod());
            }
        }

        // static初始化块
        methods.add(generateStaticInitializer());

    } catch (IOException e) {
        throw new InternalError("unexpected I/O Exception", e);
    }

    if (methods.size() > 65535) {
        throw new IllegalArgumentException("method limit exceeded");
    }
    if (fields.size() > 65535) {
        throw new IllegalArgumentException("field limit exceeded");
    }

    /* 第三步：写入class文件 */

    /*
        * Make sure that constant pool indexes are reserved for the
        * following items before starting to write the final class file.
        */
    cp.getClass(dotToSlash(className));
    cp.getClass(superclassName);
    for (Class<?> intf: interfaces) {
        cp.getClass(dotToSlash(intf.getName()));
    }

    /*
        * Disallow new constant pool additions beyond this point, since
        * we are about to write the final constant pool table.
        */
    cp.setReadOnly();

    ByteArrayOutputStream bout = new ByteArrayOutputStream();
    DataOutputStream dout = new DataOutputStream(bout);

    try {
        /*
            * Write all the items of the "ClassFile" structure.
            * See JVMS section 4.1.
            */
                                    // u4 magic;
        dout.writeInt(0xCAFEBABE);
                                    // u2 minor_version;
        dout.writeShort(CLASSFILE_MINOR_VERSION);
                                    // u2 major_version;
        dout.writeShort(CLASSFILE_MAJOR_VERSION);

        cp.write(dout);             // (write constant pool)

                                    // u2 access_flags;
        dout.writeShort(accessFlags);
                                    // u2 this_class;
        dout.writeShort(cp.getClass(dotToSlash(className)));
                                    // u2 super_class;
        dout.writeShort(cp.getClass(superclassName));

                                    // u2 interfaces_count;
        dout.writeShort(interfaces.length);
                                    // u2 interfaces[interfaces_count];
        for (Class<?> intf : interfaces) {
            dout.writeShort(cp.getClass(
                dotToSlash(intf.getName())));
        }

                                    // u2 fields_count;
        dout.writeShort(fields.size());
                                    // field_info fields[fields_count];
        for (FieldInfo f : fields) {
            f.write(dout);
        }

                                    // u2 methods_count;
        dout.writeShort(methods.size());
                                    // method_info methods[methods_count];
        for (MethodInfo m : methods) {
            m.write(dout);
        }

                                        // u2 attributes_count;
        dout.writeShort(0); // (no ClassFile attributes for proxy classes)

    } catch (IOException e) {
        throw new InternalError("unexpected I/O Exception", e);
    }

    return bout.toByteArray();
}
```

###  SpringAOP中JDK代理的实现

SpringAOP扮演的是JDK代理的创建和调用两个角色，我们通过这两个方向来看下SpringAOP的代码（JdkDynamicAopProxy类）

#### SpringAOP Jdk代理的创建

代理的创建比较简单，调用getProxy方法，然后直接调用JDK中Proxy.newProxyInstance()方法将classloader和被代理的接口方法传入即可。

```java
@Override
public Object getProxy() {
    return getProxy(ClassUtils.getDefaultClassLoader());
}

@Override
public Object getProxy(@Nullable ClassLoader classLoader) {
    if (logger.isTraceEnabled()) {
        logger.trace("Creating JDK dynamic proxy: " + this.advised.getTargetSource());
    }
    return Proxy.newProxyInstance(classLoader, this.proxiedInterfaces, this);
}
```

#### SpringAOP Jdk代理的执行

执行的方法如下：

```java
/**
    * Implementation of {@code InvocationHandler.invoke}.
    * <p>Callers will see exactly the exception thrown by the target,
    * unless a hook method throws an exception.
    */
@Override
@Nullable
public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
    Object oldProxy = null;
    boolean setProxyContext = false;

    TargetSource targetSource = this.advised.targetSource;
    Object target = null;

    try {
        // 执行的是equal方法
        if (!this.equalsDefined && AopUtils.isEqualsMethod(method)) {
            // The target does not implement the equals(Object) method itself.
            return equals(args[0]);
        }
        // 执行的是hashcode方法
        else if (!this.hashCodeDefined && AopUtils.isHashCodeMethod(method)) {
            // The target does not implement the hashCode() method itself.
            return hashCode();
        }
        // 如果是包装类，则dispatch to proxy config
        else if (method.getDeclaringClass() == DecoratingProxy.class) {
            // There is only getDecoratedClass() declared -> dispatch to proxy config.
            return AopProxyUtils.ultimateTargetClass(this.advised);
        }
        // 用反射方式来执行切点
        else if (!this.advised.opaque && method.getDeclaringClass().isInterface() &&
                method.getDeclaringClass().isAssignableFrom(Advised.class)) {
            // Service invocations on ProxyConfig with the proxy config...
            return AopUtils.invokeJoinpointUsingReflection(this.advised, method, args);
        }

        Object retVal;

        if (this.advised.exposeProxy) {
            // Make invocation available if necessary.
            oldProxy = AopContext.setCurrentProxy(proxy);
            setProxyContext = true;
        }

        // Get as late as possible to minimize the time we "own" the target,
        // in case it comes from a pool.
        target = targetSource.getTarget();
        Class<?> targetClass = (target != null ? target.getClass() : null);

        // 获取拦截链
        List<Object> chain = this.advised.getInterceptorsAndDynamicInterceptionAdvice(method, targetClass);

        // Check whether we have any advice. If we don't, we can fallback on direct
        // reflective invocation of the target, and avoid creating a MethodInvocation.
        if (chain.isEmpty()) {
            // We can skip creating a MethodInvocation: just invoke the target directly
            // Note that the final invoker must be an InvokerInterceptor so we know it does
            // nothing but a reflective operation on the target, and no hot swapping or fancy proxying.
            Object[] argsToUse = AopProxyUtils.adaptArgumentsIfNecessary(method, args);
            retVal = AopUtils.invokeJoinpointUsingReflection(target, method, argsToUse);
        }
        else {
            // We need to create a method invocation...
            MethodInvocation invocation =
                    new ReflectiveMethodInvocation(proxy, target, method, args, targetClass, chain);
            // Proceed to the joinpoint through the interceptor chain.
            retVal = invocation.proceed();
        }

        // Massage return value if necessary.
        Class<?> returnType = method.getReturnType();
        if (retVal != null && retVal == target &&
                returnType != Object.class && returnType.isInstance(proxy) &&
                !RawTargetAccess.class.isAssignableFrom(method.getDeclaringClass())) {
            // Special case: it returned "this" and the return type of the method
            // is type-compatible. Note that we can't help if the target sets
            // a reference to itself in another returned object.
            retVal = proxy;
        }
        else if (retVal == null && returnType != Void.TYPE && returnType.isPrimitive()) {
            throw new AopInvocationException(
                    "Null return value from advice does not match primitive return type for: " + method);
        }
        return retVal;
    }
    finally {
        if (target != null && !targetSource.isStatic()) {
            // Must have come from TargetSource.
            targetSource.releaseTarget(target);
        }
        if (setProxyContext) {
            // Restore old proxy.
            AopContext.setCurrentProxy(oldProxy);
        }
    }
}
```

## Spring事务原理探究