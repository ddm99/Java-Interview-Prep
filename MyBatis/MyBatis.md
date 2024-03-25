[toc]



# JDBC

## 简介 

JDBC(Java Database Connectivity)是一个独立于特定数据库管理系统、通用的SQL数据库存取和操作的公共接口（一组API），定义了用来访问数据库的标准 Java 类库，（`java.sql`, `javax.sql`）使用这些类库可以以一种标准的方法、方便地访问数据库资源。

JDBC 的目标是使Java程序员使用 JDBC 可以连接任何提供了JDBC驱动程序的数据库系统，这样就使得程序员无需对特定的数据库系统的特点有过多的了解，从而大大简化和加快了开发过程。

![](.\JDBC.png)

> ODBC(Open Database Connectivity，开放式数据库连接)，是微软在 Windows 平台下推出的。使用者在程序中只需要调用 ODBC API，由 ODBC 驱动程序将调用转换成为对特定的数据库的调用请求。
>



## 数据库流程

1. 注册驱动，获取连接
2. 创建`Statement`对象
3. `execute()`方法执行sql
4. 把结果集转换成POJO对象
5. 关闭资源



## Driver 接口

`java.sql.Driver` 接口是所有 JDBC 驱动程序需要实现的接口。这个接口是提供给数据库厂商使用的，不同数据库厂商提供不同的实现。在程序中不需要直接去访问实现了 `Driver` 接口的类，而是由驱动程序管理器类(`java.sql.DriverManager`)去调用这些`Driver`实现。

### 加载与注册JDBC驱动

加载驱动：加载 JDBC 驱动，并将其注册进`DriverManager`。因为 `Driver` 接口的驱动程序类都包含了静态代码块，在这个静态代码块中，会调用 `DriverManager.registerDriver()` 方法来注册自身的一个实例。

```java
Class.forName("com.mysql.jdbc.Driver");
```

### 连接到JDBC

```java
@Test
public void testConnection4() {
    try {
        //1.数据库连接的4个基本要素：
        String url = "jdbc:mysql://localhost:3306/test";
        String user = "root";
        String password = "abc123";
        String driverName = "com.mysql.jdbc.Driver";

        //2.加载驱动 （①实例化Driver ②注册驱动）
        Class.forName(driverName);


        //3.获取连接
        Connection conn = DriverManager.getConnection(url, user, password);
        System.out.println(conn);
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```



## PreparedStatement

`PreparedStatement` 接口是 `Statement` 的子接口，它表示一条预编译过的 SQL 语句。`PreparedStatement` 能最大可能提高性能：

- DBServer 会对预编译语句提供性能优化。因为预编译语句有可能被重复调用，所以语句在被 DBServer 的编译器编译后的执行代码被缓存下来，那么下次调用时只要是相同的预编译语句就不需要编译，只要将参数直接传入编译过的语句执行代码中就会得到执行。
- 在 statement 语句中,即使是相同操作但因为数据内容不一样,所以整个语句本身不能匹配,没有缓存语句的意义.事实是没有数据库会对普通语句编译后的执行代码缓存。这样每执行一次都要对传入的语句编译一次。
- 语法检查，语义检查，翻译成二进制命令，缓存

`````java
//Create a PreparedStatement:  Prepare your SQL statement with parameter placeholders (indicated by question marks ?)
String sql = "INSERT INTO customers (name, address, city) VALUES (?, ?, ?)"; 
PreparedStatement pstmt = conn.prepareStatement(sql);

//Set Parameter Values:  Use pstmt's setXXX() methods to assign values to the placeholders, where 'XXX' corresponds to the data type
pstmt.setString(1, "Alice Johnson");  // Set the first parameter to a String
pstmt.setString(2, "123 Main Street");
pstmt.setString(3, "New York");

//Execute the Statement and Process Results
ResultSet rs = pstmt.executeQuery();
while (rs.next()) {
     String name = rs.getString("name"); 
     String address = rs.getString("address");
     // ...
}
`````



## ResultSet

- 查询需要调用 `PreparedStatement` 的 `executeQuery()` 方法，查询结果是一个 ResultSet 对象
- `ResultSet` 返回的实际上就是一张数据表。有一个指针指向数据表的第一条记录的前面。
- `ResultSet` 对象维护了一个指向当前数据行的游标，初始的时候，游标在第一行之前，可以通过 `ResultSet` 对象的 `next()` 方法移动到下一行。调用 `next()` 方法检测下一行是否有效。若有效，该方法返回 true，且指针下移。相当于`Iterator`对象的 `hasNext()` 和 `next()` 方法的结合体。
- 当指针指向一行时, 可以通过调用 `getXxx(int index)` 或 `getXxx(int columnName)` 获取每一列的值。



## 资源的释放

* 释放 `ResultSet`, `Statement`, `Connection`。
* 数据库连接（Connection）是非常稀有的资源，用完后必须马上释放，如果Connection不能及时正确的关闭将导致系统宕机。Connection的使用原则是尽量晚创建，尽量早的释放。
* 可以在 `finally` 中关闭，保证及时其他代码出现异常，资源也一定能被关闭。



## JDBC的缺点

* 重复代码太多，降低开发效率（比较繁琐，有些代码没有必要重复）例如上面获取name和address需要重复调用`rs.getString()`
* 在JDBC开发中sql语句是编写在java程序中的，sql语句不支持配置。当SQL语句后期需要调优时，SQL语句被修改的概率是很高的。在Java程序中编写SQL语句，后期修改，就得修改Java源代码，这就导致代码需要重新编译，项目需要重新部署等操作。违背OCP原则
* 需要手动管理资源
* 结果集需要手动处理



# Spring JDBCTemplate

Spring 提供了一个 Spring JDBC 模块，它对 JDBC API 进行了封装，其的主要目的降低 JDBC API 的使用难度，以一种更直接、更简洁的方式使用 JDBC API。

使用 Spring JDBC，开发人员只需要定义必要的参数、指定需要执行的 SQL 语句，即可轻松的进行 JDBC 编程，对数据库进行访问。

至于驱动的加载、数据库连接的开启与关闭、SQL 语句的创建与执行、异常处理以及事务处理等繁杂乏味的工作，则都是由 Spring JDBC 完成的。这样就可以使开发人员从繁琐的 JDBC API 中解脱出来，有更多的精力专注于业务的开发。

JdbcTemplate 是 Spring JDBC 核心包（core）中的核心类，它可以通过配置文件、注解、Java 配置类等形式获取数据库的相关信息，实现了对 JDBC 开发过程中的驱动加载、连接的开启和关闭、SQL 语句的创建与执行、异常处理、事务处理、数据类型转换等操作的封装。我们只要对其传入SQL 语句和必要的参数即可轻松进行 JDBC 编程。



# Hibernate

## 对象/关系映射 (Object Relational Mapping)



对象/关系映射（ORM）是一种将关系数据库中的表中的记录映射成为对象的技术，以对象的形式展现，程序员可以把对数据库的操作转化为对对象的操作。

ORM 的作用是在关系型数据库和业务实体对象之间作一个映射，这样，我们在具体的操作业务对象的时候，就不需要再去和复杂的 SQL 语句打交道，只要像平时操作对象一样操作它就可以了。

ORM 对 JDBC 进行了非常轻量级的对象封装，它将 POJO 与数据库表建立映射关系，是一个全自动的 ORM 框架，Hibernate 可以自动生成 SQL 语句，自动执行，使得 Java 程序员可以随心所欲地使用对象编程思维来操纵数据库。



## 缺点

- 不能指定部分字段
- 无法自定义 SQL，优化困难
- 不支持动态 SQL



# MyBatis

Mybatis是一个基于Java的持久层 / ORM框架：

- 持久（Persistence），即把数据（如内存中的对象）保存到可永久保存的存储设备中（如磁盘）。持久化的主要应用是将内存中的数据存储在关系型的数据库中，当然也可以存储在磁盘文件中、XML数据文件中等，但是一般都会存放在关系型数据库中如：MySQL、Oracle等。
- 持久层（Persistence Layer），即专注于实现数据持久化应用领域的某个特定系统的一个逻辑层面，将数据使用者和数据实体相关联。比如我们的pojo层、Dao层和Service层的关联。

之所以要做持久化和使用ORM设计，因为持久化解决了数据的存储问题，ORM解决的主要是对象关系的映射的问题。

在目前的企业应用系统设计中，都是以MVC为主要的系统架构模式(MVC即 Model（模型）- View（视图）- Control（控制）)。MVC 中的 Model 包含了复杂的业务逻辑和数据逻辑，以及数据存取机制等（如 JDBC的连接、SQL生成和Statement创建、还有ResultSet结果集的读取等）。将这些复杂的业务逻辑和数据逻辑分离，以将系统的紧耦合关系转化为松耦合关系（即解耦合），是降低系统耦合度迫切要做的，也是持久化要做的工作。

MVC 模式实现了架构上将表现层（即View）和数据处理层（即Model）分离的解耦合，持久化的设计则实现了数据处理层内部的业务逻辑和数据逻辑分离的解耦合。

![](.\mybatisStructure.png)

**特性：**

- 使用连接池对连接进行管理
- SQL和代码分离，集中管理
- 参数映射和动态sql
- 结果集映射
- 缓存管理
- 重复SQL的提取
- 插件机制



## Mybatis和Hibernate的区别

**开发方面**

- Hibernate 是全自动 ORM 映射工具，使用 Hibernate 查询关联对象或者关联集合对象时，可以根据对象关系模型直接获取，所以它是全自动的。
- Mybatis 是半自动 ORM 映射工具，Mybatis 在查询关联对象或关联集合对象时，需要手动编写 SQL来完成，所以，称之为半自动 ORM 映射工具。

**底层方面**

- Hibernate 的底层则是 JPA 规范的实现。
- Mybatis 的底层封装了 JDBC 的代码。

**SQL优化方面**

- Hibernate 自动生成 SQL，有些语句较为繁琐，会多消耗一些性能
- Mybatis 手动编写 SQL，可以避免不需要的查询，提高系统性能

**学习成本方面**

- Hibernate 的学习门槛高，要精通门槛更高，而且怎么设计 O/R 映射，在性能和对象模型之间如何权衡，以及怎样用好 Hibernate 需要具有很强的经验和能力才行。
- Mybatis 的学习门槛低，简单易学，程序员只需要把重心放在写原生态 SQL 上即可。

**对象管理方面**

- Hibernate 是完整的对象/关系映射的框架，对象/关系映射能力极强，开发工程中，无需过多关注底层实现，只要去管理对象即可。
- Mybatis 需要自行管理映射关系。

**缓存方面**

- Hibernate 的二级缓存配置在 `SessionFactory` 生成的配置文件中进行详细配置，然后再在具体的表-对象映射中配置是那种缓存。
- 
- MyBatis 的二级缓存配置都是在每个具体的表-对象映射中进行详细配置，这样针对不同的表可以自定义不同的缓存机制。



## MyBatis初始化的方式及引入

MyBatis的初始化可以有两种方式：

- 基于XML配置文件：基于XML配置文件的方式是将MyBatis的所有配置信息放在XML文件中，MyBatis通过加载并XML配置文件，将配置文信息组装成内部的`Configuration`对象。
- 基于Java API：这种方式不使用XML配置文件，需要MyBatis使用者在Java代码中，手动创建`Configuration`对象，然后将配置参数`set` 进入`Configuration`对象中。

### 初始化方式 - XML配置

`````java
// mybatis初始化
String resource = "mybatis-config.xml";  
InputStream inputStream = Resources.getResourceAsStream(resource);  
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

// 创建SqlSession
SqlSession sqlSession = sqlSessionFactory.openSession();  

// 执行SQL语句
List list = sqlSession.selectList("com.foo.bean.BlogMapper.queryAllBlogInfo");
`````

每个基于 MyBatis 的应用都是以一个 `SqlSessionFactory` 的实例为核心的。`SqlSessionFactory` 的实例可以通过 `SqlSessionFactoryBuilder` 获得。而 `SqlSessionFactoryBuilder` 则可以从 XML 配置文件或一个预先配置的 `Configuration` 实例来构建出 `SqlSessionFactory` 实例。



## mybatis-config.xml

XML 配置文件中包含了对 MyBatis 系统的核心设置，包括获取数据库连接实例的数据源（DataSource）以及决定事务作用域和控制方式的事务管理器（TransactionManager）。

### Properties

`````xml
<!-- 方法一： 从外部指定properties配置文件, 除了使用resource属性指定外，还可通过url属性指定url  
        <properties resource="dbConfig.properties"></properties> 
    -->
    <!-- 方法二： 直接配置为xml -->
    <properties>
        <property name="driver" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/test1"/>
        <property name="username" value="root"/>
        <property name="password" value="root"/>
    </properties>
`````

### Envirements

```xml
<environments default="development">
    <environment id="development">
        <!-- 
        JDBC–这个配置直接简单使用了JDBC的提交和回滚设置。它依赖于从数据源得到的连接来管理事务范围。
        MANAGED–这个配置几乎没做什么。它从来不提交或回滚一个连接。而它会让容器来管理事务的整个生命周期（比如Spring或JEE应用服务器的上下文）。
        -->
        <transactionManager type="JDBC"/>
        <!--
        UNPOOLED–这个数据源的实现是每次被请求时简单打开和关闭连接
        POOLED–mybatis实现的简单的数据库连接池类型，它使得数据库连接可被复用，不必在每次请求时都去创建一个物理的连接。
        JNDI – 通过jndi从tomcat之类的容器里获取数据源。
        -->
        <dataSource type="POOLED">
            <!--
            如果上面没有指定数据库配置的properties文件，那么此处可以这样直接配置 
            <property name="driver" value="com.mysql.jdbc.Driver"/>
            <property name="url" value="jdbc:mysql://localhost:3306/test1"/>
            <property name="username" value="root"/>
            <property name="password" value="root"/>
            -->
         
            <!-- 上面指定了数据库配置文件， 配置文件里面也是对应的这四个属性 -->
            <property name="driver" value="${driver}"/>
            <property name="url" value="${url}"/>
            <property name="username" value="${username}"/>
            <property name="password" value="${password}"/>  
        </dataSource>
    </environment>
    
    <!-- 我再指定一个environment -->
    <environment id="test">
        <transactionManager type="JDBC"/>
        <dataSource type="POOLED">
            <property name="driver" value="com.mysql.jdbc.Driver"/>
            <!-- 与上面的url不一样 -->
            <property name="url" value="jdbc:mysql://localhost:3306/demo"/>
            <property name="username" value="root"/>
            <property name="password" value="root"/>
        </dataSource>
    </environment>
</environments>
```

**尽管可以配置多个环境，但每个 SqlSessionFactory 实例只能选择一种环境。**

所以，如果你想连接两个数据库，就需要创建两个 SqlSessionFactory 实例，每个数据库对应一个。

```java
//为了指定创建哪种环境，只要将它作为可选的参数传递给 SqlSessionFactoryBuilder 即可
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, environment);
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, environment, properties);
```

### TypeAliases

类型别名可为 Java 类型设置一个缩写名字。 它仅用于 XML 配置，意在降低冗余的全限定类名书写。

```xml
<typeAliases>
  <typeAlias alias="Blog" type="domain.blog.Blog"/>
</typeAliases>
```

当这样配置时，Blog 可以用在任何使用 domain.blog.Blog 的地方。

### TypeHandlers

MyBatis 在设置预处理语句（PreparedStatement）中的参数或从结果集中取出一个值时， 都会用类型处理器将获取到的值以合适的方式转换成 Java 类型。

你可以重写已有的类型处理器或创建你自己的类型处理器来处理不支持的或非标准的类型。 具体做法为：实现 `org.apache.ibatis.type.TypeHandler` 接口， 或继承一个很便利的类 `org.apache.ibatis.type.BaseTypeHandler`， 并且可以（可选地）将它映射到一个 JDBC 类型。

```java
@MappedJdbcTypes(JdbcType.VARCHAR)
public class ExampleTypeHandler extends BaseTypeHandler<String> {

  @Override
  public void setNonNullParameter(PreparedStatement ps, int i, String parameter, JdbcType jdbcType) throws SQLException {
    ps.setString(i, parameter);
  }

  @Override
  public String getNullableResult(ResultSet rs, String columnName) throws SQLException {
    return rs.getString(columnName);
  }

  @Override
  public String getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
    return rs.getString(columnIndex);
  }

  @Override
  public String getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
    return cs.getString(columnIndex);
  }
}
```

在 mybatis-config.xml 中:

```xml
<typeHandlers>
  <typeHandler handler="org.mybatis.example.ExampleTypeHandler"/>
</typeHandlers>
```

使用上述的类型处理器将会覆盖已有的处理 Java String 类型的属性以及 VARCHAR 类型的参数和结果的类型处理器。 要注意 MyBatis 不会通过检测数据库元信息来决定使用哪种类型，所以你必须在参数和结果映射中指明字段是 VARCHAR 类型， 以使其能够绑定到正确的类型处理器上。这是因为 MyBatis 直到语句被执行时才清楚数据类型。

### ObjectFactory

每次 MyBatis 创建结果对象的新实例时，它都会使用一个对象工厂（ObjectFactory）实例来完成实例化工作。 默认的对象工厂需要做的仅仅是实例化目标类，要么通过默认无参构造方法，要么通过存在的参数映射来调用带有参数的构造方法。 如果想覆盖对象工厂的默认行为，可以通过创建自己的对象工厂来实现。

```java
public class ExampleObjectFactory extends DefaultObjectFactory {
  @Override
  public <T> T create(Class<T> type) {
    return super.create(type);
  }

  @Override
  public <T> T create(Class<T> type, List<Class<?>> constructorArgTypes, List<Object> constructorArgs) {
    return super.create(type, constructorArgTypes, constructorArgs);
  }

  @Override
  public void setProperties(Properties properties) {
    super.setProperties(properties);
  }

  @Override
  public <T> boolean isCollection(Class<T> type) {
    return Collection.class.isAssignableFrom(type);
  }}
```

在mybatis-config.xml中:

```xml
<objectFactory type="org.mybatis.example.ExampleObjectFactory">
  <property name="someProperty" value="100"/>
</objectFactory>
```

### Plugins

plugins 是一个可选配置。mybatis中的plugin其实就是个interceptor， 它可以拦截Executor 、ParameterHandler 、ResultSetHandler 、StatementHandler 的部分方法，处理我们自己的逻辑。

Executor就是真正执行sql语句的东西， ParameterHandler 是处理我们传入参数的，还记得前面讲TypeHandler的时候提到过，mybatis默认帮我们实现了不少的typeHandler, 当我们不显示配置typeHandler的时候，mybatis会根据参数类型自动选择合适的typeHandler执行，其实就是ParameterHandler 在选择。ResultSetHandler 就是处理返回结果的。

### Mappers

既然 MyBatis 的行为已经由上述元素配置完了，我们现在就要来定义 SQL 映射语句了。 但首先，我们需要告诉 MyBatis 到哪里去找到这些语句。 在自动查找资源方面，Java 并没有提供一个很好的解决方案，所以最好的办法是直接告诉 MyBatis 到哪里去找映射文件。

```xml
<configuration>
    ......
    <mappers>
        <!-- 第一种方式：通过resource指定 -->
        <mapper resource="com/dy/dao/userDao.xml"/>
    
        <!-- 第二种方式， 通过class指定接口，进而将接口与对应的xml文件形成映射关系
             不过，使用这种方式必须保证 接口与mapper文件同名(不区分大小写)， 
             我这儿接口是UserDao,那么意味着mapper文件为UserDao.xml 
        <mapper class="com.dy.dao.UserDao"/>
        -->
      
        <!-- 第三种方式，直接指定包，自动扫描，与方法二同理 
        <package name="com.dy.dao"/>
        -->
        <!-- 第四种方式：通过url指定mapper文件位置
        <mapper url="file://........"/>
        -->
    </mappers>
    ......
</configuration>
```

### Settings

**全局配置**

- `cacheEnabled`: 全局性地开启或关闭所有映射器配置文件中已配置的任何缓存。
- `lazyLoadingEnabled`: 延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。
- `defaultExecutorType`: 配置默认的执行器。
  - `SIMPLE`: 普通的执行器
  - `REUSE`: 重用预处理语句（PreparedStatement）
  - `BATCH`: 重用语句并执行批量更新
- `logImpl`: 指定 MyBatis 所用日志的具体实现，未指定时将自动查找。

**本地缓存**

- `localCacheScope`: MyBatis 利用本地缓存机制（Local Cache）防止循环引用和加速重复的嵌套查询。
  - `SESSION`: 缓存一个会话中执行的所有查询
  - `STATEMENT`: 本地缓存仅用于执行语句，对相同 SqlSession 的不同查询将不会进行缓存

**延迟加载**

- `aggressiveLazyLoading`: 开启时，任一方法的调用都会加载该对象的所有延迟加载属性。 否则，每个延迟加载属性会按需加载
- `lazyLoadTriggerMethods`: 指定对象的哪些方法触发一次延迟加载。



## 作用域（Scope）和生命周期(Lifecycle)

依赖注入框架(Spring)可以创建线程安全的、基于事务的 `SqlSession` 和映射器，并将它们直接注入到你的 bean 中，因此可以直接忽略它们的生命周期。否则:

#### **SqlSessionFactoryBuilder**

这个类可以被实例化、使用和丢弃，一旦创建了 `SqlSessionFactory`，就不再需要它了。 因此 `SqlSessionFactoryBuilder` 实例的最佳作用域是方法作用域（也就是局部方法变量）。 你可以重用 `SqlSessionFactoryBuilder` 来创建多个 `SqlSessionFactory` 实例，但最好还是不要一直保留着它，以保证所有的 XML 解析资源可以被释放给更重要的事情。

#### **SqlSessionFactory**

`SqlSessionFactory` 一旦被创建就应该在应用的运行期间一直存在，没有任何理由丢弃它或重新创建另一个实例。 使用 `SqlSessionFactory` 的最佳实践是在应用运行期间不要重复创建多次，多次重建 `SqlSessionFactory` 被视为一种代码“坏习惯”。因此 `SqlSessionFactory` 的最佳作用域是应用作用域。 有很多方法可以做到，最简单的就是使用单例模式或者静态单例模式。

#### **SqlSession**

每个线程都应该有它自己的 `SqlSession` 实例。`SqlSession` 的实例不是线程安全的，因此是不能被共享的，所以它的最佳的作用域是请求或方法作用域。 绝对不能将 `SqlSession` 实例的引用放在一个类的静态域，甚至一个类的实例变量也不行。 也绝不能将 SqlSession 实例的引用放在任何类型的托管作用域中，比如 Servlet 框架中的 `HttpSession`。 如果你现在正在使用一种 Web 框架，考虑将 `SqlSession` 放在一个和 HTTP 请求相似的作用域中。 换句话说，每次收到 HTTP 请求，就可以打开一个 `SqlSession`，返回一个响应后，就关闭它。 这个关闭操作很重要，为了确保每次都能执行关闭操作，你应该把这个关闭操作放到 finally 块中。

#### **映射器实例**

映射器是一些绑定映射语句的接口。映射器接口的实例是从 SqlSession 中获得的。虽然从技术层面上来讲，任何映射器实例的最大作用域与请求它们的 SqlSession 相同。但方法作用域才是映射器实例的最合适的作用域。 也就是说，映射器实例应该在调用它们的方法中被获取，使用完毕之后即可丢弃。 映射器实例并不需要被显式地关闭。



## Mapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.mybatis.example.BlogMapper">
  <select id="selectBlog" resultType="Blog">
    select * from Blog where id = #{id}
  </select>
</mapper>
```

使用：

```java
Blog blog = (Blog) session.selectOne("org.mybatis.example.BlogMapper.selectBlog", 101);
```

或：

```java
BlogMapper mapper = session.getMapper(BlogMapper.class);
Blog blog = mapper.selectBlog(101);
```

### insert, update, delete 的配置及使用

```xml
<?xml version="1.0" encoding="UTF-8" ?>   
<!DOCTYPE mapper   
PUBLIC "-//ibatis.apache.org//DTD Mapper 3.0//EN"  
"http://ibatis.apache.org/dtd/ibatis-3-mapper.dtd"> 

<!-- mapper 为根元素节点， 一个namespace对应一个dao -->
<!-- 
Mapper元素只有一个属性namespace，它有两个作用：`一是用于区分不同的mapper`（在不同的mapper文件里，子元素的id可以相同，mybatis通过namespace和子元素的id联合区分），`二是与接口关联`（应用程序通过接口访问mybatis时，mybatis通过接口的完整名称查找对应的mapper配置，因此namespace的命名务必小心一定要某接口同名）。
-->
<mapper namespace="com.dy.dao.UserDao">
    
    <!-- 
    cache- 配置本定命名空间的缓存。
        type- cache实现类，默认为PERPETUAL，可以使用自定义的cache实现类（别名或完整类名皆可）
        eviction- 回收算法，默认为LRU，可选的算法有：
            LRU– 最近最少使用的：移除最长时间不被使用的对象。
            FIFO– 先进先出：按对象进入缓存的顺序来移除它们。
            SOFT– 软引用：移除基于垃圾回收器状态和软引用规则的对象。
            WEAK– 弱引用：更积极地移除基于垃圾收集器状态和弱引用规则的对象。
        flushInterval- 刷新间隔，默认为1个小时，单位毫秒
        size- 缓存大小，默认大小1024，单位为引用数
        readOnly- 只读
    -->
    <cache type="PERPETUAL" eviction="LRU" flushInterval="60000"  
        size="512" readOnly="true" />
    
    <!-- 
    cache-ref–从其他命名空间引用缓存配置。

    如果你不想定义自己的cache，可以使用cache-ref引用别的cache。因为每个cache都以namespace为id，所以cache-ref只需要配置一个namespace属性就可以了。需要注意的是，如果cache-ref和cache都配置了，以cache为准。
    -->
    <cache-ref namespace="com.someone.application.data.SomeMapper"/>
    
    <insert
      <!-- 1. id （必须配置）
        id是命名空间中的唯一标识符，可被用来代表这条语句。 
        一个命名空间（namespace） 对应一个dao接口, 
        这个id也应该对应dao里面的某个方法（相当于方法的实现），因此id 应该与方法名一致 -->
      
      id="insertUser"
      
      <!-- 2. parameterType （可选配置, 默认为mybatis自动选择处理）
        将要传入语句的参数的完全限定类名或别名， 如果不配置，mybatis会通过ParameterHandler 根据参数类型默认选择合适的typeHandler进行处理
        parameterType 主要指定参数类型，可以是int, short, long, string等类型，也可以是复杂类型（如对象） -->
      
      parameterType="com.demo.User"
      
      <!-- 3. flushCache （可选配置，默认配置为true）
        将其设置为 true，任何时候只要语句被调用，都会导致本地缓存和二级缓存都会被清空，默认值：true（对应插入、更新和删除语句） -->
      
      flushCache="true"
      
      <!-- 4. statementType （可选配置，默认配置为PREPARED）
        STATEMENT，PREPARED 或 CALLABLE 的一个。这会让 MyBatis 分别使用 Statement，PreparedStatement 或 CallableStatement，默认值：PREPARED。 -->
      
      statementType="PREPARED"
      
      <!-- 5. keyProperty (可选配置， 默认为unset)
       （仅对 insert 和 update 有用）唯一标记一个属性，MyBatis 会通过 getGeneratedKeys 的返回值或者通过 insert 语句的 selectKey 子元素设置它的键值，默认：unset。如果希望得到多个生成的列，也可以是逗号分隔的属性名称列表。 -->
      
      keyProperty=""
      
      <!-- 6. keyColumn  (可选配置)
        （仅对 insert 和 update 有用）通过生成的键值设置表中的列名，这个设置仅在某些数据库（像PostgreSQL）是必须的，当主键列不是表中的第一列的时候需要设置。如果希望得到多个生成的列，也可以是逗号分隔的属性名称列表。 -->
      
      keyColumn=""
      
      <!-- 7. useGeneratedKeys (可选配置， 默认为false)
        （仅对 insert 和 update 有用）这会令 MyBatis 使用 JDBC 的 getGeneratedKeys 方法来取出由数据库内部生成的主键（比如：像 MySQL 和 SQL Server 这样的关系数据库管理系统的自动递增字段），默认值：false。  -->
      
      useGeneratedKeys="false"
      
      <!-- 8. timeout  (可选配置，默认为unset, 依赖驱动)
        这个设置是在抛出异常之前，驱动程序等待数据库返回请求结果的秒数。默认值为 unset（依赖驱动）。 -->
      timeout="20">

    <!-- update 和 delete 同理 -->
    <update
      id="updateUser"
      parameterType="com.demo.User"
      flushCache="true"
      statementType="PREPARED"
      timeout="20">

    <delete
      id="deleteUser"
      parameterType="com.demo.User"
      flushCache="true"
      statementType="PREPARED"
      timeout="20">
</mapper>
```

### selectKey

`selectKey`在Mybatis中是为了解决Insert数据时不支持主键自动生成的问题，他可以很随意的设置生成主键的方式。 使用mybatis的`selectKey`就可以得到sequence的值，同时也会将值返回。另外，`selectKey`节点生成的`KeyGenerator`优先级高于statement节点的`useGeneratedKeys`属性生成的`KeyGenerator`对象，也就是说配置了`SelectKey`子节点就不需要再配置`useGeneratedKeys`属性了。

```xml
<selectKey
        <!-- selectKey 语句结果应该被设置的目标属性。如果希望得到多个生成的列，也可以是逗号分隔的属性名称列表。 -->
        keyProperty="id"
        <!-- 结果的类型。MyBatis 通常可以推算出来，但是为了更加确定写上也不会有什么问题。MyBatis 允许任何简单类型用作主键的类型，包括字符串。如果希望作用于多个生成的列，则可以使用一个包含期望属性的 Object 或一个 Map。 -->
        resultType="int"
        <!-- 这可以被设置为 BEFORE 或 AFTER。如果设置为 BEFORE，那么它会首先选择主键，设置 keyProperty 然后执行插入语句。如果设置为 AFTER，那么先执行插入语句，然后是 selectKey 元素 - 这和像 Oracle 的数据库相似，在插入语句内部可能有嵌入索引调用。 -->
        order="BEFORE"
        <!-- 与前面相同，MyBatis 支持 STATEMENT，PREPARED 和 CALLABLE 语句的映射类型，分别代表 PreparedStatement 和 CallableStatement 类型。 -->
        statementType="PREPARED">
```



### 简单结果映射(select)

之前你已经见过简单映射语句的示例，它们没有显式指定 `resultMap`。比如：

```xml
<select id="selectUsers" resultType="map">
  select id, username, hashedPassword
  from some_table
  where id = #{id}
</select>
```

#### select 的配置：

```xml
<select
     <!--  1. id （必须配置）
        id是命名空间中的唯一标识符，可被用来代表这条语句。 
        一个命名空间（namespace） 对应一个dao接口, 
        这个id也应该对应dao里面的某个方法（相当于方法的实现），因此id 应该与方法名一致
     -->
     
     id="selectPerson"
     
     <!-- 2. parameterType （可选配置, 默认为mybatis自动选择处理）
        将要传入语句的参数的完全限定类名或别名， 如果不配置，mybatis会通过ParameterHandler 根据参数类型默认选择合适的typeHandler进行处理
        parameterType 主要指定参数类型，可以是int, short, long, string等类型，也可以是复杂类型（如对象） -->
     parameterType="int"
     
     <!-- 3. resultType (resultType 与 resultMap 二选一配置)
         resultType用以指定返回类型，指定的类型可以是基本类型，可以是java容器，也可以是javabean -->
     resultType="hashmap"
     
     <!-- 4. resultMap (resultType 与 resultMap 二选一配置)
         resultMap用于引用我们通过 resultMap标签定义的映射类型，这也是mybatis组件高级复杂映射的关键 -->
     resultMap="personResultMap"
     
     <!-- 5. flushCache (可选配置)
         将其设置为 true，任何时候只要语句被调用，都会导致本地缓存和二级缓存都会被清空，默认值：false -->
     flushCache="false"
     
     <!-- 6. useCache (可选配置)
         将其设置为 true，将会导致本条语句的结果被二级缓存，默认值：对 select 元素为 true -->
     useCache="true"
     
     <!-- 7. timeout (可选配置) 
         这个设置是在抛出异常之前，驱动程序等待数据库返回请求结果的秒数。默认值为 unset（依赖驱动）-->
     timeout="10000"
     
     <!-- 8. fetchSize (可选配置) 
         这是尝试影响驱动程序每次批量返回的结果行数和这个设置值相等。默认值为 unset（依赖驱动)-->
     fetchSize="256"
     
     <!-- 9. statementType (可选配置) 
         STATEMENT，PREPARED 或 CALLABLE 的一个。这会让 MyBatis 分别使用 Statement，PreparedStatement 或 CallableStatement，默认值：PREPARED-->
     statementType="PREPARED"
     
     <!-- 10. resultSetType (可选配置) 
         FORWARD_ONLY，SCROLL_SENSITIVE 或 SCROLL_INSENSITIVE 中的一个，默认值为 unset （依赖驱动）-->
     resultSetType="FORWARD_ONLY">
```

虽然上面的例子不用显式配置 `ResultMap`。 但为了讲解，我们来看看如果在刚刚的示例中，显式使用外部的 `resultMap` 会怎样，这也是解决列名不匹配的另外一种方式。

```xml
<resultMap id="userResultMap" type="User">
  <id property="id" column="user_id" />
  <result property="username" column="user_name"/>
  <result property="password" column="hashed_password"/>
</resultMap>
```

然后在引用它的语句中设置 `resultMap` 属性就行了（注意我们去掉了 `resultType` 属性）。比如:

```xml
<select id="selectUsers" resultMap="userResultMap">
  select user_id, user_name, hashed_password
  from some_table
  where id = #{id}
</select>
```

### 高级结果映射(resultMap)

MyBatis 创建时的一个思想是：数据库不可能永远是你所想或所需的那个样子。 我们希望每个数据库都具备良好的第三范式或 BCNF 范式，可惜它们并不都是那样。 如果能有一种数据库映射模式，完美适配所有的应用程序，那就太好了，但可惜也没有。 而 ResultMap 就是 MyBatis 对这个问题的答案。

比如，我们如何映射下面这个语句？

```xml
<!-- 非常复杂的语句 -->
<select id="selectBlogDetails" resultMap="detailedBlogResultMap">
  select
       B.id as blog_id,
       B.title as blog_title,
       B.author_id as blog_author_id,
       A.id as author_id,
       A.username as author_username,
       A.password as author_password,
       A.email as author_email,
       A.bio as author_bio,
       A.favourite_section as author_favourite_section,
       P.id as post_id,
       P.blog_id as post_blog_id,
       P.author_id as post_author_id,
       P.created_on as post_created_on,
       P.section as post_section,
       P.subject as post_subject,
       P.draft as draft,
       P.body as post_body,
       C.id as comment_id,
       C.post_id as comment_post_id,
       C.name as comment_name,
       C.comment as comment_text,
       T.id as tag_id,
       T.name as tag_name
  from Blog B
       left outer join Author A on B.author_id = A.id
       left outer join Post P on B.id = P.blog_id
       left outer join Comment C on P.id = C.post_id
       left outer join Post_Tag PT on PT.post_id = P.id
       left outer join Tag T on PT.tag_id = T.id
  where B.id = #{id}
</select>
```

```xml
<!-- 非常复杂的结果映射 -->
<resultMap id="detailedBlogResultMap" type="Blog">
  <constructor>
    <idArg column="blog_id" javaType="int"/>
  </constructor>
  <result property="title" column="blog_title"/>
  <association property="author" javaType="Author">
    <id property="id" column="author_id"/>
    <result property="username" column="author_username"/>
    <result property="password" column="author_password"/>
    <result property="email" column="author_email"/>
    <result property="bio" column="author_bio"/>
    <result property="favouriteSection" column="author_favourite_section"/>
  </association>
  <collection property="posts" ofType="Post">
    <id property="id" column="post_id"/>
    <result property="subject" column="post_subject"/>
    <association property="author" javaType="Author"/>
    <collection property="comments" ofType="Comment">
      <id property="id" column="comment_id"/>
    </collection>
    <collection property="tags" ofType="Tag" >
      <id property="id" column="tag_id"/>
    </collection>
    <discriminator javaType="int" column="draft">
      <case value="1" resultType="DraftPost"/>
    </discriminator>
  </collection>
</resultMap>
```

####  resultMap 的配置：

```xml
<!-- 
  resultMap –结果映射，用来描述如何从数据库结果集映射到你想要的对象。

  1.type 对应类型，可以是javabean, 也可以是其它
  2.id 必须唯一， 用于标示这个resultMap的唯一性，在使用resultMap的时候，就是通过id指定
 -->
<resultMap type="" id="">

    <!-- id, 唯一性，注意啦，这个id用于标示这个javabean对象的唯一性， 不一定会是数据库的主键（不要把它理解为数据库对应表的主键） 
        property属性对应javabean的属性名，column对应数据库表的列名
       （这样，当javabean的属性与数据库对应表的列名不一致的时候，就能通过指定这个保持正常映射了）
     -->
    <id property="" column=""/>

    <!-- result与id相比， 对应普通属性 -->    
    <result property="" column=""/>

    <!-- 
       constructor对应javabean中的构造方法
     -->
    <constructor>
        <!-- idArg 对应构造方法中的id参数；-->
        <idArg column=""/>
        <!-- arg 对应构造方法中的普通参数；-->
        <arg column=""/>
    </constructor>

    <!-- 
     聚集元素用来处理“一对多”的关系。需要指定映射的Java实体类的属性，属性的javaType（一般为ArrayList）；列表中对象的类型ofType（Java实体类）；对应的数据库表的列名称；

     	collection，对应javabean中容器类型, 是实现一对多的关键 
     	property 为javabean中容器对应字段名
     	column 为体现在数据库中列名
     	ofType 就是指定javabean中容器指定的类型

     不同情况需要告诉MyBatis 如何加载一个聚集。MyBatis 可以用两种方式加载：
           1. select: 执行一个其它映射的SQL 语句返回一个Java实体类型。较灵活；
           2. resultMap: 使用一个嵌套的结果映射来处理通过join查询结果集，映射成Java实体类型。
     -->
    <collection property="" column="" ofType=""></collection>

    <!-- 
   		联合元素用来处理“一对一”的关系。需要指定映射的Java实体类的属性，属性的javaType（通常MyBatis 自己会识别）。对应的数据库表的列名称。如果想覆写的话返回结果的值，需要指定typeHandler。

            association 为关联关系，是实现N对一的关键。
            property 为javabean中容器对应字段名
            column 为体现在数据库中列名
            javaType 指定关联的类型

      	不同情况需要告诉MyBatis 如何加载一个联合。MyBatis可以用两种方式加载：
             1. select: 执行一个其它映射的SQL 语句返回一个Java实体类型。较灵活；
             2. resultMap: 使用一个嵌套的结果映射来处理，通过join查询结果集，映射成Java实体类型。
     -->
    <association property="" column="" javaType=""></association>

    <!-- 
       有时一个单独的数据库查询也许返回很多不同（但是希望有些关联）数据类型的结果集。鉴别器元素就是被设计来处理这个情况的，还有包括类的继承层次结构。鉴别器非常容易理解，因为它的表现很像Java语言中的switch语句。

       定义鉴别器指定了column和javaType属性。列是MyBatis查找比较值的地方。JavaType是需要被用来保证等价测试的合适类型（尽管字符串在很多情形下都会有用）。

        下面这个例子为，当classId为20000001时，才映射classId属性。
   -->
    <discriminator column="CLASS_ID" javaType="String" jdbcType="VARCHAR">  
        <case value="20000001" resultType="liming.student.manager.data.model.StudentEntity" >  
            <result property="classId" column="CLASS_ID" javaType="String" jdbcType="VARCHAR"/>  
        </case> 
    </discriminator>
</resultMap>
```



### 关联(Association)

```xml
<association property="author" column="blog_author_id" javaType="Author">
  <id property="id" column="author_id"/>
  <result property="username" column="author_username"/>
</association>
```

关联（association）元素处理“has one”类型的关系。 比如，在我们的示例中，一个博客有一个(has one)用户。关联结果映射和其它类型的映射工作方式差不多。 你需要指定目标属性名以及属性的`javaType`（很多时候 MyBatis 可以自己推断出来），在必要的情况下你还可以设置 JDBC 类型，如果你想覆盖获取结果值的过程，还可以设置类型处理器。

关联的不同之处是，你需要告诉 MyBatis 如何加载关联。MyBatis 有两种不同的方式加载关联：

- 嵌套 Select 查询：通过执行另外一个 SQL 映射语句来加载期望的复杂类型。
- 嵌套结果映射：使用嵌套的结果映射来处理连接结果的重复子集。

首先，先让我们来看看这个元素的属性。你将会发现，和普通的结果映射相比，它只在 select 和 resultMap 属性上有所不同。

| 属性          | 描述                                                         |
| ------------- | ------------------------------------------------------------ |
| `property`    | 映射到列结果的字段或属性。如果用来匹配的 JavaBean 存在给定名字的属性，那么它将会被使用。否则 MyBatis 将会寻找给定名称的字段。 无论是哪一种情形，你都可以使用通常的点式分隔形式进行复杂属性导航。 比如，你可以这样映射一些简单的东西：“username”，或者映射到一些复杂的东西上：“address.street.number”。 |
| `javaType`    | 一个 Java 类的完全限定名，或一个类型别名（关于内置的类型别名，可以参考上面的表格）。 如果你映射到一个 JavaBean，MyBatis 通常可以推断类型。然而，如果你映射到的是 HashMap，那么你应该明确地指定 javaType 来保证行为与期望的相一致。 |
| `jdbcType`    | JDBC 类型，所支持的 JDBC 类型参见这个表格之前的“支持的 JDBC 类型”。 只需要在可能执行插入、更新和删除的且允许空值的列上指定 JDBC 类型。这是 JDBC 的要求而非 MyBatis 的要求。如果你直接面向 JDBC 编程，你需要对可能存在空值的列指定这个类型。 |
| `typeHandler` | 我们在前面讨论过默认的类型处理器。使用这个属性，你可以覆盖默认的类型处理器。 这个属性值是一个类型处理器实现类的完全限定名，或者是类型别名。 |

#### 关联的嵌套 Select 查询

| 属性        | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| `column`    | 数据库中的列名，或者是列的别名。一般情况下，这和传递给 `resultSet.getString(columnName)` 方法的参数一样。 注意：在使用复合主键的时候，你可以使用 `column="{prop1=col1,prop2=col2}"` 这样的语法来指定多个传递给嵌套 Select 查询语句的列名。这会使得 `prop1` 和 `prop2` 作为参数对象，被设置为对应嵌套 Select 语句的参数。 |
| `select`    | 用于加载复杂类型属性的映射语句的 ID，它会从 column 属性指定的列中检索数据，作为参数传递给目标 select 语句。 具体请参考下面的例子。注意：在使用复合主键的时候，你可以使用 `column="{prop1=col1,prop2=col2}"` 这样的语法来指定多个传递给嵌套 Select 查询语句的列名。这会使得 `prop1` 和 `prop2` 作为参数对象，被设置为对应嵌套 Select 语句的参数。 |
| `fetchType` | 可选的。有效值为 `lazy` 和 `eager`。 指定属性后，将在映射中忽略全局配置参数 `lazyLoadingEnabled`，使用属性的值。 |

示例：

```java
<resultMap id="blogResult" type="Blog">
  <association property="author" column="author_id" javaType="Author" select="selectAuthor"/>
</resultMap>

<select id="selectBlog" resultMap="blogResult">
  SELECT * FROM BLOG WHERE ID = #{id}
</select>

<select id="selectAuthor" resultType="Author">
  SELECT * FROM AUTHOR WHERE ID = #{id}
</select>
```

就是这么简单。我们有两个 select 查询语句：一个用来加载博客（Blog），另外一个用来加载作者（Author），而且博客的结果映射描述了应该使用 `selectAuthor` 语句加载它的 author 属性。

其它所有的属性将会被自动加载，只要它们的列名和属性名相匹配。

这种方式虽然很简单，但在大型数据集或大型数据表上表现不佳。这个问题被称为“N+1 查询问题”。 概括地讲，N+1 查询问题是这样子的：

- 你执行了一个单独的 SQL 语句来获取结果的一个列表（就是“+1”）。
- 对列表返回的每条记录，你执行一个 select 查询语句来为每条记录加载详细信息（就是“N”）。

这个问题会导致成百上千的 SQL 语句被执行。有时候，我们不希望产生这样的后果。

好消息是，MyBatis 能够对这样的查询进行延迟加载，因此可以将大量语句同时运行的开销分散开来。 然而，如果你加载记录列表之后立刻就遍历列表以获取嵌套的数据，就会触发所有的延迟加载查询，性能可能会变得很糟糕。

所以还有另外一种方法。

#### 关联的嵌套结果映射

| 属性            | 描述                                                         |
| --------------- | ------------------------------------------------------------ |
| `resultMap`     | 结果映射的 ID，可以将此关联的嵌套结果集映射到一个合适的对象树中。 它可以作为使用额外 select 语句的替代方案。它可以将多表连接操作的结果映射成一个单一的 `ResultSet`。这样的 `ResultSet` 有部分数据是重复的。 为了将结果集正确地映射到嵌套的对象树中, MyBatis 允许你“串联”结果映射，以便解决嵌套结果集的问题。使用嵌套结果映射的一个例子在表格以后。 |
| `columnPrefix`  | 当连接多个表时，你可能会不得不使用列别名来避免在 `ResultSet` 中产生重复的列名。指定 columnPrefix 列名前缀允许你将带有这些前缀的列映射到一个外部的结果映射中。 详细说明请参考后面的例子。 |
| `notNullColumn` | 默认情况下，在至少一个被映射到属性的列不为空时，子对象才会被创建。 你可以在这个属性上指定非空的列来改变默认行为，指定后，Mybatis 将只在这些列中任意一列非空时才创建一个子对象。可以使用逗号分隔来指定多个列。默认值：未设置（unset）。 |
| `autoMapping`   | 如果设置这个属性，MyBatis 将会为本结果映射开启或者关闭自动映射。 这个属性会覆盖全局的属性 autoMappingBehavior。注意，本属性对外部的结果映射无效，所以不能搭配 `select` 或 `resultMap` 元素使用。默认值：未设置（unset）。 |

现在我们将博客表和作者表连接在一起，而不是执行一个独立的查询语句，就像这样：

```xml
<select id="selectBlog" resultMap="blogResult">
  select
    B.id            as blog_id,
    B.title         as blog_title,
    B.author_id     as blog_author_id,
    A.id            as author_id,
    A.username      as author_username,
    A.password      as author_password,
    A.email         as author_email,
    A.bio           as author_bio
  from Blog B left outer join Author A on B.author_id = A.id
  where B.id = #{id}
</select>
```

注意查询中的连接，以及为确保结果能够拥有唯一且清晰的名字，我们设置的别名。 这使得进行映射非常简单。现在我们可以映射这个结果：

```xml
<resultMap id="blogResult" type="Blog">
  <id property="id" column="blog_id" />
  <result property="title" column="blog_title"/>
  <association property="author" column="blog_author_id" javaType="Author" resultMap="authorResult"/>
</resultMap>

<resultMap id="authorResult" type="Author">
  <id property="id" column="author_id"/>
  <result property="username" column="author_username"/>
  <result property="password" column="author_password"/>
  <result property="email" column="author_email"/>
  <result property="bio" column="author_bio"/>
</resultMap>

 <!-- 或者可以直接将结果映射作为子元素嵌套在内 -->
<resultMap id="blogResult" type="Blog">
  <id property="id" column="blog_id" />
  <result property="title" column="blog_title"/>
  <association property="author" javaType="Author">
    <id property="id" column="author_id"/>
    <result property="username" column="author_username"/>
    <result property="password" column="author_password"/>
    <result property="email" column="author_email"/>
    <result property="bio" column="author_bio"/>
  </association>
</resultMap>
```

在上面的例子中，你可以看到，博客（Blog）作者（author）的关联元素委托名为 “authorResult” 的结果映射来加载作者对象的实例。



## MyBatis缓存详解

> 每当我们使用MyBatis开启一次和数据库的会话，MyBatis会创建出一个SqlSession对象表示一次数据库会话。
>
> 在对数据库的一次会话中，我们有可能会反复地执行完全相同的查询语句，如果不采取一些措施的话，每一次查询都会查询一次数据库,而我们在极短的时间内做了完全相同的查询，那么它们的结果极有可能完全相同，由于查询一次数据库的代价很大，这有可能造成很大的资源浪费。
>
> 为了解决这一问题，减少资源的浪费，MyBatis会在表示会话的SqlSession对象中建立一个简单的缓存，将每次查询到的结果结果缓存起来，当下次查询的时候，如果判断先前有个完全一样的查询，会直接从缓存中直接将结果取出，返回给用户，不需要再进行一次数据库查询了。

|     缓存实现类     |       描述       |                             作用                             |                     装饰条件                     |
| :----------------: | :--------------: | :----------------------------------------------------------: | :----------------------------------------------: |
|      基本缓存      |  缓存基本实现类  | 默认是PerpetualCache，也可以自定义比如RedisCache、EhCache等，具备基本功能的缓存类 |                        无                        |
|      LruCache      |  LRU策略的缓存   |          当缓存到达上限时候，删除最近最少使用的缓存          |              eviction="LRU"（默认）              |
|     FifoCache      |  FIFO策略的缓存  |            当缓存到达上限时候，删除最先入队的缓存            |                 eviction="FIFO"                  |
|     SoftCache      | 带清理策略的缓存 | 通过JVM的软引用和弱引用来实现缓存，当JVM内存不足时，会自动清理掉这些缓存 |                 eviction="SOFT"                  |
|     WeakCache      | 带清理策略的缓存 | 通过JVM的软引用和弱引用来实现缓存，当JVM内存不足时，会自动清理掉这些缓存 |                 eviction="WEAK"                  |
|    LoggingCache    | 带日志功能的缓存 |                     比如：输出缓存命中率                     |                        无                        |
| SynchronizedCache  |     同步缓存     |           基于synchronized关键字实现，解决并发问题           |                        无                        |
|   BlockingCache    |     阻塞缓存     | 通过在get/put方式中加锁，保证只有一个线程操作缓存，基于Java重入锁实现 |                  blocking=true                   |
|  SerializedCache   | 支持序列化的缓存 |          将对象序列化以后存到缓存中，取出时反序列化          |              readOnly=false（默认）              |
|   ScheduledCache   |  定时调度的缓存  | 在进行get/put/remove/getSize等操作前，判断缓存时间是否超过了设置的最长缓存时间（默认是一小时），如果是则清空缓存--即每隔一段时间清空一次缓存 |   <mapper>中<cache>标签的`flushInterval`不为空   |
| TransactionalCache |     事务缓存     |      在二级缓存中使用，可一次存入多个缓存，移除多个缓存      | 在`TransactionalCacheManager`中用Map维护对应关系 |

### MyBatis一级缓存

一级缓存是SqlSession级别的缓存。在操作数据库时需要构造 sqlSession对象，在对象中有一个(内存区域)数据结构（HashMap）用于存储缓存数据。不同的sqlSession之间的缓存数据区域（HashMap）是互相不影响的。

Session级别的一级缓存实际上就是使用`PerpetualCache`维护的，是默认开启的。

`PerpetualCache`实现原理其实很简单，其内部就是通过一个简单的`HashMap<k,v>` 来实现的，没有其他的任何限制。

#### 一级缓存的生命周期有多长

MyBatis在开启一个数据库会话时，会创建一个新的`SqlSession`对象，当创建了一个`SqlSession`对象时，MyBatis会为这个`SqlSession`对象创建一个新的`Executor`执行器，而缓存信息就被维护在这个`Executor`执行器中，MyBatis将缓存和对缓存相关的操作封装成了`Cache`接口中。当会话结束时，`SqlSession`对象及其内部的`Executor`对象还有`PerpetualCache`对象也一并释放掉。

- 如果`SqlSession`调用了`close()`方法，会释放掉一级缓存`PerpetualCache`对象，一级缓存将不可用；
- 如果`SqlSession`调用了`clearCache()`，会清空`PerpetualCache`对象中的数据，但是该对象仍可使用；
- `SqlSession`中执行了任何一个update操作(`update()`、`delete()`、`insert()`) ，都会清空`PerpetualCache`对象的数据，但是该对象可以继续使用；

#### 一级缓存的工作流程

![](.\mybatisl1Cache.png)

#### CacheKey的定义

MyBatis认为，对于两次查询，如果以下条件都完全一样，那么就认为它们是完全相同的两次查询：

- **传入的 statementId**

传入的statementId，对于MyBatis而言，你要使用它，必须需要一个statementId，它代表着你将执行什么样的Sql；

- **查询时要求的结果集中的结果范围 （结果的范围通过`rowBounds.offset`和`rowBounds.limit`表示）**

MyBatis自身提供的分页功能是通过RowBounds来实现的，它通过rowBounds.offset和rowBounds.limit来过滤查询出来的结果集，这种分页功能是基于查询结果的再过滤，而不是进行数据库的物理分页；

- **这次查询所产生的最终要传递给JDBC `java.sql.Preparedstatement`的Sql语句字符串（`boundSql.getSql()`）**

由于MyBatis底层还是依赖于JDBC实现的，那么，对于两次完全一模一样的查询，MyBatis要保证对于底层JDBC而言，也是完全一致的查询才行。而对于JDBC而言，两次查询，只要传入给JDBC的SQL语句完全一致，传入的参数也完全一致，就认为是两次查询是完全一致的。

- **传递给`java.sql.Statement`要设置的参数值**

上述的第3个条件正是要求保证传递给JDBC的SQL语句完全一致；第4条则是保证传递给JDBC的参数也完全一致；即3、4两条MyBatis最本质的要求就是：调用JDBC的时候，传入的SQL语句要完全相同，传递给JDBC的参数值也要完全相同。

### 二级缓存

二级缓存是application级别的缓存，多个SqlSession去操作同一个Mapper的sql语句，多个SqlSession去操作数据库得到数据会存在二级缓存区域，多个SqlSession可以共用二级缓存，二级缓存是跨SqlSession的。

二级缓存是多个SqlSession共享的，其作用域是mapper的同一个namespace，不同的sqlSession两次执行相同namespace下的sql语句且向sql中传递参数也相同即最终执行相同的sql语句，第一次执行完毕会将数据库中查询的数据写到缓存（内存），第二次会从缓存中获取数据将不再从数据库查询，从而提高查询效率。Mybatis默认**没有开启二级缓存**，需要在setting全局参数中配置开启二级缓存。

当开一个会话时，一个SqlSession对象会使用一个Executor对象来完成会话操作，MyBatis的二级缓存机制的关键就是对这个Executor对象做文章。如果用户配置了"cacheEnabled=true"，那么MyBatis在为SqlSession对象创建Executor对象时，会对Executor对象加上一个装饰者：CachingExecutor，这时SqlSession使用CachingExecutor对象来完成操作请求。CachingExecutor对于查询请求，会先判断该查询请求在Application级别的二级缓存中是否有缓存结果，如果有查询结果，则直接返回缓存结果；如果缓存中没有，再交给真正的Executor对象来完成查询操作，之后CachingExecutor会将真正Executor返回的查询结果放置到缓存中，然后在返回给用户。

![](.\CachingExecutor.png)

### MyBatis二级缓存的划分

MyBatis并不是简单地对整个Application就只有一个Cache缓存对象，它将缓存划分的更细，即是Mapper级别的，即每一个Mapper都可以拥有一个Cache对象

**为每一个Mapper分配一个Cache缓存对象**

MyBatis将Application级别的二级缓存细分到Mapper级别，即对于每一个Mapper.xml,如果在其中使用了`<cache>` 节点，则MyBatis会为这个Mapper创建一个Cache缓存对象，如下图所示：

![](.\SecondaryCache.png)

上述的每一个Cache对象，都会有一个自己所属的namespace命名空间，并且会将Mapper的 namespace作为它们的ID；

**多个Mapper共用一个Cache缓存对象**（使用`<cache-ref>`节点配置）

如果你想让多个Mapper公用一个Cache的话，你可以使用`<cache-ref namespace="">`节点，来指定你的这个Mapper使用到了哪一个Mapper的Cache缓存。

![](.\SecondaryCache02.png)

### 使用方法

要启用全局的二级缓存，只需要在你的 SQL 映射文件中添加一行：

```xml
<cache/>
```

基本上就是这样。这个简单语句的效果如下:

- 映射语句文件中的所有 select 语句的结果将会被缓存。
- 映射语句文件中的所有 insert、update 和 delete 语句会刷新缓存。
- 缓存会使用最近最少使用算法（LRU, Least Recently Used）算法来清除不需要的缓存。
- 缓存不会定时进行刷新（也就是说，没有刷新间隔）。
- 缓存会保存列表或对象（无论查询方法返回哪种）的 1024 个引用。
- 缓存会被视为读/写缓存，这意味着获取到的对象并不是共享的，可以安全地被调用者修改，而不干扰其他调用者或线程所做的潜在修改。

这些属性可以通过 cache 元素的属性来修改。比如：

```xml
<cache
  eviction="FIFO"
  flushInterval="60000"
  size="512"
  readOnly="true"/>
```

这个更高级的配置创建了一个 FIFO 缓存，每隔 60 秒刷新，最多可以存储结果对象或列表的 512 个引用，而且返回的对象被认为是只读的，因此对它们进行修改可能会在不同线程中的调用者产生冲突。

可用的清除策略(eviction)有：

- `LRU` – 最近最少使用：移除最长时间不被使用的对象。
- `FIFO` – 先进先出：按对象进入缓存的顺序来移除它们。
- `SOFT` – 软引用：基于垃圾回收器状态和软引用规则移除对象。
- `WEAK` – 弱引用：更积极地基于垃圾收集器状态和弱引用规则移除对象。

flushInterval（刷新间隔）属性可以被设置为任意的正整数，设置的值应该是一个以毫秒为单位的合理时间量。 默认情况是不设置，也就是没有刷新间隔，缓存仅仅会在调用语句时刷新。

size（引用数目）属性可以被设置为任意正整数，要注意欲缓存对象的大小和运行环境中可用的内存资源。默认值是 1024。

readOnly（只读）属性可以被设置为 true 或 false。只读的缓存会给所有调用者返回缓存对象的相同实例。 因此这些对象不能被修改。这就提供了可观的性能提升。而可读写的缓存会（通过序列化）返回缓存对象的拷贝。 速度上会慢一些，但是更安全，因此默认值是 false。



## 动态SQL

动态SQL 是一项能够执行某些SQL 命令的技术，这些命令直到即将执行时才是已知的。通过使用动态SQL，执行的SQL 可在程序运行时发生更改。

### If

使用动态 SQL 最常见情景是根据条件包含 where 子句的一部分。比如：

```xml
<select id="findActiveBlogWithTitleLike"
     resultType="Blog">
  SELECT * FROM BLOG
  WHERE state = ‘ACTIVE’
  <if test="title != null">
    AND title like #{title}
  </if>
</select>
```

这条语句提供了可选的查找文本功能。如果不传入 “title”，那么所有处于 “ACTIVE” 状态的 BLOG 都会返回；如果传入了 “title” 参数，那么就会对 “title” 一列进行模糊查找并返回对应的 BLOG 结果

如果希望通过 “title” 和 “author” 两个参数进行可选搜索该怎么办呢？首先，我想先将语句名称修改成更名副其实的名称；接下来，只需要加入另一个条件即可。

```xml
<select id="findActiveBlogLike"
     resultType="Blog">
  SELECT * FROM BLOG WHERE state = ‘ACTIVE’
  <if test="title != null">
    AND title like #{title}
  </if>
  <if test="author != null and author.name != null">
    AND author_name like #{author.name}
  </if>
</select>
```

## choose、when、otherwise

有时候，我们不想使用所有的条件，而只是想从多个条件中选择一个使用。针对这种情况，MyBatis 提供了 choose 元素，它有点像 Java 中的 switch 语句。

还是上面的例子，但是策略变为：传入了 “title” 就按 “title” 查找，传入了 “author” 就按 “author” 查找的情形。若两者都没有传入，就返回标记为 featured 的 BLOG

```xml
<select id="findActiveBlogLike"
     resultType="Blog">
  SELECT * FROM BLOG WHERE state = ‘ACTIVE’
  <choose>
    <when test="title != null">
      AND title like #{title}
    </when>
    <when test="author != null and author.name != null">
      AND author_name like #{author.name}
    </when>
    <otherwise>
      AND featured = 1
    </otherwise>
  </choose>
</select>
```

## trim、where、set

```xml
<select id="findActiveBlogLike"
     resultType="Blog">
  SELECT * FROM BLOG
  <where>
    <if test="state != null">
         state = #{state}
    </if>
    <if test="title != null">
        AND title like #{title}
    </if>
    <if test="author != null and author.name != null">
        AND author_name like #{author.name}
    </if>
  </where>
</select>
```

*where* 元素只会在子元素返回任何内容的情况下才插入 “WHERE” 子句。而且，若子句的开头为 “AND” 或 “OR”，*where* 元素也会将它们去除。

如果 *where* 元素与你期望的不太一样，你也可以通过自定义 trim 元素来定制 *where* 元素的功能。比如，和 *where* 元素等价的自定义 trim 元素为：

```xml
<trim prefix="WHERE" prefixOverrides="AND |OR ">
  ...
</trim>
```

*prefixOverrides* 属性会忽略通过管道符分隔的文本序列（注意此例中的空格是必要的）。上述例子会移除所有 *prefixOverrides* 属性中指定的内容，并且插入 *prefix* 属性中指定的内容。

用于动态更新语句的类似解决方案叫做 *set*。*set* 元素可以用于动态包含需要更新的列，忽略其它不更新的列。比如：

```xml
<update id="updateAuthorIfNecessary">
  update Author
    <set>
      <if test="username != null">username=#{username},</if>
      <if test="password != null">password=#{password},</if>
      <if test="email != null">email=#{email},</if>
      <if test="bio != null">bio=#{bio}</if>
    </set>
  where id=#{id}
</update>
```

这个例子中，*set* 元素会动态地在行首插入 SET 关键字，并会删掉额外的逗号（这些逗号是在使用条件语句给列赋值时引入的）。

或者，你可以通过使用*trim*元素来达到同样的效果：

```xml
<trim prefix="SET" suffixOverrides=",">
  ...
</trim>
```

## foreach

动态 SQL 的另一个常见使用场景是对集合进行遍历（尤其是在构建 IN 条件语句的时候）。比如：

```xml
<select id="selectPostIn" resultType="domain.blog.Post">
  SELECT *
  FROM POST P
  <where>
    <foreach item="item" index="index" collection="list"
        open="ID in (" separator="," close=")" nullable="true">
          #{item}
    </foreach>
  </where>
</select>
```

*foreach* 元素的功能非常强大，它允许你指定一个集合，声明可以在元素体内使用的集合项（item）和索引（index）变量。它也允许你指定开头与结尾的字符串以及集合项迭代之间的分隔符。这个元素也不会错误地添加多余的分隔符。



## 批量操作

### 批量插入

```xml
<!-- The input to this SQL statement is expected to be a Java List -->
<insert id="batchInsert" parameterType="java.util.List" useGeneratedKeys="true">
    <!-- 
        resultType="long": The result of the SELECT LAST_INSERT_ID() statement (the generated ID) will be a long integer value.

        keyProperty="id": MyBatis will map the generated IDs back into a property called "id" within the objects in your input list.

        order="AFTER": The ID retrieval query will be executed after the batch insertion is complete.
        -->
        <selectKey resultType="long" keyProperty="id" order="AFTER">
            SELECT LAST_INSERT_ID()
        </selectKey>
        insert into tbl_emp (emp_id, emp_name, gender,email, d_id)
        values
    	<!--
        collection="list": MyBatis will iterate over the input List you provide.

        item="emps": Each individual element within the list will be temporarily named "emps" during each iteration of the loop.

        index="index": Provides an index variable if you need to track the position within the input list.

        separator=",": Inserts a comma between each set of values during the batch insert, creating proper SQL syntax.
        -->
        <foreach collection="list" item="emps" index="index" separator=",">
            ( #{emps.empId},#{emps.empName},#{emps.gender},#{emps.email},#{emps.dId} )
    	</foreach>
</insert>
```

### 批量更新

```xml
<update id="updateBatch">
        update tbl_emp set
        emp_name =
        <foreach collection="list" item="emps" index="index" separator=" " open="case emp_id" close="end">
            when #{emps.empId} then #{emps.empName}
        </foreach>
        ,gender =
        <foreach collection="list" item="emps" index="index" separator=" " open="case emp_id" close="end">
            when #{emps.empId} then #{emps.gender}
        </foreach>
        ,email =
        <foreach collection="list" item="emps" index="index" separator=" " open="case emp_id" close="end">
            when #{emps.empId} then #{emps.email}
        </foreach>
        where emp_id in
        <foreach collection="list" item="emps" index="index" separator="," open="(" close=")">
            #{emps.empId}
        </foreach>
    </update>
```



## 原理

### 初始化

```java
![mybatisStructure](.\mybatisStructure.png)// mybatis初始化
String resource = "mybatis-config.xml";  
InputStream inputStream = Resources.getResourceAsStream(resource);  
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
```

![](.\MyBatisInitilization.png)

SqlSessionFactoryBuilder相关的代码如下所示：

```java
//1. 调用SqlSessionFactoryBuilder对象的build(inputStream)方法
public SqlSessionFactory build(InputStream inputStream)  {
    return build(inputStream, null, null);  
}  

public SqlSessionFactory build(InputStream inputStream, String environment, Properties properties)  {  
    try  {  
        //2. 创建XMLConfigBuilder对象用来解析XML配置文件，生成Configuration对象  
        XMLConfigBuilder parser = new XMLConfigBuilder(inputStream, environment, properties);  
        //3. 将XML配置文件内的信息解析成Java对象Configuration对象  
        Configuration config = parser.parse();  
        //4. 根据Configuration对象创建出SqlSessionFactory对象  
        return build(config);  
    }
    // ...
}

// 5. SqlSessionFactoryBuilder根据Configuration对象创建一个DefaultSessionFactory对象并返回
public SqlSessionFactory build(Configuration config) {  
    return new DefaultSqlSessionFactory(config);  
}  
```

**创建Configuration对象的过程**

```java
public Configuration parse() {  
    if (parsed) {  
        throw new BuilderException("Each XMLConfigBuilder can only be used once.");  
    }  
    parsed = true;  
    //XMLConfigBuilder会将XML配置文件的信息转换为Document对象
    //然后使用XpathParser根据Xpath表达式获取基本的DOM节点Node信息
    XNode configurationNode = parser.evalNode("/configuration");  
    parseConfiguration(configurationNode);  
    return configuration;  
}  
/** 
 * 解析 "/configuration"节点下的子节点信息，然后将解析的结果设置到Configuration对象中 
 */  
private void parseConfiguration(XNode root) {  
    try {  
        //1.首先处理properties 节点     
        propertiesElement(root.evalNode("properties")); //issue #117 read properties first  
        //2.处理typeAliases  
        typeAliasesElement(root.evalNode("typeAliases"));  
        //之后同理处理mapper，envirnment等
    }
} 
```

### 解析方法：propertiesElement、environmentsElement

```java
public class XMLConfigBuilder extends BaseBuilder {

    private boolean parsed;
    // xml解析器
    private XPathParser parser;
    private String environment;
  
    // 解析mybatis配置文件中能配置的元素节点
    private void parseConfiguration(XNode root) {
        try {
            // 解析properties元素
            propertiesElement(root.evalNode("properties")); 
            // 解析environments元素
            environmentsElement(root.evalNode("environments")); 
            //...
        } 
        //...
    }
  
    
    //下面就看看解析properties的具体方法
    private void propertiesElement(XNode context) throws Exception {
        if (context != null) {
            // 将所有子节点的 name 以及value属性set进properties对象
            // Properties extends Hashtable<Object,Object>
            Properties defaults = context.getChildrenAsProperties();
            // 获取properties节点上 resource属性的值
            String resource = context.getStringAttribute("resource");
            // 获取properties节点上 url属性的值, resource和url不能同时配置
            String url = context.getStringAttribute("url");
            if (resource != null && url != null) {
                throw new BuilderException("The properties element cannot specify both a URL and a resource based property file reference.  Please specify one or the other.");
            }
            // 把解析出的properties文件set进Properties对象
            if (resource != null) {
                defaults.putAll(Resources.getResourceAsProperties(resource));
            } else if (url != null) {
                defaults.putAll(Resources.getUrlAsProperties(url));
            }
            // 将configuration对象中已配置的Properties属性与刚刚解析的融合
            // configuration这个对象会装载所解析mybatis配置文件的所有节点元素
            Properties vars = configuration.getVariables();
            if (vars != null) {
                defaults.putAll(vars);
            }
            // 把装有解析配置propertis对象set进解析器， 因为后面可能会用到
            parser.setVariables(defaults);
            // set进configuration对象
            configuration.setVariables(defaults);
        }
    }
    
    //下面再看看解析enviroments元素节点的方法
    private void environmentsElement(XNode context) throws Exception {
        if (context != null) {
            if (environment == null) {
                //解析environments节点的default属性的值
                //例如: <environments default="development">
                environment = context.getStringAttribute("default");
            }
            //递归解析environments子节点
            for (XNode child : context.getChildren()) {
                //<environment id="development">, 只有enviroment节点有id属性，那么这个属性有何作用？
                //environments 节点下可以拥有多个 environment子节点
                //类似于这样： <environments default="development"><environment id="development">...</environment><environment id="test">...</environments>
                //意思就是我们可以对应多个环境，比如开发环境，测试环境等， 由environments的default属性去选择对应的enviroment
                String id = child.getStringAttribute("id");
                //根据由environments的default属性去选择对应的enviroment
                if (isSpecifiedEnvironment(id)) {
                    //事务， mybatis有两种：JDBC 和 MANAGED, 配置为JDBC则直接使用JDBC的事务，配置为MANAGED则是将事务托管给容器， 
                    TransactionFactory txFactory = transactionManagerElement(child.evalNode("transactionManager"));
                    //enviroment节点下面就是dataSource节点了，解析dataSource节点（下面会贴出解析dataSource的具体方法）
                    DataSourceFactory dsFactory = dataSourceElement(child.evalNode("dataSource"));
                    DataSource dataSource = dsFactory.getDataSource();
                    Environment.Builder environmentBuilder = new Environment.Builder(id)
                          .transactionFactory(txFactory)
                          .dataSource(dataSource);
                    //老规矩，会将dataSource设置进configuration对象
                    configuration.setEnvironment(environmentBuilder.build());
                }
            }
        }
    }
    
    //下面看看dataSource的解析方法
    private DataSourceFactory dataSourceElement(XNode context) throws Exception {
        if (context != null) {
            //dataSource的连接池
            String type = context.getStringAttribute("type");
            //子节点 name, value属性set进一个properties对象
            Properties props = context.getChildrenAsProperties();
            //创建dataSourceFactory
            DataSourceFactory factory = (DataSourceFactory) resolveClass(type).newInstance();
            factory.setProperties(props);
            return factory;
        }
        throw new BuilderException("Environment declaration requires a DataSourceFactory.");
    } 
}
```

上面我们看到，在配置dataSource的时候使用了 ${driver} 这种表达式， 这种形式是怎么解析的？其实，是通过PropertyParser这个类解析：

```java
/**
 * 这个类解析${}这种形式的表达式
 */
public class PropertyParser {

    public static String parse(String string, Properties variables) {
        VariableTokenHandler handler = new VariableTokenHandler(variables);
        GenericTokenParser parser = new GenericTokenParser("${", "}", handler);
        return parser.parse(string);
    }

    private static class VariableTokenHandler implements TokenHandler {
        private Properties variables;

        public VariableTokenHandler(Properties variables) {
            this.variables = variables;
        }

        public String handleToken(String content) {
            if (variables != null && variables.containsKey(content)) {
                return variables.getProperty(content);
            }
            return "${" + content + "}";
        }
    }
}
```

### Mapper解析

#### 子元素之cache解析

Mapper配置文件是由XMLMapperBuilder解析的，其中cacheElement方法负责解析cache元素，它通过调用CacheBuilder的相应方法完成cache的创建。每个cache内部都有一个唯一的ID，这个id的值就是namespace。创建好的cache对象存入configuration的cache缓存中（该缓存以cache的ID属性即namespace为key，这里再次体现了mybatis的namespace的强大用处）。

#### 子元素之cache-ref解析

cacheRefElement方法负责解析cache-ref元素，它通过调用CacheRefResolver的相应方法完成cache的引用。创建好的cache-ref引用关系存入configuration的cacheRefMap缓存中。

#### 子元素之resultMap解析

resultMapElement方法负责解析resultMap元素，它通过调用ResultMapResolver的相应方法完成resultMap的解析。创建好的resultMap存入configuration的resultMaps缓存中（该缓存以namespace+resultMap的id为key，这里再次体现了mybatis的namespace的强大用处）。

#### 子元素之sql解析

sqlElement方法负责解析sql元素。id属性用于区分不同的sql元素，在同一个mapper配置文件中可以配置多个sql元素。

#### **子元素之statement解析**

`buildStatementFromContext`方法负责解析statement元素。id属性用于区分不同的statement元素，在同一个配置文件中可以配置多个statement元素。通过调用`XMLStatementBuilder`的`parseStatementNode`方法完成解析。在这个方法内有几个重要的步骤，理解他们对正确的配置statement元素很有帮助。

**动态解析子元素**

statement节点可以配置各种子元素，比如前面提到的include子元素和selectKey子元素等（在动态sql里还有更多的子元素，具体参考mybatis的官方文档）。动态解析子元素通过parseDynamicTags方法完成。该方法根据子元素的类型递归的解析成一个个的SqlNode，这些SqlNode对象提供了apply方法，供后续调用时生成sql语句所需。需要注意的是SelectKey没有对应的SqlNode对象，因为它的功能是用来生成KeyGenerator对象的（具体来说是SelectKeyGenerator对象)。

**生成SqlSource**

SqlSource用于后续调用时根据SqlNode和参数对象生成sql语句。它接收一个叫做rootsqlNode的对象作为构造参数。

**生成KeyGenerator**

如果配置了selectKey子元素，KeyGenerator直接使用selectKey子元素里生成的KeyGenerator对象（具体来说是SelectKeyGenerator对象）。若没配置，则如果useGeneratedKeys属性的值为"true"且配置了 keyProperty属性，则生成默认的Jdbc3KeyGenerator对象，该对象调用JDBC驱动的getGeneratedKeys方法返回insert语句执行后生成的自增长主键。

**创建MappedStatement**

MappedStatement对象封装了statement元素的所有属性以及子节点值，MappedStatement对象有一个id属性用于唯一标记它，这个id由namespace加statement元素的id属性值构成。创建好的MappedStatement对象存入Configuration对象的mappedStatements缓存中，key为MappedStatement对象的id值。

XMLMapperBuilder.java：

```java
  private void buildStatementFromContext(List<XNode> list) {
    if (configuration.getDatabaseId() != null) {
      buildStatementFromContext(list, configuration.getDatabaseId());
    }
      //解析所有statement，获得mappedStatement对象
    buildStatementFromContext(list, null);
  }

  private void buildStatementFromContext(List<XNode> list, String requiredDatabaseId) {
    for (XNode context : list) {
      final XMLStatementBuilder statementParser = new XMLStatementBuilder(configuration, builderAssistant, context, requiredDatabaseId);
      try {
          //增删查改(CRUD)标签的解析，把sql语句放进mappedStatement
          //是一个
        statementParser.parseStatementNode();
      } catch (IncompleteElementException e) {
        configuration.addIncompleteStatement(statementParser);
      }
    }
  }
```

XMLStatementBuilder.java：

```java
 public void parseStatementNode() {
    String id = context.getStringAttribute("id");
    String databaseId = context.getStringAttribute("databaseId");

    if (!databaseIdMatchesCurrent(id, databaseId, this.requiredDatabaseId)) return;
	
     //准备工作
	// ...

    // Parse selectKey after includes and remove them.
    processSelectKeyNodes(id, parameterTypeClass, langDriver);
     
	//...
     
     //sql语句在这里被放进了一个mappedStatement
    builderAssistant.addMappedStatement(id, sqlSource, statementType, sqlCommandType,
        fetchSize, timeout, parameterMap, parameterTypeClass, resultMap, resultTypeClass,
        resultSetTypeEnum, flushCache, useCache, resultOrdered, 
        keyGenerator, keyProperty, keyColumn, databaseId, langDriver, resultSets);
  }
```

`mappedStatements`是一个map类的实现，通过`mappedStatements.put(ms.getId(), ms)`  放入处理过的sql语句

**注册mapper类型**

我们知道每个mapper配置文件的namespace属性对应于某个接口，应用程序通过接口访问mybatis时，mybatis会为这个接口生成一个代理对象，这个对象就叫`mapper`对象，在生成代理对象前mybatis会校验接口是否已注册，未注册的接口会产生一个异常。为了避免这种异常，就需要注册`mapper`类型。这个步骤是在`XMLMapperBuilder`的`bindMapperForNamespace`方法中完成的。它通过调用Configuration对象的`addMapper`方法完成，而Configuration对象的`addMapper`方法是通过MapperRegistry的`addMapper`方法完成的，它只是简单的将namespace属性对应的接口类型存入本地缓存中： `knownMappers.put(type, new MapperProxyFactory<>(type))`

```java
  private void bindMapperForNamespace() {
    String namespace = builderAssistant.getCurrentNamespace();
    if (namespace != null) {
      Class<?> boundType = null;
      try {
        boundType = Resources.classForName(namespace);
      } catch (ClassNotFoundException e) {
        //ignore, bound type is not required
      }
      if (boundType != null) {
        if (!configuration.hasMapper(boundType)) {
          // Spring may not know the real resource name so we set a flag
          // to prevent loading again this resource from the mapper interface
          // look at MapperAnnotationBuilder#loadXmlResource
          configuration.addLoadedResource("namespace:" + namespace);
          configuration.addMapper(boundType);
        }
      }
    }
  }
```



### 创建会话(SqlSession)

正如其名，Sqlsession对应着一次数据库会话。由于数据库会话不是永久的，因此Sqlsession的生命周期也不应该是永久的，相反，在你每次访问数据库时都需要创建它（当然并不是说在Sqlsession里只能执行一次sql，你可以执行多次，当一旦关闭了Sqlsession就需要重新创建它）。

那么咱们就先看看是怎么获取SqlSession的吧:

![](.\SqlSession.jpg)

首先，SqlSessionFactoryBuilder去读取mybatis的配置文件，然后build一个DefaultSqlSessionFactory。源码如下：

```java
 public SqlSessionFactory build(Reader reader, String environment, Properties properties) {
   try {
     //通过XMLConfigBuilder解析配置文件，解析的配置相关信息都会封装为一个Configuration对象
     XMLConfigBuilder parser = new XMLConfigBuilder(reader, environment, properties);
     //这儿创建DefaultSessionFactory对象
     return build(parser.parse());
   }
     //...
 }

 public SqlSessionFactory build(Configuration config) {
   return new DefaultSqlSessionFactory(config);
 }
```

当我们获取到SqlSessionFactory之后，就可以通过SqlSessionFactory去获取SqlSession对象。源码如下：

```java
private SqlSession openSessionFromDataSource(ExecutorType execType, TransactionIsolationLevel level, boolean autoCommit) {
   Transaction tx = null;
   try {
     //通过Confuguration对象去获取Mybatis相关配置信息, Environment对象包含了数据源和事务的配置
     final Environment environment = configuration.getEnvironment();
     final TransactionFactory transactionFactory = getTransactionFactoryFromEnvironment(environment);
     tx = transactionFactory.newTransaction(environment.getDataSource(), level, autoCommit);
     //之前说了，从表面上来看，咱们是用sqlSession在执行sql语句， 实际呢，其实是通过excutor执行， excutor是对于Statement的封装
     final Executor executor = configuration.newExecutor(tx, execType);
     //关键看这儿，创建了一个DefaultSqlSession对象
     return new DefaultSqlSession(configuration, executor, autoCommit);
   } 
     //...
 }
```

创建sqlsession经过了以下几个主要步骤：

- 从配置中获取Environment；
- 从Environment中取得DataSource；
- 从Environment中取得TransactionFactory；
- 从DataSource里获取数据库连接对象Connection；
- 在取得的数据库连接上创建事务对象Transaction；
- 创建Executor对象（该对象非常重要，事实上sqlsession的所有操作都是通过它完成的）；
- 创建sqlsession对象。

### MapperProxy

```java
UserDao userMapper = sqlSession.getMapper(UserDao.class);  
```

使用mapper的原理是通过SqlSession从Configuration获取之前注册过的mapperProxyFactory产生mapper的动态代理mapperProxy，通过代理的`invoke()`执行sql语句

```java
public <T> T getMapper(Class<T> type, SqlSession sqlSession) {
   //从mappedRegistry中的knownMappers获取之前放入的mapperProxyFactory
   final MapperProxyFactory<T> mapperProxyFactory = (MapperProxyFactory<T>) knownMappers.get(type);
   if (mapperProxyFactory == null) {
     throw new BindingException("Type " + type + " is not known to the MapperRegistry.");
   }
   try {
     //关键在这儿
     return mapperProxyFactory.newInstance(sqlSession);
   } catch (Exception e) {
     throw new BindingException("Error getting mapper instance. Cause: " + e, e);
   }
 }
```

```java
 public T newInstance(SqlSession sqlSession) {
   final MapperProxy<T> mapperProxy = new MapperProxy<T>(sqlSession, mapperInterface, methodCache);
   return newInstance(mapperProxy);
 }

protected T newInstance(MapperProxy<T> mapperProxy) {
   //动态代理我们写的dao接口
   return (T) Proxy.newProxyInstance(mapperInterface.getClassLoader(), new Class[] { mapperInterface }, mapperProxy);
 }
```

我们知道对被代理对象的方法的访问都会落实到代理者的invoke上来，MapperProxy的invoke如下：

```java
  /**
   * MapperProxy在执行时会触发此方法
   */
  @Override
  public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
    if (Object.class.equals(method.getDeclaringClass())) {
      try {
        return method.invoke(this, args);
      } catch (Throwable t) {
        throw ExceptionUtil.unwrapThrowable(t);
      }
    }
    final MapperMethod mapperMethod = cachedMapperMethod(method);
    //二话不说，主要交给MapperMethod自己去管
    return mapperMethod.execute(sqlSession, args);
  }
```

#### MapperMethod

就像是一个分发者，他根据参数和返回值类型选择不同的sqlsession方法来执行。这样mapper对象与sqlsession就真正的关联起来了。

```java
  /**
   * 看着代码不少，不过其实就是先判断CRUD类型，然后根据类型去选择到底执行sqlSession中的哪个方法，绕了一圈，又转回sqlSession了
   * @param sqlSession
   * @param args
   * @return
   */
  public Object execute(SqlSession sqlSession, Object[] args) {
    Object result;
    if (SqlCommandType.INSERT == command.getType()) {
      Object param = method.convertArgsToSqlCommandParam(args);
      result = rowCountResult(sqlSession.insert(command.getName(), param));
    } else if (SqlCommandType.UPDATE == command.getType()) {
      Object param = method.convertArgsToSqlCommandParam(args);
      result = rowCountResult(sqlSession.update(command.getName(), param));
    } else if (SqlCommandType.DELETE == command.getType()) {
      Object param = method.convertArgsToSqlCommandParam(args);
      result = rowCountResult(sqlSession.delete(command.getName(), param));
    } else if (SqlCommandType.SELECT == command.getType()) {
      if (method.returnsVoid() && method.hasResultHandler()) {
        executeWithResultHandler(sqlSession, args);
        result = null;
      } else if (method.returnsMany()) {
        result = executeForMany(sqlSession, args);
      } else if (method.returnsMap()) {
        result = executeForMap(sqlSession, args);
      } else {
        Object param = method.convertArgsToSqlCommandParam(args);
        result = sqlSession.selectOne(command.getName(), param);
      }
    } else {
      throw new BindingException("Unknown execution method for: " + command.getName());
    }
    if (result == null && method.getReturnType().isPrimitive() && !method.returnsVoid()) {
      throw new BindingException("Mapper method '" + command.getName() 
          + " attempted to return null from a method with a primitive return type (" + method.getReturnType() + ").");
    }
    return result;
  }
```

sqlsession只是一个门面，真正发挥作用的是executor，对sqlsession方法的访问最终都会落到executor的相应方法上去。Executor分成两大类，一类是CacheExecutor，另一类是普通Executor。

### Excutor

Sqlsession对数据库的操作都是通过Executor来完成的,与Sqlsession一样，Executor也是动态创建的：

![](.\executor.png)

- **Executor创建的源代码**：

```java
public Executor newExecutor(Transaction transaction, ExecutorType executorType) {  
	// executorType默认是Simple
    executorType = executorType == null ? defaultExecutorType : executorType;  

    executorType = executorType == null ?ExecutorType.SIMPLE : executorType;  

    Executor executor;  
	//根据executorType创建基础executor
    if(ExecutorType.BATCH == executorType) {
        executor = new BatchExecutor(this,transaction);
    } else if(ExecutorType.REUSE == executorType) {
        executor = new ReuseExecutor(this,transaction);  
    } else {  
        executor = newSimpleExecutor(this, transaction);
    }
	//如果二级缓存开启，会在此基础上添加一个装饰器
    if (cacheEnabled) {
        executor = new CachingExecutor(executor);  
    }
    executor = (Executor) interceptorChain.pluginAll(executor);  
    return executor;  
} 
```

Executor对象是可以被插件拦截的，如果定义了针对Executor类型的插件，最终生成的Executor对象是被各个插件插入后的代理对象。

SqlSession的CRUD方法, 比如:

```java
  public <E> List<E> selectList(String statement, Object parameter, RowBounds rowBounds) {
    try {
      MappedStatement ms = configuration.getMappedStatement(statement);
      //CRUD实际上是交给Excetor去处理， excutor其实也只是穿了个马甲而已
      return executor.query(ms, wrapCollection(parameter), rowBounds, Executor.NO_RESULT_HANDLER);
    } catch (Exception e) {
      throw ExceptionFactory.wrapException("Error querying database.  Cause: " + e, e);
    } finally {
      ErrorContext.instance().reset();
    }
  }
```

#### Executor.query

- **CacheExecutor**

CacheExecutor有一个重要属性delegate，它保存的是某类普通的Executor，值在构照时传入。执行数据库update操作时，它直接调用delegate的update方法，执行query方法时先尝试从cache中取值，取不到再调用delegate的查询方法，并将查询结果存入cache中。代码如下：

```java
public List query(MappedStatement ms, Object parameterObject, RowBounds rowBounds,ResultHandler resultHandler) throws SQLException {  
    if (ms != null) {  
        Cache cache = ms.getCache();  
        if (cache != null) {  
            //如果进行过update操作，清空缓存
            flushCacheIfRequired(ms);  
            //读锁，允许多个线程同时读取
            cache.getReadWriteLock().readLock().lock();  
            try {  
                if (ms.isUseCache() && resultHandler ==null) {  
                    CacheKey key = createCacheKey(ms, parameterObject, rowBounds);  
                    final List cachedList = (List)cache.getObject(key);  
                    if (cachedList != null) {  
                        //如果cache命中，直接返回命中的条目
                        return cachedList;  
                    } else {  
                        //否则执行query，将结果加入cache
                        List list = delegate.query(ms,parameterObject, rowBounds, resultHandler);  
                        tcm.putObject(cache,key, list);  
                        return list;  
                    }  
                } else {  
                    return delegate.query(ms,parameterObject, rowBounds, resultHandler);  
                }  
            } finally {  
                cache.getReadWriteLock().readLock().unlock();  
            }
        }  
    }  
    return delegate.query(ms,parameterObject, rowBounds, resultHandler);  
}
```

- **普通Executor**

有3类，他们都继承于BaseExecutor，BatchExecutor专门用于执行批量sql操作，ReuseExecutor会重用statement执行sql操作，SimpleExecutor只是简单执行sql没有什么特别的。下面以SimpleExecutor为例：

```java
public List doQuery(MappedStatement ms, Object parameter, RowBounds rowBounds,ResultHandler resultHandler) throws SQLException {  
    Statement stmt = null;  
    try {  
        Configuration configuration = ms.getConfiguration();  
        //StatementHandler封装了Statement, 让 StatementHandler 去处理
        StatementHandler handler = configuration.newStatementHandler(this, ms,parameter, rowBounds,resultHandler);  
        stmt = prepareStatement(handler);  
        return handler.query(stmt, resultHandler);  
    } finally {  
        closeStatement(stmt);  
    }  
} 
```

Mybatis内置的ExecutorType有3种，默认的是simple，该模式下它为每个语句的执行创建一个新的预处理语句，单条提交sql；而batch模式重复使用已经预处理的语句， 并且批量执行所有更新语句，显然batch性能将更优；

但batch模式也有自己的问题，比如在Insert操作时，在事务没有提交之前，是没有办法获取到自增的id，这在某型情形下是不符合业务要求的；

通过走码和研读spring相关文件发现，在同一事务中batch模式和simple模式之间无法转换，由于本项目一开始选择了simple模式，所以碰到需要批量更新时，只能在单独的事务中进行；

在代码中使用batch模式可以使用以下方式:

```java
//从spring注入原有的sqlSessionTemplate
@Autowired
private SqlSessionTemplate sqlSessionTemplate;
 
public void testInsertBatchByTrue() {
    //新获取一个模式为BATCH，自动提交为false的session
    //如果自动提交设置为true,将无法控制提交的条数，改为最后统一提交，可能导致内存溢出
    SqlSession session = sqlSessionTemplate.getSqlSessionFactory().openSession(ExecutorType.BATCH, false);
    //通过新的session获取mapper
    fooMapper = session.getMapper(FooMapper.class);
    int size = 10000;
    try {
        for (int i = 0; i < size; i++) {
            Foo foo = new Foo();
            foo.setName(String.valueOf(System.currentTimeMillis()));
            fooMapper.insert(foo);
            if (i % 1000 == 0 || i == size - 1) {
                //手动每1000个一提交，提交后无法回滚
                session.commit();
                //清理缓存，防止溢出
                session.clearCache();
            }
        }
    } catch (Exception e) {
        //没有提交的数据可以回滚
        session.rollback();
    } finally {
        session.close();
    }
}
```

### StatementHandler

可以看出，Executor本质上也是个甩手掌柜，具体的事情原来是StatementHandler来完成的。当Executor将指挥棒交给StatementHandler后，接下来的工作就是StatementHandler的事了。我们先看看StatementHandler是如何创建的：

```java
public StatementHandler newStatementHandler(Executor executor, 					MappedStatement mappedStatement, ObjectparameterObject, RowBounds rowBounds, ResultHandler resultHandler) {  
    StatementHandler statementHandler = new RoutingStatementHandler(executor,mappedStatement,parameterObject,rowBounds, resultHandler);  
    statementHandler = (StatementHandler) interceptorChain.pluginAll(statementHandler);  
    return statementHandler;
} 
```

可以看到每次创建的StatementHandler都是RoutingStatementHandler，它只是一个分发者，他一个属性delegate用于指定用哪种具体的StatementHandler。可选的StatementHandler有SimpleStatementHandler、PreparedStatementHandler和CallableStatementHandler三种。选用哪种在mapper配置文件的每个statement里指定，默认的是PreparedStatementHandler。同时还要注意到StatementHandler是可以被拦截器拦截的，和Executor一样，被拦截器拦截后的对像是一个代理对象。由于mybatis没有实现数据库的物理分页，众多物理分页的实现都是在这个地方使用拦截器实现的

BaseStatementHandler的构造方法里有这么一句：

```java
this.boundSql= mappedStatement.getBoundSql(parameterObject);
```

它触发了sql 的解析，在解析sql的过程中，TypeHandler也被决断出来了，决断的原则就是根据参数的类型和参数对应的JDBC类型决定使用哪个TypeHandler。比如：参数类型是String的话就用StringTypeHandler，参数类型是整数的话就用IntegerTypeHandler等。

StatementHandler创建后需要执行一些初始操作，比如statement的开启和参数设置、对于PreparedStatement还需要执行参数的设置操作等。

handler.parameterize通过调用ParameterHandler的setParameters完成参数的设置，ParameterHandler随着StatementHandler的创建而创建，默认的实现是DefaultParameterHandler：

```java
public void setParameters(PreparedStatement ps) throws SQLException {  
    ErrorContext.instance().activity("settingparameters").object(mappedStatement.getParameterMap().getId());  
    //hold details about each parameter in the SQL statement, such as its name, intended data type, and JDBC type
    List<ParameterMapping> parameterMappings = boundSql.getParameterMappings();  
    if(parameterMappings != null) {  
        MetaObject metaObject = parameterObject == null ? null :configuration.newMetaObject(parameterObject);  
        for (int i = 0; i< parameterMappings.size(); i++) {  
            ParameterMapping parameterMapping = parameterMappings.get(i);  
            if(parameterMapping.getMode() != ParameterMode.OUT) {  
                Object value;  
                String propertyName = parameterMapping.getProperty();  
                PropertyTokenizer prop = newPropertyTokenizer(propertyName);  
                if (parameterObject == null) {  
                    value = null;  
                    //查找相关联的typeHandler
                } else if (typeHandlerRegistry.hasTypeHandler(parameterObject.getClass())){ 
                    value = parameterObject;  
                } else if (boundSql.hasAdditionalParameter(propertyName)){  
                    value = boundSql.getAdditionalParameter(propertyName);  
                } else if(propertyName.startsWith(ForEachSqlNode.ITEM_PREFIX)  
                          && boundSql.hasAdditionalParameter(prop.getName())){  
                    value = boundSql.getAdditionalParameter(prop.getName());  
                    if (value != null) {  
                        value = configuration.newMetaObject(value).getValue(propertyName.substring(prop.getName().length()));  
                    }  
                } else {  
                    value = metaObject == null ? null :metaObject.getValue(propertyName);  
                }  
                //使用查找到的typeHandler
                TypeHandler typeHandler = parameterMapping.getTypeHandler();  
                if (typeHandler == null) {  
                    throw new ExecutorException("Therewas no TypeHandler found for parameter " + propertyName  + " of statement " + mappedStatement.getId());  
                }  
                typeHandler.setParameter(ps, i + 1, value,parameterMapping.getJdbcType());  
            }  
        }  
    }  
} 
```

参数设置完毕后，执行数据库操作（update或query）。如果是query最后还有个查询结果的处理过程。

接下来，咱们看看StatementHandler 的一个实现类 PreparedStatementHandler（这也是我们最常用的，封装的是PreparedStatement）, 看看它使怎么去处理的：

```java
  public <E> List<E> query(Statement statement, ResultHandler resultHandler) throws SQLException {
    // 到此，原形毕露， PreparedStatement, 这个大家都已经滚瓜烂熟了吧
    PreparedStatement ps = (PreparedStatement) statement;
    ps.execute();
    // 结果交给了ResultSetHandler 去处理
    return resultSetHandler.<E> handleResultSets(ps);
  }
```

结果处理使用ResultSetHandler来完成，默认的ResultSetHandler是FastResultSetHandler。ResultSetHandler内部一条记录一条记录的处理，在处理每条记录的每一列时会调用TypeHandler转换结果，如下：

```java
protected boolean applyAutomaticMappings(ResultSet rs, List<String> unmappedColumnNames,MetaObject metaObject) throws SQLException {  
    boolean foundValues = false;  
    for (String columnName : unmappedColumnNames) {  
        final String property = metaObject.findProperty(columnName);  
        if (property!= null) {  
            final ClasspropertyType =metaObject.getSetterType(property);  
            if (typeHandlerRegistry.hasTypeHandler(propertyType)) {  
                final TypeHandler typeHandler = typeHandlerRegistry.getTypeHandler(propertyType);  
                final Object value = typeHandler.getResult(rs,columnName);  
                if (value != null) {  
                    metaObject.setValue(property, value);  
                    foundValues = true;  
                }  
            }  
        }  
    }  
    return foundValues;  
}
```



## Plugin

MyBatis 允许你在已映射语句执行过程中的某一点进行拦截调用。默认情况下，MyBatis允许使用插件来拦截的方法调用包括：

- Executor (update, query, flushStatements, commit, rollback, getTransaction, close, isClosed) 拦截执行器的方法
- ParameterHandler (getParameterObject, setParameters) 拦截参数的处理
- ResultSetHandler (handleResultSets, handleOutputParameters) 拦截结果集的处理
- StatementHandler (prepare, parameterize, batch, update, query) 拦截Sql语法构建的处理

Mybatis采用责任链模式，通过动态代理组织多个拦截器（插件），通过这些拦截器可以改变Mybatis的默认行为（诸如SQL重写之类的），由于插件会深入到Mybatis的核心，因此在编写自己的插件前最好了解下它的原理，以便写出安全高效的插件。

### 拦截器的使用

```java
public interface Interceptor {
  //实现拦截逻辑的地方，内部要通过invocation.proceed()显式地推进责任链前进，也就是调用下一个拦截器拦截目标方法
  Object intercept(Invocation invocation) throws Throwable;
    
  //用当前这个拦截器生成对目标target的代理，实际是通过Plugin.wrap(target,this)来完成的，把目标target和拦截器this传给了包装函数
  Object plugin(Object target);
    
  //用于设置额外的参数，参数配置在拦截器的Properties节点里
  void setProperties(Properties properties);
}

@Intercepts({@Signature(type= Executor.class, method = "update", args = {MappedStatement.class,Object.class})})
public class ExamplePlugin implements Interceptor {
  public Object intercept(Invocation invocation) throws Throwable {
    return invocation.proceed();
  }
  public Object plugin(Object target) {
    return Plugin.wrap(target, this);
  }
  public void setProperties(Properties properties) {
  }
}
```

全局xml配置：

```xml
<plugins>
    <plugin interceptor="org.format.mybatis.cache.interceptor.ExamplePlugin"></plugin>
</plugins>
```

这个拦截器拦截Executor接口的update方法（其实也就是SqlSession的新增，删除，修改操作），所有执行executor的update方法都会被该拦截器拦截到。

### 拦截器源码分析

XMLConfigBuilder解析MyBatis全局配置文件的pluginElement私有方法：

```java
private void pluginElement(XNode parent) throws Exception {
    if (parent != null) {
        for (XNode child : parent.getChildren()) {
            String interceptor = child.getStringAttribute("interceptor");
            Properties properties = child.getChildrenAsProperties();
            //通过反射实例化plugin节点中的interceptor属性表示的类
            Interceptor interceptorInstance = (Interceptor) resolveClass(interceptor).newInstance();
            interceptorInstance.setProperties(properties);
            configuration.addInterceptor(interceptorInstance);
        }
    }
}

//configuration中的addInteceptor
public void addInterceptor(Interceptor interceptor) {
    interceptorChain.addInterceptor(interceptor);
}
```

这个interceptorChain是Configuration的内部属性，类型为InterceptorChain，也就是一个拦截器链，我们来看下它的定义：

```java
public class InterceptorChain {

    private final List<Interceptor> interceptors = new ArrayList<Interceptor>();

    public Object pluginAll(Object target) {
        for (Interceptor interceptor : interceptors) {
            target = interceptor.plugin(target);
        }
        return target;
    }

    public void addInterceptor(Interceptor interceptor) {
        interceptors.add(interceptor);
    }

    public List<Interceptor> getInterceptors() {
        return Collections.unmodifiableList(interceptors);
    }

}
```

```java
xxxHandler = (xxxHandler) interceptorChain.pluginAll(xxxHandler)
```

这个方法在MyBatis的每一个操作(新增，删除，修改，查询)中都会被执行到，执行的先后顺序是`Executor`，`ParameterHandler`，`ResultSetHandler`，`StatementHandler`(其中`ParameterHandler`和`ResultSetHandler`的创建是在创建`StatementHandler` [3个可用的实现类`CallableStatementHandler`, `PreparedStatementHandler`, `SimpleStatementHandler`] 的时候，其构造函数调用的 [这3个实现类的构造函数其实都调用了父类`BaseStatementHandler`的构造函数] )。

这4个方法实例化了对应的对象之后，都会调用interceptorChain的pluginAll方法，InterceptorChain的pluginAll刚才已经介绍过了，就是遍历所有的拦截器，然后调用各个拦截器的plugin方法。注意：拦截器的plugin方法的返回值会直接被赋值给原先的对象。

由于可以拦截StatementHandler，这个接口主要处理sql语法的构建，因此比如分页的功能，可以用拦截器实现，只需要在拦截器的plugin方法中处理StatementHandler接口实现类中的sql即可，可使用反射实现。

### 代理链的生成

下面以Executor为例。Mybatis在创建Executor对象时会执行下面一行代码：

```java
executor =(Executor) interceptorChain.pluginAll(executor);
```

nterceptorChain里保存了所有的拦截器，它在mybatis初始化的时候创建。上面这句代码的含义是调用拦截器链里的每个拦截器依次对executor进行plugin（插入？）

对于plugin方法而言，其实Mybatis已经为我们提供了一个实现。Mybatis中有一个叫做Plugin的类，里面有一个静态方法wrap(Object target,Interceptor interceptor)，通过该方法可以决定要返回的对象是目标对象还是对应的代理。这里我们先来看一下Plugin的源码：

```java
package org.apache.ibatis.plugin;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
import java.util.HashMap;
import java.util.HashSet;
import java.util.Map;
import java.util.Set;

import org.apache.ibatis.reflection.ExceptionUtil;

//这个类是Mybatis拦截器的核心,大家可以看到该类继承了InvocationHandler
//又是JDK动态代理机制
public class Plugin implements InvocationHandler {

  //目标对象
  private Object target;
  //拦截器
  private Interceptor interceptor;
  //记录需要被拦截的类与方法
  private Map<Class<?>, Set<Method>> signatureMap;

  private Plugin(Object target, Interceptor interceptor, Map<Class<?>, Set<Method>> signatureMap) {
    this.target = target;
    this.interceptor = interceptor;
    this.signatureMap = signatureMap;
  }

  //一个静态方法,对一个目标对象进行包装，生成代理类。
  public static Object wrap(Object target, Interceptor interceptor) {
    //首先根据interceptor上面定义的注解 获取需要拦截的信息
    Map<Class<?>, Set<Method>> signatureMap = getSignatureMap(interceptor);
    //目标对象的Class
    Class<?> type = target.getClass();
    //返回需要拦截的接口信息
    Class<?>[] interfaces = getAllInterfaces(type, signatureMap);
    //如果长度为>0 则返回代理类 否则不做处理
    if (interfaces.length > 0) {
      return Proxy.newProxyInstance(
          type.getClassLoader(),
          interfaces,
          new Plugin(target, interceptor, signatureMap));
    }
    return target;
  }

  //代理对象每次调用的方法
  public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
    try {
      //通过method参数定义的类 去signatureMap当中查询需要拦截的方法集合
      Set<Method> methods = signatureMap.get(method.getDeclaringClass());
      //判断是否需要拦截
      if (methods != null && methods.contains(method)) {
        return interceptor.intercept(new Invocation(target, method, args));
      }
      //不拦截 直接通过目标对象调用方法
      return method.invoke(target, args);
    } catch (Exception e) {
      throw ExceptionUtil.unwrapThrowable(e);
    }
  }

  //根据拦截器接口（Interceptor）实现类上面的注解获取相关信息
  private static Map<Class<?>, Set<Method>> getSignatureMap(Interceptor interceptor) {
    //获取注解信息
    Intercepts interceptsAnnotation = interceptor.getClass().getAnnotation(Intercepts.class);
    //为空则抛出异常
    if (interceptsAnnotation == null) { // issue #251
      throw new PluginException("No @Intercepts annotation was found in interceptor " + interceptor.getClass().getName());      
    }
    //获得Signature注解信息
    Signature[] sigs = interceptsAnnotation.value();
    Map<Class<?>, Set<Method>> signatureMap = new HashMap<Class<?>, Set<Method>>();
    //循环注解信息
    for (Signature sig : sigs) {
      //根据Signature注解定义的type信息去signatureMap当中查询需要拦截方法的集合
      Set<Method> methods = signatureMap.get(sig.type());
      //第一次肯定为null 就创建一个并放入signatureMap
      if (methods == null) {
        methods = new HashSet<Method>();
        signatureMap.put(sig.type(), methods);
      }
      try {
        //找到sig.type当中定义的方法 并加入到集合
        Method method = sig.type().getMethod(sig.method(), sig.args());
        methods.add(method);
      } catch (NoSuchMethodException e) {
        throw new PluginException("Could not find method on " + sig.type() + " named " + sig.method() + ". Cause: " + e, e);
      }
    }
    return signatureMap;
  }

  //根据对象类型与signatureMap获取接口信息
  private static Class<?>[] getAllInterfaces(Class<?> type, Map<Class<?>, Set<Method>> signatureMap) {
    Set<Class<?>> interfaces = new HashSet<Class<?>>();
    //循环type类型的接口信息 如果该类型存在与signatureMap当中则加入到set当中去
    while (type != null) {
      for (Class<?> c : type.getInterfaces()) {
        if (signatureMap.containsKey(c)) {
          interfaces.add(c);
        }
      }
      type = type.getSuperclass();
    }
    //转换为数组返回
    return interfaces.toArray(new Class<?>[interfaces.size()]);
  }

}
```

 **getSignatureMap方法解释**：首先会拿到拦截器这个类的 @Interceptors注解，然后拿到这个注解的属性 @Signature注解集合，然后遍历这个集合，遍历的时候拿出 @Signature注解的type属性(Class类型)，然后根据这个type得到带有method属性和args属性的Method。由于 @Interceptors注解的 @Signature属性是一个属性，所以最终会返回一个以type为key，value为`Set<Method>`的Map。

```java
@Intercepts({@Signature(type= Executor.class, method = "update", args = {MappedStatement.class,Object.class})})
```

比如这个 @Interceptors注解会返回一个key为Executor，value为集合(这个集合只有一个元素，也就是Method实例，这个Method实例就是Executor接口的update方法，且这个方法带有MappedStatement和Object类型的参数)。这个Method实例是根据 @Signature的method和args属性得到的。如果args参数跟type类型的method方法对应不上，那么将会抛出异常。



**getAllInterfaces方法解释**： 根据目标实例target(这个target就是之前所说的MyBatis拦截器可以拦截的类，Executor,ParameterHandler,ResultSetHandler,StatementHandler)和它的父类们，返回signatureMap中含有target实现的接口数组。

所以Plugin这个类的作用就是根据 @Interceptors注解，得到这个注解的属性 @Signature数组，然后根据每个 @Signature注解的type，method，args属性使用反射找到对应的Method。最终根据调用的target对象实现的接口决定是否返回一个代理对象替代原先的target对象。

我们再次结合(Executor)interceptorChain.pluginAll(executor)这个语句来看，这个语句内部对executor执行了多次plugin,第一次plugin后通过Plugin.wrap方法生成了第一个代理类，姑且就叫executorProxy1，这个代理类的target属性是该executor对象。第二次plugin后通过Plugin.wrap方法生成了第二个代理类，姑且叫executorProxy2，这个代理类的target属性是executorProxy1...这样通过每个代理类的target属性就构成了一个代理链（从最后一个executorProxyN往前查找，通过target属性可以找到最原始的executor类）。

### 代理链上的拦截

比如MyBatis官网的例子，当Configuration调用newExecutor方法的时候，由于Executor接口的update(MappedStatement ms, Object parameter)方法被拦截器被截获。因此最终返回的是一个代理类Plugin，而不是Executor。这样调用方法的时候，如果是个代理类，那么会执行：

```java
public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {  
    try {  
       Set<Method> methods = signatureMap.get(method.getDeclaringClass());  
        if(methods != null && methods.contains(method)) {  
           // 调用代理类所属拦截器的intercept方法，  
           return interceptor.intercept(new Invocation(target, method, args));  
        }  
        return method.invoke(target, args);  
    } catch(Exception e) {  
        throw ExceptionUtil.unwrapThrowable(e);  
    }  
}
```

没错，如果找到对应的方法被代理之后，那么会执行Interceptor接口的interceptor方法。

在invoke里，如果方法签名和拦截中的签名一致，就调用拦截器的拦截方法。我们看到传递给拦截器的是一个Invocation对象，这个对象是什么样子的，他的功能又是什么呢？

```java
public class Invocation {  
  
    private Object target;  
    private Method method;  
    private Object[] args;  
   
    public Invocation(Object target, Method method, Object[] args) {  
        this.target =target;  
        this.method =method;  
        this.args =args;  
    }  
    ...  
  
    public Object proceed() throws InvocationTargetException, IllegalAccessException {  
        return method.invoke(target, args);  
    }  
} 
```

可以看到，Invocation类保存了代理对象的目标类，执行的目标类方法以及传递给它的参数。

在每个拦截器的intercept方法内，最后一个语句一定是return invocation.proceed()（不这么做的话拦截器链就断了，你的mybatis基本上就不能正常工作了）。invocation.proceed()只是简单的调用了下target的对应方法，如果target还是个代理，就又回到了上面的Plugin.invoke方法了。这样就形成了拦截器的调用链推进。

```java
public Object intercept(Invocation invocation) throws Throwable {  
    //完成代理类本身的逻辑  
    ...
    //通过invocation.proceed()方法完成调用链的推进
    return invocation.proceed();
}
```



## 分页插件

Mybatis的分页功能很弱，它是基于内存的分页（查出所有记录再按偏移量和limit取结果），在大数据量的情况下这样的分页基本上是没有用的。本文基于插件，通过拦截StatementHandler重写sql语句，实现数据库的物理分页。

### 为什么在StatementHandler拦截

在前面章节介绍了一次sqlsession的完整执行过程，从中可以知道sql的解析是在StatementHandler里完成的，所以为了重写sql需要拦截StatementHandler。

### MetaObject简介

MetaObject是Mybatis提供的一个的工具类，通过它包装一个对象后可以获取或设置该对象的原本不可访问的属性（比如那些私有属性）。它有个三个重要方法经常用到：

- `MetaObject forObject(Object object,ObjectFactory objectFactory, ObjectWrapperFactory objectWrapperFactory)` 用于包装对象；
- `Object getValue(String name)` 用于获取属性的值（支持OGNL的方法）；
- `void setValue(String name, Object value)` 用于设置属性的值（支持OGNL的方法）；

### 拦截器签名

```java
@Intercepts({@Signature(type =StatementHandler.class, method = "prepare", args ={Connection.class})})  
public class PageInterceptor implements Interceptor {  
    ...  
} 
```

从签名里可以看出，要拦截的目标类型是StatementHandler（注意：type只能配置成接口类型），拦截的方法是名称为prepare参数为Connection类型的方法。

### intercept实现

```java
public Object intercept(Invocation invocation) throws Throwable {  
     StatementHandler statementHandler = (StatementHandler) invocation.getTarget();  
     MetaObject metaStatementHandler = MetaObject.forObject(statementHandler,  
     DEFAULT_OBJECT_FACTORY, DEFAULT_OBJECT_WRAPPER_FACTORY);  
     // 分离代理对象链(由于目标类可能被多个拦截器拦截，从而形成多次代理，通过下面的两次循环  
     // 可以分离出最原始的的目标类)  
     while (metaStatementHandler.hasGetter("h")) {  
         Object object = metaStatementHandler.getValue("h");  
         metaStatementHandler = MetaObject.forObject(object, DEFAULT_OBJECT_FACTORY,   
         DEFAULT_OBJECT_WRAPPER_FACTORY);  
     }  
     // 分离最后一个代理对象的目标类  
     while (metaStatementHandler.hasGetter("target")) {  
         Object object = metaStatementHandler.getValue("target");  
         metaStatementHandler = MetaObject.forObject(object, DEFAULT_OBJECT_FACTORY,   
         DEFAULT_OBJECT_WRAPPER_FACTORY);  
     }  
     Configuration configuration = (Configuration) metaStatementHandler.  
     getValue("delegate.configuration");  
     dialect = configuration.getVariables().getProperty("dialect");  
     if (null == dialect || "".equals(dialect)) {  
         logger.warn("Property dialect is not setted,use default 'mysql' ");  
         dialect = defaultDialect;  
     }  
     pageSqlId = configuration.getVariables().getProperty("pageSqlId");  
     if (null == pageSqlId || "".equals(pageSqlId)) {  
         logger.warn("Property pageSqlId is not setted,use default '.*Page$' ");  
         pageSqlId = defaultPageSqlId;  
     }  
     MappedStatement mappedStatement = (MappedStatement)   
     metaStatementHandler.getValue("delegate.mappedStatement");  
     // 只重写需要分页的sql语句。通过MappedStatement的ID匹配，默认重写以Page结尾的  
     //  MappedStatement的sql  
     if (mappedStatement.getId().matches(pageSqlId)) {  
         BoundSql boundSql = (BoundSql) metaStatementHandler.getValue("delegate.boundSql");  
         Object parameterObject = boundSql.getParameterObject();  
         if (parameterObject == null) {  
             throw new NullPointerException("parameterObject is null!");  
         } else {  
             // 分页参数作为参数对象parameterObject的一个属性  
             PageParameter page = (PageParameter) metaStatementHandler  
                     .getValue("delegate.boundSql.parameterObject.page");  
             String sql = boundSql.getSql();  
             // 重写sql  
             String pageSql = buildPageSql(sql, page);  
             metaStatementHandler.setValue("delegate.boundSql.sql", pageSql);  
             // 采用物理分页后，就不需要mybatis的内存分页了，所以重置下面的两个参数  
             metaStatementHandler.setValue("delegate.rowBounds.offset",   
             RowBounds.NO_ROW_OFFSET);  
             metaStatementHandler.setValue("delegate.rowBounds.limit", RowBounds.NO_ROW_LIMIT);  
             Connection connection = (Connection) invocation.getArgs()[0];  
             // 重设分页参数里的总页数等  
             setPageParameter(sql, connection, mappedStatement, boundSql, page);  
         }  
     }  
     // 将执行权交给下一个拦截器  
     return invocation.proceed();  
}
```

StatementHandler的默认实现类是RoutingStatementHandler，因此拦截的实际对象是它。RoutingStatementHandler的主要功能是分发，它根据配置Statement类型创建真正执行数据库操作的StatementHandler，并将其保存到delegate属性里。由于delegate是一个私有属性并且没有提供访问它的方法，因此需要借助MetaObject的帮忙。通过MetaObject的封装后我们可以轻易的获得想要的属性。

在上面的方法里有个两个循环，通过他们可以分离出原始的RoutingStatementHandler（而不是代理对象）。

前面提到，签名里配置的要拦截的目标类型是StatementHandler拦截的方法是名称为prepare参数为Connection类型的方法，而这个方法是每次数据库访问都要执行的。因为我是通过重写sql的方式实现分页，为了不影响其他sql（update或不需要分页的query），我采用了通过ID匹配的方式过滤。默认的过滤方式只对id以Page结尾的进行拦截（注意区分大小写），如下：

```xml
<select id="queryUserByPage" parameterType="UserDto" resultType="UserDto">  
    select * from t_user t where t.username = #{username} 
</select>
```

### sql重写

sql重写其实在原始的sql语句上加入分页的参数，目前支持mysql和oracle两种数据库的分页。

```java
private String buildPageSql(String sql, PageParameter page) {  
    if (page != null) {  
        StringBuilder pageSql = new StringBuilder();  
        if ("mysql".equals(dialect)) {  
            pageSql = buildPageSqlForMysql(sql, page);  
        } else if ("oracle".equals(dialect)) {  
            pageSql = buildPageSqlForOracle(sql, page);  
        } else {  
            return sql;  
        }  
        return pageSql.toString();  
    } else {  
        return sql;  
    }  
}  

public StringBuilder buildPageSqlForMysql(String sql, PageParameter page) {  
    StringBuilder pageSql = new StringBuilder(100);  
    String beginrow = String.valueOf((page.getCurrentPage() - 1) * page.getPageSize());  
    pageSql.append(sql);  
    pageSql.append(" limit " + beginrow + "," + page.getPageSize());  
    return pageSql;  
}  
```

### plugin实现

```java
public Object plugin(Object target) {  
    // 当目标类是StatementHandler类型时，才包装目标类，否者直接返回目标本身,减少目标被代理的  
    // 次数  
    if (target instanceof StatementHandler) {  
        return Plugin.wrap(target, this);  
    } else {  
        return target;  
    }  
}
```

