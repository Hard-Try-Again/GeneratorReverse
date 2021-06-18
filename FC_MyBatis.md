

![001](Pictures/001.png)



## MyBatis

### 引言

#### 什么是框架

框架（Framework）是整个或部分系统的可重用设计，表现为一组抽象构件及构件实例间交互的方法;另一种定

义认为，框架是可被应用开发者定制的应用骨架。前者是从应用方面而后者是从目的方面给出的定义。

简而言之，框架其实就是某种应用的半成品，就是一组组件，供你选用完成你自己的系统。简单说就是使用别

人搭好的舞台，你来做表演。而且，框架一般是成熟的，不断升级的软件。

> 软件的半成品，解决了软件开发过程当中的普适性问题，从而简化了开发步骤，提高了开发的效率。

#### 框架要解决的问题

框架要解决的最重要的一个问题是技术整合的问题，在 J2EE 的 框架中，有着各种各样的技术，不同的软件企

业需要从 J2EE 中选择不同的技术，这就使得软件企业最终的应用依赖于这些技术，技术自身的复杂性和技术

的风险性将会直接对应用造成冲击。而应用是软件企业的核心，是竞争力的关键所在，因此应该将应用自身的

设计和具体的实现技术解耦。这样，软件企业的研发将集中在应用的设计上，而不是具体的技术实现，技术实

现是应用的底层支撑，它不应该直接对应用产生影响。

> 框架一般处在低层应用平台（如 **J2EE**）和高层业务逻辑之间的中间层。

#### 软件开发的分层重要性

框架的重要性在于它实现了部分功能，并且能够很好的将低层应用平台和高层业务逻辑进行了缓和。为了实现软件工程中的“高内聚、低耦合”。把问题划分开来各个解决，易于控制，易于延展，易于分配资源。我们常见的 MVC 软件设计思想就是很好的分层思想。

![](Pictures/MVC概述.bmp)

#### 什么是 ORM 框架

> * ORM（Object Relational Mapping）对象关系映射，将程序中的[一个对象与表中的一行数据一一对应]()。
> * ORM 框架提供了持久化类与表的映射关系，在运行时参照映射文件的信息，[把对象持久化存储到数据库中]()。

####  使用 JDBC 完成 ORM 操作的缺点

> * 存在大量的冗余代码。
>
> * 手动创建 Connection、Statement 等。
>
> * 手动将结果集封装成实体对象。
>
> * 查询效率低，没有对数据访问进行过优化（Not Cache）。

###  MyBatis 框架概述

#### 概念

> * MyBatis 本是 apache 的一个开源项目 iBatis，2010年这个项目由 apache software foundation 迁移到了google code，并且改名为 MyBatis 。2013年11月迁移到 Github。
> * iBATIS 一词来源于 “internet” 和 “abatis” 的组合，是一个基于 Java 的持久层框架。iBATIS 提供的持久层框架包括 SQL Maps 和 Data Access Objects（DAOs）
> * MyBatis 是一个优秀的基于Java的持久层框架，支持自定义SQL，存储过程和高级映射。
> * **MyBatis 对原有 JDBC 操作进行了封装，几乎消除了所有 JDBC 代码，使开发者只需关注 SQL 本身**。
> * MyBatis 可以使用简单的 XML 或 Annotation 来配置执行 SQL，并自动完成 ORM 操作，将执行结果作为一个实体类进行返回，完成数据库中的记录与接口、POJO 的映射。
> * 当前，最新版本是 MyBatis 3.5.6 ，其发布时间是2020年10月7日。

#### 访问与下载

> 官方网站
>
> ```http
> https://mybatis.org/mybatis-3/
> ```
>
> 
>
> 下载地址
>
> ```http
> https://github.com/mybatis/mybatis-3/releases
> ```
>
> 
>
> 官方中文文档
>
> ```http
> https://mybatis.org/mybatis-3/zh/index.html
> ```

### MyBatis 框架原理图解【重点】

![](Pictures/MyBatis执行流程.png)

1、MyBatis 配置

MyBatis.xml，此文件作为 MyBatis 的全局配置文件，配置了 MyBatis 的运行环境等信息。 

mapper.xml 文件即 sql 映射文件，文件中配置了操作数据库的 sql 语句。此文件需要在 MyBatis.xml 中加载。  

2、通过 MyBatis 环境等配置信息构造 SqlSessionFactory 即会话工厂

3、由会话工厂创建 sqlSession 即会话，操作数据库需要通过 sqlSession 进行。

4、MyBatis 底层自定义了 Executor 执行器接口操作数据库，Executor 接口有两个实现，一个是基本执行器、一个是缓存执行器。

5、Mapped Statement也是 MyBatis 一个底层封装对象，它包装了 MyBatis 配置信息及sql映射信息等。mapper.xml文件中一个sql对应一个Mapped Statement对象，sql的id即是Mapped statement的id。

6、Mapped Statement 对 sql 执行输入参数进行定义，包括 HashMap、基本类型、实体类对象，Executor 通过 Mapped Statement 在执行sql前将输入的 java 对象映射至 sql 中，输入参数映射就是 jdbc 编程中对 preparedStatement 设置参数。

MappedStatement 对 sql 执行输出结果进行定义，包括 HashMap、基本类型、实体类对象，Executor 通过 Mapped Statement 在执行 sql 后将输出结果映射至 java 对象中，输出结果映射过程相当于 jdbc 编程中对结果的解析处理过程。

### MyBatis 初识【重点】

#### 使用流程【固定格式】

```
1、pom.xml中引入核心依赖
2、声明Mybatis的配置文件
3、声明dao层接口
4、声明接口对应的mapper映射文件
5、将mapper映射文件注册到MyBatis配置文件中
6、根据流程操作获取结果
```

#### 案例代码

1、pom.xml 中引入 MyBatis 核心依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.fc</groupId>
    <artifactId>02_MyBatis</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <!--mybatis依赖-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.6</version>
        </dependency>

        <!--数据库连接-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>

        <!--单元测试-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.10</version>
        </dependency>
    </dependencies>
</project>
```

2、创建 mybatis-config.xml 配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<!--MyBatis配置-->
<configuration>
    <!--JDBC环境配置、选中默认环境-->
    <environments default="development">
        <!--MySql数据库环境配置-->
        <environment id="development">
            <!--事务管理-->
            <transactionManager type="JDBC"/>
            <!--连接池相关配置-->
            <dataSource type="org.apache.ibatis.datasource.pooled.PooledDataSourceFactory">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/FC2021?useUnicode=true&amp;characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
</configuration>
```

>【注意】配置文件要放在 resources 文件目录下

3、建立数据表

```	mysql
create table student(
    id int primary key auto_increment, 
    name varchar(20) not null, 
    age tinyint, 
    gender char(1) default '女', 
    birthday datetime, 
    info text);
```

4、声明数据表对应的实体类

```java
public class Student {
    private Integer id;
    private String name;
    private Byte age;
    private String gender;
    private Date birthday;
    private String info;

    // Constructor、Setters、Getters
}
```

> 【注意】必须符合 JavaBean 规范

5、定义 dao 接口

```java
public interface StudentDao {
    Student selectStudentById(Integer id);
}
```

6、编写 Mapper 文件

mapper 规范

```
1、dao接口的全限定名要和mapper映射文件的 namespace 值一致。
2、dao接口的方法名要和mapper映射文件的 statement 的id一致。
3、dao接口的方法参数类型要和 mapper 映射文件的 statement 的parameterType的值一致，并且参数只能是一个。
4、dao接口的方法返回值类型要和mapper映射文件的statement的resultType的值一致。
```

StudentMapper.xml 配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace对应所需实现的接口全限定名-->
<mapper namespace="com.fc.dao.StudentDao">
    <!--id对应所需重写的接口抽象方法，resultType对应查询后所需返回的对象类型-->
    <select id="selectStudentById" resultType="com.fc.bean.Student">
        <!--SQL语句，参数使用固定格式： #{} -->
        SELECT * FROM student WHERE id = #{arg0}
    </select>
</mapper>
```

>【注意】mapper 配置文件建议声明在 resources 文件目录下

7、将 Mapper.xml 注册到 mybatis-config.xml 中

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<!--MyBatis配置-->
<configuration>
    
    <!--Mapper注册-->
    <mappers>
        <!--注册Mapper文件的所在位置-->
        <mapper resource="StudentMapper.xml"/>
    </mappers>
</configuration>
```

8、编写测试类

流程【固定格式】

```
1、读取配置文件
2、获取SqlSession工厂
3、获取SqlSession对象
4、通过SqlSession获取接口的代理对象
5、通过代理对象调用方法操作数据库
6、关闭资源【重点】
```

案例代码

```java
public class StudentTest {
    // 使用@Test注解进行单元测试
    @Test
    public void testSelectStudentById() {
        try {
            // 读取配置文件
            InputStream resource = Resources.getResourceAsStream("mybatis-config.xml");

            // 获取SqlSession工厂对象
            SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(resource);

            // 获取SqlSession对象
            SqlSession sqlSession = factory.openSession();

            // 通过SqlSession获取接口的代理对象
            StudentDao mapper = sqlSession.getMapper(StudentDao.class);

            // 通过代理对象调用查询方法操作数据库并获取实体类对象
            Student student = mapper.selectStudentById(1);
            
            // 关闭资源
            sqlSession.close();
            
            // 展示
            System.out.println(student);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

【补充】iBatis 传统操作方式

```java
public class StudentTest {
    @Test
    public void testTraditionalWay() {
        try {
            // 读取配置文件
            InputStream resource = Resources.getResourceAsStream("mybatis-config.xml");

            // 获取SqlSession工厂对象
            SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(resource);

            // 获取SqlSession对象
            SqlSession sqlSession = factory.openSession();

            // 使用SqlSession对象查询单条数据，需要的参数为接口中的方法的完整路径名以及参数，获取对应的实体类对象
            Object o = sqlSession.selectOne("com.fc.dao.StudentDao.selectStudentById", 2);
            
            // 关闭资源
            sqlSession.close();

            // 展示
            System.out.println(o);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### 细节补充

#### 特殊符号处理

在XML中，有一些符号作为作为XML 的标记符号，不能直接使用，可以通过其他方式对特定字符进行转换

#####  <![CDATA[]>

> 在 xml 文件中，被<![CDATA[]]>包括起来的内容会被原样输出

案例代码

```xml
select * from student where id <![CDATA[<=]]> 4
```

##### 实体引用（字符实体）

| 实体 | 实体引用 | 描述 |
| ---- | -------- | ---- |
| lt   | &lt;     | <    |
| gt   | &gt;     | >    |
| amp  | &amp;    | &    |
| apos | &apos;   | '    |
| quot | &quot;   | "    |

案例代码

```xml
select * from student where id &lt;= 4
```

#### resultType 和 resultMap

- 进行 select 映射的时候，返回类型可以用 resultType，也可以用 resultMap，resultType 直接表示返回类型，而 resultMap 则是对外部 resultMap 的引用，resultType 和 resultMap 不能同时存在。

- 在 MyBatis 进行查询映射时，其实查询出来的每一个属性都是放在一个对应的 Map 里面的，其中键是属性名，值则是其对应的值。

- 当提供的返回类型属性是 resultType 时，MyBatis 会将 Map 里面的键值对取出赋给 resultType 所指定的对象对应的属性。所以其实 MyBatis 的每一个查询映射的返回类型都是 ResultMap，只是当提供的返回类型属性是 resultType 的时候，MyBatis 对自动的给把对应的值赋给 resultType 所指定对象的属性。当提供的返回类型是 resultMap 时，因为 Map 不能很好表示领域模型，就需要自己再进一步的把它转化为对应的对象，这常常在复杂查询中很有作用。


#### \#{}与${}区别【重点】

- \#{}在预编译时，相当于一个参数占位符“?”，用来补全预编译语句，能够防止注入式攻击。
- ${}表示内容的原样输出，相当于单纯的内容替换，拼接完成后才会对SQL进行编译、执行。

#### 使用 properties 配置文件

> 对于mybatis-config.xml的核心配置中，如果存在需要频繁改动的数据内容，可以提取到properties中

声明 jdbc.properties

```properties
# jdbc.properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/FC2021?useUnicode=true&characterEncoding=UTF8
jdbc.username=root
jdbc.password=root
```

修改mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<!--MyBatis配置-->
<configuration>
    <!--添加properties配置文件路径(外部配置、动态替换)-->
    <properties resource="jdbc.properties" />

    <!--JDBC环境配置、选中默认环境-->
    <environments default="MySqlDB">
        <!--MySql数据库环境配置-->
        <environment id="MySqlDB">
            <!--事务管理-->
            <transactionManager type="JDBC"/>
            <!--连接池相关配置-->
            <dataSource type="org.apache.ibatis.datasource.pooled.PooledDataSourceFactory">
                <!--使用$ + 占位符-->
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>

    <!--Mapper注册-->
    <mappers>
        <!--注册Mapper文件的所在位置-->
        <mapper resource="StudentMapper.xml"/>
    </mappers>
</configuration>
```

#### 类型别名

> 为实体类定义别名，提高书写效率。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<!--MyBatis配置-->
<configuration>
    ...

    <!--定义别名-->
    <typeAliases>
        <!--给指定类起一个别名-->
        <typeAlias type="com.fc.bean.Student" alias="Student" />
    </typeAliases>

    ...
</configuration>
```

>【注意】声明别名的 typeAliases 标签必须跟在 properties 标签后，environments 标签之前

#### 使用 log4j 输出执行日志

1、pom.xml 配置文件中添加 log4j 依赖

```xml
<!-- log4j日志依赖 -->
<dependency>
	<groupId>log4j</groupId>
	<artifactId>log4j</artifactId>
	<version>1.2.17</version>
</dependency>
```

2、创建并配置 log4j.properties 文件

```properties
# Global logging configuration
log4j.rootLogger=DEBUG, stdout
# MyBatis logging configuration...
log4j.logger.org.mybatis.example.BlogMapper=TRACE
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```

### 参数绑定【重点】

#### 序号参数绑定【不要用】

1、dao 层接口

```java
public interface StudentSelectDao {
    /**
     * 根据id和name查询对应的学生
     * 使用原生序号参数绑定
     *
     * @param id 学生的id
     * @param name 学生的name
     * @return 返回对应的学生对象
     */
    Student selectStudentByIdAndName(Integer id, String name);
}
```

2、mapper 配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.fc.select.StudentSelectDao">
    <select id="selectStudentByIdAndName" resultType="Student" >
        select * from student where id = #{param1} and name = #{param2}
    </select>
</mapper>
```

>【注意】参数绑定除了能用 param (从1开始依次后续)还可以使用 arg (从0开始依次后续)
>
>```xml
><select id="selectStudentByIdAndName" resultType="Student" >
>    select * from student where id = #{arg0} and name = #{arg1}
></select>
>```

3、测试类

```java
public class StudentTest {	
	@Test
    public void testSelectStudentByIdAndName() {
        try {
            // 读取配置文件
            InputStream resource = Resources.getResourceAsStream("mybatis-config.xml");

            // 获取SqlSession工厂对象
            SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(resource);

            // 获取SqlSession对象
            SqlSession sqlSession = factory.openSession();

            // 通过SqlSession获取接口的代理对象
            StudentSelectDao mapper = sqlSession.getMapper(StudentSelectDao.class);

            // 调用接口中的方法
            Student student = mapper.selectStudentByIdAndName(1, "张三");

            // 关闭资源
            sqlSession.close();

            System.out.println(student);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### 注解参数绑定【推荐】

1、dao 层接口

```java
public interface StudentSelectDao {
    /**
     * 根据id和gender查询对应的学生
     * 使用@Param注解进行参数绑定
     *
     * @param id 学生的id
     * @param gender 学生的gender
     * @return 返回对应的学生对象
     */
    Student selectStudentByIdAndGender(@Param("id") Integer id, @Param("gender") String gender);
}
```

2、mapper 配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.fc.select.StudentSelectDao">
    <select id="selectStudentByIdAndGender" resultType="Student">
        <!--获取声明的注解中对应的值-->
        select * from student where id = #{id} and gender = #{gender}
    </select>
</mapper>
```

3、测试类

```java
public class StudentTest {
	@Test
    public void testSelectStudentByIdAndGender() {
        try {
            // 读取配置文件
            InputStream resource = Resources.getResourceAsStream("mybatis-config.xml");

            // 获取SqlSession工厂对象
            SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(resource);

            // 获取SqlSession对象
            SqlSession sqlSession = factory.openSession();

            // 通过SqlSession获取接口的代理对象
            StudentSelectDao mapper = sqlSession.getMapper(StudentSelectDao.class);

            // 调用接口中的方法
            Student student = mapper.selectStudentByIdAndGender(1, "男");

            // 关闭资源
            sqlSession.close();

            System.out.println(student);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### Map 参数绑定【不要用】

1、dao 层接口

```java
public interface StudentSelectDao {
    /**
     * 根据id和info查询对应的学生
     *
     * @param map 传入包含学生信息的map
     * @return 返回对应的学生对象
     */
    Student selectStudentByIdAndInfo(Map<String, Object> map);
}
```

2、mapper 配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.fc.select.StudentSelectDao">
    <select id="selectStudentByIdAndInfo" resultType="Student">
        <!--获取map中键对应的值-->
        select * from student where id = #{id} and info = #{info}
    </select>
</mapper>
```

3、测试类

```java
public class StudentTest {
	// 测试使用map进行参数绑定
    @Test
    public void selectStudentByIdAndInfo() {
        try {
            // 读取配置文件
            InputStream resource = Resources.getResourceAsStream("mybatis-config.xml");

            // 获取SqlSession工厂对象
            SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(resource);

            // 获取SqlSession对象
            SqlSession sqlSession = factory.openSession();

            // 通过SqlSession获取接口的代理对象
            StudentSelectDao mapper = sqlSession.getMapper(StudentSelectDao.class);

            // 声明一个map并赋值
            Map<String, Object> map = new HashMap<>();

            map.put("id", 1);
            map.put("info", "真帅");

            // 调用接口中的方法
            Student student = mapper.selectStudentByIdAndInfo(map);

            // 关闭资源
            sqlSession.close();

            System.out.println(student);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### 对象参数绑定【重点】

1、dao 层接口

```java
public interface StudentSelectDao {
    /**
     * 根据学生对象查询对应的学生
     *
     * @param student 传入一个学生对象
     * @return 返回对应的学生对象
     */
    Student selectStudentByStudent(Student student);
}
```

2、mapper 配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.fc.select.StudentSelectDao">
    <select id="selectStudentByStudent" resultType="Student">
        <!--获取对象中的属性-->
        select * from student where id = #{id} and name = #{name} and age = #{age} and gender = #{gender} and info = #{info}
    </select>
</mapper>
```

3、测试类

```java
public class StudentTest {
    // 测试使用对象进行参数绑定
	@Test
    public void selectStudentByStudent() {
        try {
            // 读取配置文件
            InputStream resource = Resources.getResourceAsStream("mybatis-config.xml");

            // 获取SqlSession工厂对象
            SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(resource);

            // 获取SqlSession对象
            SqlSession sqlSession = factory.openSession();

            // 通过SqlSession获取接口的代理对象
            StudentSelectDao mapper = sqlSession.getMapper(StudentSelectDao.class);

            // 声明一个学生对象并赋值
            Student student = new Student();
            student.setId(1);
            student.setName("张三");
            student.setAge((byte) 20);
            student.setGender("男");
            student.setInfo("真帅");

            // 调用接口中的方法
            Student result = mapper.selectStudentByStudent(student);

            // 关闭资源
            sqlSession.close();

            System.out.println(result);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### MyBatis 的 CRUD 操作【重点】

#### 查询

> 标签：< select id="" resultType="" >

##### 模糊查询

1、dao 层接口

```java
public interface StudentCrudDao {
    /**
     * 根据关键字进行模糊查询
     * 
     * @param forward 关键字
     * @return 返回一个包含学生对象的集合
     */
    List<Student> selectStudentByForward(@Param("forward") String forward);
}
```

2、mapper 配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.fc.crud.StudentCrudDao">
    <select id="selectStudentByForward" resultType="Student">
        select * from student where name like #{forward}
    </select>
</mapper>
```

3、测试类

```java
public class StudentCrudTest {
    @Test
    public void testSelectStudentByForward() {
        try {
            // 读取配置文件
            InputStream resource = Resources.getResourceAsStream("mybatis-config.xml");

            // 获取SqlSession工厂对象
            SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(resource);

            // 获取SqlSession对象
            SqlSession sqlSession = factory.openSession();

            // 通过SqlSession获取接口的代理对象
            StudentCrudDao mapper = sqlSession.getMapper(StudentCrudDao.class);

            // 调用接口中的方法并传入对应的参数
            List<Student> students = mapper.selectStudentByForward("%张%");

            // 关闭资源
            sqlSession.close();
            
            // 增强for循环遍历集合
            for (Student student : students) {
                System.out.println(student);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### 删除

> 标签：< delete id="" parameterType="" >

1、dao 层接口

```java
public interface StudentCrudDao {
    /**
     * 删除学生
     *
     * @param id 传入指定的id
     * @return 返回受影响的行数
     */
    int deleteStudent(@Param("id") Integer id);
}
```

2、mapper 配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.fc.crud.StudentCrudDao">
    <!--删除-->
    <delete id="deleteStudent" parameterType="Integer">
        delete from student where id = #{id}
    </delete>
</mapper>
```

>【注意】此处的 resultType 使用了别名

3、测试类

```java
public class StudentCrudTest {
    // 测试删除学生
    @Test
    public void testDeleteStudent() {
        try {
            // 读取配置文件
            InputStream resource = Resources.getResourceAsStream("mybatis-config.xml");

            // 获取SqlSession工厂对象
            SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(resource);

            // 获取SqlSession对象
            SqlSession sqlSession = factory.openSession();

            // 通过SqlSession获取接口的代理对象
            StudentCrudDao mapper = sqlSession.getMapper(StudentCrudDao.class);

            // 调用接口中的方法并传入对应的参数
            int affectedRows = mapper.deleteStudent(7);

            // 提交
            sqlSession.commit();

            // 关闭资源
            sqlSession.close();

            System.out.println(affectedRows);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

>【注意】默认关闭自动提交，需要手动 commit

#### 修改

> 标签：< update id="" parameterType="" >

1、dao 层接口

```java
public interface StudentCrudDao {
    /**
     * 修改学生
     *
     * @param student 传入一个学生对象
     * @return 返回受影响的行数
     */
    int updateStudent(Student student);
}
```

2、mapper 配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.fc.crud.StudentCrudDao">
    <!--修改-->
    <update id="updateStudent" parameterType="Student">
        update student set name = #{name}, gender = #{gender}, age = #{age}, info = #{info} where id = #{id}
    </update>
</mapper>
```

>【注意】此处的 resultType 使用了别名

3、测试类

```java
public class StudentCrudTest {
    // 测试修改学生
    @Test
    public void testUpdateStudent() {
        try {
            // 读取配置文件
            InputStream resource = Resources.getResourceAsStream("mybatis-config.xml");

            // 获取SqlSession工厂对象
            SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(resource);

            // 获取SqlSession对象
            SqlSession sqlSession = factory.openSession();

            // 通过SqlSession获取接口的代理对象
            StudentCrudDao mapper = sqlSession.getMapper(StudentCrudDao.class);

            // 声明学生对象并赋值
            Student student = new Student();

            student.setId(3);
            student.setName("王五");
            student.setGender("男");
            student.setAge((byte) 19);
            student.setBirthday(new Date());
            student.setInfo("多财多亿");

            // 调用接口中的方法并传入对应的参数
            int affectedRows = mapper.updateStudent(student);

            // 提交
            sqlSession.commit();

            // 关闭资源
            sqlSession.close();

            System.out.println(affectedRows);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

>【注意】默认关闭自动提交，需要手动 commit

#### 添加

> 标签：< insert id="" parameterType="" >

1、dao 层接口

```java
public interface StudentCrudDao {
    /**
     * 添加学生
     *
     * @param student 传入一个学生对象
     * @return 返回受影响的行数
     */
    int addStudent(Student student);
}
```

2、mapper 配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.fc.crud.StudentCrudDao">
    <!--插入-->
    <insert id="addStudent" parameterType="Student">
        insert into student(name, age, gender, birthday, info) values (#{name}, #{age}, #{gender}, #{birthday}, #{info})
    </insert>
</mapper>
```

>【注意】此处的 resultType 使用了别名

3、测试类

```java
public class StudentCrudTest {
    // 测试添加学生
    @Test
    public void testAddStudent() {
        try {
            // 读取配置文件
            InputStream resource = Resources.getResourceAsStream("mybatis-config.xml");

            // 获取SqlSession工厂对象
            SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(resource);

            // 获取SqlSession对象
            SqlSession sqlSession = factory.openSession();

            // 通过SqlSession获取接口的代理对象
            StudentCrudDao mapper = sqlSession.getMapper(StudentCrudDao.class);

            // 声明学生对象并赋值
            Student student = new Student();

            student.setName("二妞");
            student.setGender("女");
            student.setAge((byte) 18);
            student.setBirthday(new Date());
            student.setInfo("好看");

            // 调用接口中的方法并传入对应的参数
            int affectedRows = mapper.addStudent(student);

            // 提交
            sqlSession.commit();

            // 关闭资源
            sqlSession.close();

            System.out.println(affectedRows);

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

>【注意】默认关闭自动提交，需要手动 commit

#### 主键回填

> 标签：< selectKey id="" parameterType="" order="AFTER|BEFORE">

1、dao 层接口

```java
public interface StudentCrudDao {
    /**
     * 添加学生
     *
     * @param student 传入一个学生对象
     * @return 返回受影响的行数
     */
    int addStudent(Student student);
}
```

2、mapper 配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.fc.crud.StudentCrudDao">
    <!--将自增长id存入学生对象的id中-->
    <insert id="getAutoIncrement" parameterType="Student" useGeneratedKeys="true" keyProperty="id">
        insert into student(name, age, gender, birthday, info) values (#{name}, #{age}, #{gender}, #{birthday}, #{info})
    </insert>
</mapper>
```

>【注意】此处的 resultType 使用了别名，useGeneratedKeys 参数设置使用自增长 id 为 true，keyProperty 为自增长主键映射到的位置

3、测试类

```java
public class StudentCrudTest {
    // 测试获取自增长主键
    @Test
    public void testGetAutoIncrement() {
        try {
            // 读取配置文件
            InputStream resource = Resources.getResourceAsStream("mybatis-config.xml");

            // 获取SqlSession工厂对象
            SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(resource);

            // 获取SqlSession对象
            SqlSession sqlSession = factory.openSession();

            // 通过SqlSession获取接口的代理对象
            StudentCrudDao mapper = sqlSession.getMapper(StudentCrudDao.class);

            // 声明学生对象并赋值
            Student student = new Student();

            student.setName("玉田");
            student.setGender("男");
            student.setAge((byte) 20);
            student.setBirthday(new Date());
            student.setInfo("强壮");

            // 调用接口中的方法并传入对应的参数
            int affectedRows = mapper.getAutoIncrement(student);

            // 提交
            sqlSession.commit();

            // 关闭资源
            sqlSession.close();

            // 展示学生的id，即为自增长的主键
            System.out.println(student.getId());
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

>【注意】默认关闭自动提交，需要手动 commit

### MyBatis 工具类【`重点`】

> * Resource：用于获得读取配置文件的IO对象，耗费资源，建议通过IO一次性读取所有所需要的数据。
> * SqlSessionFactory：SqlSession工厂类，内存占用多，耗费资源，建议每个应用只创建一个对象。
> * SqlSession：相当于Connection，可控制事务，应为线程私有，不被多线程共享。
>* 将获得连接、关闭连接、提交事务、回滚事务、获得接口实现类等方法进行封装。

封装工具类

```java
/**
 * MyBatis工具类
 * 
 * 封装了获取连接，关闭连接，获取接口实现类对象的方法
 */
public class MyBatisUtils {
    // 准备SqlSession工厂对象
    private static SqlSessionFactory factory;

    // 创建ThreadLocal绑定当前线程中的SqlSession对象
    private static final ThreadLocal<SqlSession> threadLocal = new ThreadLocal<>();

    // 通过静态代码块加载SqlSession工厂对象
    static {
        try {
            // 读取配置文件
            InputStream resource = Resources.getResourceAsStream("mybatis-config.xml");

            // 获取工厂对象
            factory = new SqlSessionFactoryBuilder().build(resource);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    // 获得连接（从ThreadLocal中获得当前线程SqlSession）
    private static SqlSession openSession() {
        // 从ThreadLocal中获取连接
        SqlSession sqlSession = threadLocal.get();

        // 如果连接为null
        if (sqlSession == null) {

            // 从工厂对象中获取连接
            sqlSession = factory.openSession();

            // 将连接存入ThreadLocal
            threadLocal.set(sqlSession);
        }

        return sqlSession;
    }

    // 释放连接（释放当前线程中的SqlSession）
    private static void closeSession() {
        // 从ThreadLocal中获取连接
        SqlSession sqlSession = threadLocal.get();

        // 关闭连接
        sqlSession.close();

        // 从ThreadLocal中移除连接
        threadLocal.remove();
    }

    // 提交事务（提交当前线程中的SqlSession所管理的事务）
    public static void commit() {
        // 获取连接
        SqlSession session = openSession();

        // 提交事务
        session.commit();

        // 关闭资源
        closeSession();
    }

    // 回滚事务（回滚当前线程中的SqlSession所管理的事务）
    public static void rollback() {
        // 获取连接
        SqlSession session = openSession();

        // 回滚事务
        session.rollback();

        // 关闭资源
        closeSession();
    }

    // 获取接口实现类对象
    public static <T> T getMapper(Class<T> clazz) {
        // 获取连接
        SqlSession session = openSession();

        // 获取接口代理对象
        return session.getMapper(clazz);
    }
}
```

>【注意】这个代码以后复制粘贴直接用

测试工具类案例代码

```java
public class MyBatisUtilsTest {
    @Test
    public void testSelectAllStudent() {
        try {
            // 通过工具类获取接口实现类对象
            StudentDao mapper = MyBatisUtils.getMapper(StudentDao.class);

            // 执行对应的方法
            List<Student> students = mapper.selectAllStudents();

            // 提交
            MyBatisUtils.commit();

            // 增强for循环遍历
            for (Student student : students) {
                System.out.println(student);
            }
        } catch (Exception e) {
            // 回滚
            MyBatisUtils.rollback();
            e.printStackTrace();
        }

    }
}
```

### ORM 映射【重点】

#### MyBatis 自动 ORM 失效问题

> MyBatis 只能自动维护库表”列名“与”属性名“相同时的一一对应关系，二者不同时，无法自动ORM。

|       自动ORM失效        |
| :----------------------: |
| ![007](Pictures/007.png) |

创建表

```mysql
create table t_managers(mgr_id int primary key auto_increment, mgr_name varchar(20) unique not null, mgr_pwd varchar(16) not null);
```

创建实体类

```java
public class Manager {
    private int id;
    private String name;
    private String pwd;
    
    // Constructor、Setter、Getter
}
```

测试类案例代码

```java
public class ManagerTest {
    @Test
    public void testSelectAllManagers() {
        try {
            // 获取接口实现类对象
            ManagerDao mapper = MyBatisUtils.getMapper(ManagerDao.class);

            // 执行对应的方法
            List<Manager> list = mapper.selectAllManagers();

            // 提交
            MyBatisUtils.commit();

            // 增强for循环遍历集合
            for (Manager manager : list) {
                System.out.println(manager);
            }
        } catch (Exception e) {
            // 回滚
            MyBatisUtils.rollback();
        }
    }
}
```

#### 解决方案一：给列起别名【了解】

> 在 SQL 中使用 as 为查询字段添加列别名，以匹配属性名。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.fc.map.ManagerDao">
    <select id="selectAllManagers" resultType="com.fc.bean.Manager">
        select mgr_id as id, mgr_name as name, mgr_pwd as password from t_managers
    </select>
</mapper>
```

#### 解决方案二：结果映射（ResultMap - 查询结果的封装规则）

> 通过< resultMap id="" type="" >映射，匹配列名与属性名。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.fc.map.ManagerDao">
    <!--声明对应的resultMap，id对应映射的内容，type对应实体类-->
    <resultMap id="selectAllManagersMap" type="com.fc.bean.Manager">
        <!--将主键和成员变量相关联-->
        <id property="id" column="mgr_id"/>
        <!--将列名和成员变量相关联-->
        <result property="name" column="mgr_name"/>
        <result property="password" column="mgr_pwd"/>
    </resultMap>

    <!--使用resultMap作为ORM映射依据-->
    <select id="selectAllManagers" resultMap="selectAllManagersMap">
        select * from t_managers
    </select>
</mapper>
```

#### 解决方案三：驼峰映射（MyBatis - Settings）

> MyBatis 支持属性使用驼峰的命名

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<!--MyBatis配置-->
<configuration>

    <!--配置MyBatis运行状态的设置-->
    <settings>
        <!--开启将下划线映射到驼峰命名的规则规则-->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>
</configuration>
```

### MyBatis 处理关联关系-多表连接【重点】

> 实体间的关系：关联关系（拥有 has、属于 belong）
>
>  * OneToOne：一对一关系（Passenger--- Passport）
>
> * OneToMany：一对多关系（Employee --- Department）
>
> * ManyToMany：多对多关系（Student --- Subject）

创建数据表

部门表

```mysql
CREATE TABLE department (
      dept_id int PRIMARY KEY AUTO_INCREMENT,
      dept_name varchar(20) NOT NULL UNIQUE
)
```

员工表

```mysql
CREATE TABLE employee (
      emp_id int PRIMARY KEY AUTO_INCREMENT,
      emp_name varchar(20) NOT NULL,
      dept_id int NOT NULL,
      CONSTRAINT fk_dept_emp FOREIGN KEY (dept_id) REFERENCES department (dept_id)
)
```

声明实体类

员工类

```java
public class Employee {
    private int id;
    private String name;
    // 一个员工对应一个部门
    private Department department;
    
    // Constructor、Setter、Getter
}
```

部门类

```java
public class Department {
    private int id;
    private String name;
    // 一个部门中有多个员工
    private List<Employee> employeeList;
    
    // Constructor、Setter、Getter
}
```

#### 一对一

>注意：指定“一方”关系时（对象），使用< association javaType="" >

声明 mapper.xml 配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.fc.one2one.EmployeeDao">
    <!--声明结果映射的名称以及对应的实体类类型-->
    <resultMap id="selectAllEmployeeMap" type="com.fc.bean.Employee">
        <!--主键映射-->
        <id property="id" column="emp_id"/>
        <!--字段映射-->
        <result property="name" column="emp_name"/>
        <!--指定对象的封装规则,包含字段名以及对应的实体类类型-->
        <association property="department" javaType="com.fc.bean.Department">
            <!--主键映射-->
            <id property="id" column="dept_id"/>
            <!--字段映射-->
            <result property="name" column="dept_name"/>
        </association>
    </resultMap>

    <!--声明结果映射规则-->
    <select id="selectAllEmployee" resultMap="selectAllEmployeeMap">
        # 连表查询
        SELECT
            employee.emp_id,
            employee.emp_name,
            department.dept_id,
            department.dept_name
        FROM
            employee
                INNER JOIN department
                           ON employee.dept_id = department.dept_id
    </select>
</mapper>
```

注册到 MyBatis 配置文件中

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<!--MyBatis配置-->
<configuration>
    ...
    <mappers>
        <mapper resource="EmployeeMapper.xml"/>
    </mappers>
</configuration>
```

测试案例代码

```java
public class EmployeeTest {
    @Test
    public void testSelectAllEmployee() {
        // 获取接口实现类对象
        EmployeeDao mapper = MyBatisUtils.getMapper(EmployeeDao.class);

        // 执行查询方法获取List集合
        List<Employee> list = mapper.selectAllEmployee();

        // 提交
        MyBatisUtils.commit();

        // 增强for循环遍历
        for (Employee employee : list) {
            System.out.println(employee);
        }
    }
}
```

#### 一对多

>注意：注意：指定“多方”关系时（集合），使用< collection ofType="" >

1、声明 mapper.xml 配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.fc.one2many.DepartmentDao">
    <!--声明结果映射的名称以及对应的实体类类型-->
    <resultMap id="selectDepartmentByEmployeeIdMap" type="com.fc.bean.Department">
        <!--主键映射-->
        <id property="id" column="dept_id"/>
        <!--字段映射-->
        <result property="name" column="dept_name"/>

        <!--指定集合的封装规则,包含字段名以及对应泛型的实体类类型-->
        <collection property="employeeList" javaType="List" ofType="com.fc.bean.Employee">
            <!--主键映射-->
            <id property="id" column="emp_id"/>
            <!--字段映射-->
            <result property="name" column="emp_name"/>
        </collection>
    </resultMap>

    <!--声明结果映射规则-->
    <select id="selectDepartmentByEmployeeId" parameterType="Integer" resultMap="selectDepartmentByEmployeeIdMap">
        # 连表查询
        SELECT
            dept.dept_id,
            dept.dept_name,
            emp.emp_id,
            emp.emp_name
        FROM
            department dept
                INNER JOIN employee emp ON emp.dept_id = dept.dept_id
        WHERE
            dept.dept_id = #{id};
    </select>
</mapper>
```

【注意】也可以通过嵌套结果映射的方式来实现

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.fc.one2many.DepartmentDao">
    <!--员工信息映射-->
    <resultMap id="selectEmployeeMap" type="com.fc.bean.Employee">
        <!--主键映射-->
        <id property="id" column="emp_id"/>
        <!--字段映射-->
        <result property="name" column="emp_name"/>
    </resultMap>

    <resultMap id="selectDepartmentByEmployeeIdMap" type="com.fc.bean.Department">
        <!--主键映射-->
        <id property="id" column="dept_id"/>
        <!--字段映射-->
        <result property="name" column="dept_name"/>

        <!--通过指定的封装规则进行映射-->
        <collection property="employeeList" javaType="List" resultMap="selectEmployeeMap"/>
    </resultMap>

    <!--声明结果映射规则-->
    <select id="selectDepartmentByEmployeeId" parameterType="Integer" resultMap="selectDepartmentByEmployeeIdMap">
        # 连表查询
        SELECT
            dept.dept_id,
            dept.dept_name,
            emp.emp_id,
            emp.emp_name
        FROM
            department dept
                INNER JOIN employee emp ON emp.dept_id = dept.dept_id
        WHERE
            dept.dept_id = #{id};
    </select>
</mapper>
```

2、注册到 MyBatis 配置文件中

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<!--MyBatis配置-->
<configuration>
    ...
    <mappers>
        <mapper resource="DepartmentMapper.xml"/>
    </mappers>
</configuration>
```

3、测试案例代码

```java
public class DepartmentTest {
    @Test
    public void testSelectDepartmentByEmployeeId() {
        // 获取接口实现类对象
        DepartmentDao mapper = MyBatisUtils.getMapper(DepartmentDao.class);

        // 执行对应的方法
        Department department = mapper.selectDepartmentByEmployeeId(1);

        // 提交
        MyBatisUtils.commit();
        
        System.out.println(department);
    }
}
```

### 动态 SQL【重点】

> MyBatis 的映射文件中支持在基础 SQL 上添加一些逻辑操作，并动态拼接成完整的 SQL 之后再执行，以达到 SQL 复用、简化编程的效果。
>
> 动态SQL元素和使用JSTL或其他相似的基于XML的文本处理器相似。

#### < sql >

> 这个元素可以被用来定义可重用的 SQL 代码段，可以包含在其他语句中。

声明 dao 层接口

```java
public interface StudentDao {
    // 查询所有学生
    List<Student> selectAllStudent();
}
```

声明 mapper.xml 配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.fc.dynamicSql.StudentDao">
    <!--定义SQL片段-->
    <sql id="Student_Field">
        select id, name, age, gender, birthday, info
    </sql>
    
    <!--通过id引用SQL片段进行拼接-->
    <select id="selectAllStudent" resultType="Student">
        <include refid="Student_Field"/>
        from student
    </select>
</mapper>
```

测试案例代码

```java
public class DynamicSqlTest {
    @Test
    public void testSelectStudents() {
        StudentDao mapper = MyBatisUtils.getMapper(StudentDao.class);

        List<Student> students = mapper.selectAllStudent();

        MyBatisUtils.commit();

        for (Student student : students) {
            System.out.println(student);
        }
    }
}
```

#### < if >

>此元素标签用于进行条件判断，如果符合要求则执行对应的内容

声明 dao 层接口

```java
public interface StudentDao {
    // 模糊查询
    List<Student> selectStudentByForward(@Param("name") String name, @Param("age") String age);
}
```

声明 mapper.xml 配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.fc.dynamicSql.StudentDao">
    <!--定义SQL片段-->
    <sql id="Student_Field">
        select id, name, age, gender, birthday, info
    </sql>
    
    <!--使用if进行判断，如果不为空则添加对应条件-->
    <select id="selectStudentByForward" resultType="Student">
        <include refid="Student_Field"/>
        from student
        <if test="name != null">
            where name like #{name}
        </if>

        <if test="age != null">
            and age like #{age}
        </if>
    </select>
</mapper>
```

测试案例代码

```java
public class DynamicSqlTest {
    @Test
    public void testSelectStudentByForward() {
        StudentDao mapper = MyBatisUtils.getMapper(StudentDao.class);

        List<Student> students = mapper.selectStudentByForward("%张%", "%2%");

        MyBatisUtils.commit();

        for (Student student : students) {
            System.out.println(student);
        }
    }
}
```

#### < where >

>【注意】会添加where关键字，并自动忽略前后缀（如：and | or）

声明 dao 层接口

```java
public interface StudentDao {
    // 根据条件查询学生
    List<Student> selectStudentByStudent(Student student);
}
```

声明 mapper.xml 配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.fc.dynamicSql.StudentDao">
    <!--定义SQL片段-->
    <sql id="Student_Field">
        select id, name, age, gender, birthday, info
    </sql>
    
    <!--Where可以自动去除前后缀（and | or）-->
    <select id="selectStudentByStudent" resultType="Student">
        <include refid="Student_Field"/>
        from student
        <where>
            <if test="id != null">
                and id = #{id}
            </if>

            <if test="name != null">
                and name = #{name}
            </if>

            <if test="age != null">
                and age = #{age}
            </if>

            <if test="gender != null">
                and gender = #{gender}
            </if>

            <if test="info != null">
                and info = #{info}
            </if>
        </where>
    </select>
</mapper>
```

测试案例代码

```java
public class DynamicSqlTest {
    @Test
    public void testSelectStudentByStudent() {
        StudentDao mapper = MyBatisUtils.getMapper(StudentDao.class);

        Student student = new Student();
        student.setId(1);
        student.setName("张三");

        List<Student> students = mapper.selectStudentByStudent(student);

        MyBatisUtils.commit();

        for (Student student1 : students) {
            System.out.println(student1);
        }
    }
}
```

#### < set >

>set元素会动态前置SET关键字，而且也会消除任意无关的逗号

声明 dao 层接口

```java
public interface StudentDao {
    // 修改学生
    int updateStudent(Student student);
}
```

声明 mapper.xml 配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.fc.dynamicSql.StudentDao">
    <!--set可以自动去除前后符号（,）-->
    <update id="updateStudent" parameterType="Student">
        update student
        <set>
            <if test="name != null">
                name = #{name},
            </if>
            <if test="age != null">
                age = #{age},
            </if>

            <if test="gender != null">
                gender = #{gender},
            </if>

            <if test="birthday != null">
                birthday = #{birthday},
            </if>

            <if test="info != null">
                info = #{info},
            </if>
        </set>
        where id = #{id}
    </update>
</mapper>
```

测试案例代码

```java
public class DynamicSqlTest {
    // 测试set元素标签
    @Test
    public void testUpdateStudent() {
        try {
            StudentDao mapper = MyBatisUtils.getMapper(StudentDao.class);

            Student student = new Student();
            student.setId(1);
            student.setBirthday(new Date());
            student.setInfo("越活越年轻");

            int affectedRows = mapper.updateStudent(student);

            MyBatisUtils.commit();

            System.out.println("受影响的行数：" + affectedRows);
        } catch (Exception e) {
            MyBatisUtils.rollback();
        }
    }
}
```

#### < trim >

> < trim prefix="where" prefixOverrides="and | or" >代替< where > 
>
> < trim suffix="set" suffixOverrides="," >代替< set >

案例代码一

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.fc.dynamicSql.StudentDao">
    <!--定义SQL片段-->
    <sql id="Student_Field">
        select id, name, age, gender, birthday, info
    </sql>
    
    <!--相当于where，自动增加前缀忽略后缀-->
    <select id="selectStudentByStudent" resultType="Student">
        <include refid="Student_Field"/>
        from student
        <trim prefix="where" prefixOverrides="and | or">
            <if test="id != null">
                and id = #{id}
            </if>

            <if test="name != null">
                and name = #{name}
            </if>

            <if test="age != null">
                and age = #{age}
            </if>

            <if test="gender != null">
                and gender = #{gender}
            </if>

            <if test="info != null">
                and info = #{info}
            </if>
        </trim>
    </select>
</mapper>
```

案例代码二

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.fc.dynamicSql.StudentDao">
    <!--相当于set，自动增加前缀，忽略后缀-->
    <update id="updateStudent" parameterType="Student">
        update student
        <trim prefix="set" suffixOverrides=",">
            <if test="name != null">
                name = #{name},
            </if>
            <if test="age != null">
                age = #{age},
            </if>

            <if test="gender != null">
                gender = #{gender},
            </if>

            <if test="birthday != null">
                birthday = #{birthday},
            </if>

            <if test="info != null">
                info = #{info},
            </if>
        </trim>
        where id = #{id}
    </update>
</mapper>
```

#### < foreach >

>foreach 用于迭代一个集合获取数组，通常是构建在IN条件中

参数说明

| 参数       | 描述     | 取值                                          |
| ---------- | -------- | --------------------------------------------- |
| collection | 容器类型 | list、array、map                              |
| open       | 起始符   | (                                             |
| close      | 结束符   | )                                             |
| separator  | 分隔符   | ,                                             |
| index      | 下标号   | 从0开始，依次递增                             |
| item       | 当前项   | 任意名称（循环中通过 #{任意名称} 表达式访问） |

声明 dao 层接口

```java
public interface StudentDao {
    // 根据多个id删除对应的学生
    int deleteManyStudent(Integer... ids);
}
```

声明 mapper.xml 配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.fc.dynamicSql.StudentDao">
    <!--foreach用于迭代一个集合获取数组，通常是构建在IN条件中-->
    <delete id="deleteManyStudent">
        delete from student where id in
        <foreach collection="array" separator="," open="(" close=")" item="id" index="i">
            #{id}
        </foreach>
    </delete>
</mapper>
```

测试案例代码

```java
public class DynamicSqlTest {
    // 测试foreach元素标签
    @Test
    public void testDeleteManyStudent() {
        try {
            StudentDao mapper = MyBatisUtils.getMapper(StudentDao.class);

            int affectedRows = mapper.deleteManyStudent(11, 12, 13);

            MyBatisUtils.commit();

            System.out.println("受影响的行数：" + affectedRows);
        } catch (Exception e) {
            MyBatisUtils.rollback();
        }
    }
}
```

### 缓存（Cache）【重点】

> 内存中的一块存储空间，服务于某个应用程序，旨在将频繁读取的数据临时保存在内存中，便于二次快速访问。

| 无缓存：用户在访问相同数据时，需要发起多次对数据库的直接访问，导致产生大量IO、读写硬盘的操作，效率低下 |
| :----------------------------------------------------------: |
|                   ![012](Pictures/012.png)                   |

| 有缓存：首次访问时，查询数据库，将数据存储到缓存中；再次访问时，直接访问缓存，减少IO、硬盘读写次数、提高效率 |
| :----------------------------------------------------------: |
|                   ![013](Pictures/013.png)                   |

#### 一级缓存

SqlSession 级别的缓存，同一个 SqlSession 的发起多次同构查询，会将数据保存在一级缓存中。

> 注意：无需任何配置，默认开启一级缓存。

#### 二级缓存

SqlSessionFactory 级别的缓存，同一个 SqlSessionFactory 构建的 SqlSession 发起的多次同构查询，会将数据保存在二级缓存中。

> 注意：在 sqlSession.commit() 或者 sqlSession.close() 之后生效。

##### 开启全局缓存

> 【注意】< settings >是MyBatis中极为重要的调整设置，他们会改变MyBatis的运行行为，其他详细配置可参考官方文档。

```xml
<configuration>
	<properties .../>
  	
  	<!-- 注意书写位置，必须放在properties之后，typeAliases之前 -->
    <settings>
        <!-- mybaits-config.xml中开启全局缓存（默认开启） -->
        <setting name="cacheEnabled" value="true"/> 
    </settings>
  
  	<typeAliases></typeAliases>
</configuration>
```

##### 指定 Mapper 开启缓存

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.fc.dynamicSql.StudentDao">
    <!--开启二级缓存-->
    <cache/>
    <!--定义SQL片段-->
    <sql id="Student_Field">
        select id, name, age, gender, birthday, info
    </sql>

    <!--通过id引用SQL片段进行拼接-->
    <select id="selectAllStudent" resultType="Student">
        <include refid="Student_Field"/>
        from student
    </select>
</mapper>
```

##### 实体类实现序列化接口

```java
@Data
public class Student implements Serializable {
    private Integer id;
    private String name;
    private Byte age;
    private String gender;
    private Date birthday;
    private String info;
}
```

##### 测试案例代码

```java
// 测试缓存
@Test
public void testCache() {
    StudentDao mapper = MyBatisUtils.getMapper(StudentDao.class);

    List<Student> students = mapper.selectAllStudent();

    // 必须关闭SqlSession才能缓存数据
    MyBatisUtils.commit();

    for (Student student : students) {
        System.out.println(student);
    }

    StudentDao mapper1 = MyBatisUtils.getMapper(StudentDao.class);

    List<Student> list = mapper1.selectAllStudent();

    MyBatisUtils.commit();

    // 缓存击中
    for (Student student : list) {
        System.out.println(student);
    }
}
```

>【注意】
>
>1、缓存要求实体类必须实现序列化
>
>2、如果对缓存的数据进行了增删改操作，缓存会失效，下次操作会重新缓存

### 使用 PageHelper 分页插件【重点】

#### PageHelper 概述

PageHelper 是适用于 MyBatis 框架的一个分页插件，使用方式极为便捷，支持任何复杂的单表、多表分页查询操作。

>官方网站：https://pagehelper.github.io/
>
>下载地址：https://github.com/pagehelper/Mybatis-PageHelper
>
>API：https://apidoc.gitee.com/free/Mybatis_PageHelper/

#### 案例代码

>* 只有在PageHelper.startPage()方法之后的[第一个查询会有执行分页]()。
>* 分页插件[不支持带有“for update”]()的查询语句。
>* 分页插件不支持[“嵌套查询”]()，由于嵌套结果方式会导致结果集被折叠，所以无法保证分页结果数量正确。

1、pom.xml 中导入依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.fc</groupId>
    <artifactId>02_MyBatis_05_PageHelper</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

    <dependencies>
        <!--mybatis-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.6</version>
        </dependency>

        <!--数据库连接-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.48</version>
        </dependency>

        <!--分页插件-->
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper</artifactId>
            <version>5.2.0</version>
        </dependency>

        <!--单元测试-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13</version>
        </dependency>
    </dependencies>
</project>
```

2、mybatis-config.xml 中引入 pageHelper 插件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<!--MyBatis配置-->
<configuration>
    
    <plugins>
        <!--pageHelper分页插件-->
        <plugin interceptor="com.github.pagehelper.PageInterceptor">
            <!--自动检测当前的数据库链接-->
            <property name="helperDialect" value="mysql"/>

            <!--分页合理化参数，小于0页显示第一页，大于最后一页显示最后一页-->
            <property name="reasonable" value="true"/>

            <!--通过 Mapper 接口参数来传递分页参数-->
            <property name="supportMethodsArguments" value="true"/>
        </plugin>
    </plugins>
    
    <!--JDBC环境配置、选中默认环境-->
    <environments default="development">
        <!--MySql数据库环境配置-->
        <environment id="development">
            <!--事务管理-->
            <transactionManager type="JDBC"/>
            <!--连接池相关配置-->
            <dataSource type="org.apache.ibatis.datasource.pooled.PooledDataSourceFactory">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/FC2021?useUnicode=true&amp;characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
    <!--Mapper注册-->
    <mappers>
        <!--注册Mapper文件的所在位置-->
        <mapper resource="StudentMapper.xml"/>
    </mappers>
</configuration>
```

3、声明 dao 层接口

```java
public interface StudentDao {
    // 查询所有学生
    List<Student> findAll();
}
```

4、编写对应的 mapper.xml 映射文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace对应所需实现的接口全限定名-->
<mapper namespace="com.fc.dao.StudentDao">
    <!--查询所有学生-->
    <select id="findAll" resultType="com.fc.bean.Student">
        select * from student
    </select>
</mapper>
```

5、测试类

```java
public class PageTest {
    // 测试分页
    @Test
    public void testPage() {
        // 通过工具类获取接口的实现类
        StudentDao mapper = MyBatisUtils.getMapper(StudentDao.class);

        // 查询指定页中的多少条数据
        PageHelper.startPage(1, 2);

        // 执行sql语句
        List<Student> list = mapper.findAll();

        // 遍历结果
        for (Student student : list) {
            System.out.println(student);
        }

        // 提交事务
        MyBatisUtils.commit();
    }
}
```

#### PageInfo

##### 概述

对分页结果进行包装

![](Pictures/PageInfo.png)

##### 案例代码

```java
public class PageTest {
    // 测试PageInfo
    @Test
    public void testPageInfo() {
        // 通过工具类获取接口的实现类
        StudentDao mapper = MyBatisUtils.getMapper(StudentDao.class);

        // 查询指定页中的多少条数据
        PageHelper.startPage(1, 2);

        // 执行sql语句
        List<Student> list = mapper.findAll();

        // 将分页结果包装到PageInfo对象中
        PageInfo<Student> pageInfo = new PageInfo<>(list);

        System.out.println(pageInfo);

        // 总页数
        System.out.println("总页数：" + pageInfo.getPages());

        // 当前页
        System.out.println("当前页：" + pageInfo.getPageNum());

        // 每页的数量
        System.out.println("每页的数量：" + pageInfo.getPageSize());

        // 当前页的数量
        System.out.println("当前页的数量：" + pageInfo.getSize());

        // 总数据量
        System.out.println("总数据量：" + pageInfo.getTotal());

        // 从pageInfo中获取查询数据
        List<Student> students = pageInfo.getList();

        for (Student student : students) {
            System.out.println(student);
        }
        // 提交事务
        MyBatisUtils.commit();
    }
}
```

#### 分页插件参数【了解】

| 参数                        | 描述                                                         |
| --------------------------- | ------------------------------------------------------------ |
| **helperDialect**           | 分页插件会自动检测当前的数据库链接，自动选择合适的分页方式。 你可以配置`helperDialect`属性来指定分页插件使用哪种方言。配置时，可以使用下面的缩写值：<br/> `oracle`,`mysql`,`mariadb`,`sqlite`,`hsqldb`,`postgresql`,`db2`,`sqlserver`,`informix`,`h2`,`sqlserver2012`,`derby` |
| offsetAsPageNum             | 默认值为 `false`，该参数对使用 `RowBounds` 作为分页参数时有效。 当该参数设置为 `true` 时，会将 `RowBounds` 中的 `offset` 参数当成 `pageNum` 使用，可以用页码和页面大小两个参数进行分页。 |
| rowBoundsWithCount          | 默认值为`false`，该参数对使用 `RowBounds` 作为分页参数时有效。 当该参数设置为`true`时，使用 `RowBounds` 分页会进行 count 查询。 |
| pageSizeZero                | 默认值为 `false`，当该参数设置为 `true` 时，如果 `pageSize=0` 或者 `RowBounds.limit = 0` 就会查询出全部的结果（相当于没有执行分页查询，但是返回结果仍然是 `Page` 类型）。 |
| **reasonable**              | 分页合理化参数，默认值为`false`。当该参数设置为 `true` 时，`pageNum<=0` 时会查询第一页， `pageNum>pages`（超过总数时），会查询最后一页。默认`false` 时，直接根据参数进行查询。 |
| params                      | 了支持`startPage(Object params)`方法，增加了该参数来配置参数映射，用于从对象中根据属性名取值， 可以配置 `pageNum,pageSize,count,pageSizeZero,reasonable` |
| **supportMethodsArguments** | 支持通过 Mapper 接口参数来传递分页参数，默认值`false`，分页插件会从查询方法的参数值中，自动根据上面 `params` 配置的字段中取值，查找到合适的值时就会自动分页。 |
| autoRuntimeDialect          | 默认值为 `false`。设置为 `true` 时，允许在运行时根据多数据源自动识别对应方言的分页 （不支持自动选择`sqlserver2012`，只能使用`sqlserver`） |
| closeConn                   | 默认值为 `true`。当使用运行时动态数据源或没有设置 `helperDialect` 属性自动获取数据库类型时，会自动获取一个数据库连接， 通过该属性来设置是否关闭获取的这个连接，默认`true`关闭，设置为 `false` 后，不会关闭获取的连接 |

### 基于注解使用 MyBatis 【了解】

通过在接口中直接添加MyBatis注解，完成CRUD。

> - 注意：接口注解定义完毕后，需将接口全限定名注册到mybatis-config.xml的< mappers >中。
> - 经验：注解模式属于硬编码到.java文件中，失去了使用配置文件外部修改的优势，可结合需求选用。

#### 案例代码

1、声明 dao 层接口

```java
public interface StudentDao {
    /**
     * 查询所有学生
     * 使用@Select注解，value为查询SQL语句
     * 
     * @return 返回一个包含所有查询结果对应的实体类的集合
     */
    @Select("select * from student")
    List<Student> selectAllStudents();
}
```

2、配置 mybatis-config.xml 配置文件，声明映射的位置

```xml
<mappers>
    <mapper class="com.fc.dao.StudentDao"/>
</mappers>
```

3、测试案例代码

```java
public class StudentTest {
    @Test
    public void testAnnotation() {
        try {
            // 读取配置文件
            InputStream resource = Resources.getResourceAsStream("mybatis-config.xml");

            // 获取SqlSession工厂对象
            SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(resource);

            // 获取SqlSession对象
            SqlSession sqlSession = factory.openSession();

            // 通过SqlSession获取接口的代理对象
            StudentDao mapper = sqlSession.getMapper(StudentDao.class);

            // 通过代理对象调用查询方法操作数据库并获取实体类对象
            List<Student> list = mapper.selectAllStudents();

            // 关闭资源
            sqlSession.close();

            // 增强for循环
            for (Student student : list) {
                System.out.println(student);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### 相关注解

| 注解            | 描述                                                         |
| --------------- | ------------------------------------------------------------ |
| @Insert         | 执行插入操作                                                 |
| @Delete         | 执行删除操作                                                 |
| @Update         | 执行更新操作                                                 |
| @Select         | 执行查询操作                                                 |
| @Results        | 修饰返回的结果集，用于关联实体类属性和数据库字段。如果实体类属性和数据库属性名保持一致，就不需要这个属性来修饰。 |
| @Result         | 数据表字段和属性之间的单独结果映射                           |
| @Param          | 如果你的映射器的方法需要多个参数，这个注解可以被应用于映射器的方法参数来给每个参数一个名字 |
| @SelectKey      | 用于返回新插入数据的id值。<br />statement：获取新插入记录主键值的SQL语句；<br/>keyProperty：获取的该主键值返回后初始化对象的哪个属性；<br/>resultType：返回值类型；<br/>before：指定主键的生成相对于insert语句的执行先后顺序，该属性不能够省略； |
| @One            | 一的关系的嵌套查询                                           |
| @Many           | 多的关系的嵌套查询                                           |
| @InsertProvider | 实现插入动态sql                                              |
| @DeleteProvider | 实现删除动态sql                                              |
| @UpdateProvider | 实现修改动态sql                                              |
| @SelectProvider | 实现查询动态sql                                              |

### MyBatis 处理关联关系-嵌套查询【了解】

> 思路：查询部门信息时，及联查询所属的员工信息。
>
> * DepartmentDao接口中定义selectDepartmentById，并实现Mapper。
> * EmployeeDao接口中定义selectEmployeesByDeptId，并实现Mapper，
> * 当selectDepartmentById被执行时，通过< collection >调用selectEmployeesByDeptId方法，并传入条件参数。

#### 主表查询

> 定义selectEmployeesByDeptId，并书写Mapper，实现根据部门ID查询员工信息

```java
public interface EmployeeDao {
    /**
     * 根据部门编号查询员工信息
     * @param id 部门编号
     * @return 该部门中的所有员工
     */
    List<Employee> findByDeptId(Integer id);
}
```

```xml
<mapper namespace="com.qf.mybatis.part2.one2many.EmployeeDao">
    <!-- 根据部门编号查询所有员工 -->
    <select id="findByDeptId" resultType="com.fc.bean.Employee">
        select * from employee where d_id = #{id}
    </select>
</mapper>
```

#### 及联调用

> ##### 定义selectDepartmentById，并书写Mapper，实现根据部门ID查询部门信息，并及联查询该部门员工信息

```java
public interface DepartmentDao {
    /**
     * 根据部门名称查询对应的部门
     * 
     * @param name 部门名称
     * @return 对应的部门
     */
    Department findById(String name);
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.fc.dao.DepartmentDao">
    <resultMap id="departmentMap" type="com.fc.bean.Department">
        <id column="d_id" property="dId"/>
        <result column="d_name" property="dName"/>

        <!--对集合的映射规则，通过select传递到指定dao接口中的方法完成映射，column为传递的参数-->
        <collection property="employees" javaType="list" ofType="com.fc.bean.Employee" column="d_id" select="com.fc.dao.EmployeeDao.findByDeptId"/>
    </resultMap>

    <select id="findById" resultMap="departmentMap">
        select * from department where d_name = #{name}
    </select>
</mapper>
```

### 懒加载【了解】

也称为延时加载，刚开始查询的时候，不会查询出关联的数据，当使用这些数据时，再进行查询。

mybatis-config.xml 中开启延迟加载

```xml
<settings>
    <!-- 启用懒加载，默认为false -->
	<setting name="lazyLoadingEnabled" value="true"/>
	<!-- 将积极加载改为消极加载即按需加载。必须写,且为false才会懒加载 -->  
	<setting name="aggressiveLazyLoading" value="false"/>
</settings>
```

* [注意：开启延迟加载后，如果不使用及联数据，则不会触发及联查询操作，有利于加快查询速度、节省内存资源。]()

### mybatis-config.xml 配置文件标签顺序【了解】

properties --> settings --> typeAliases --> typeHandlers --> objectFactory --> objectWrapperFactory --> reflectorFactory --> plugins --> environments --> databaseIdProvider --> mappers

### MyBatis 逆向生成【了解】

>官方文档：http://mybatis.org/generator/

1、pom.xml 依赖：

```xml
<dependencies>
    <!--mysql-->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.41</version>
    </dependency>

    <!--mybatis-->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.4.5</version>
    </dependency>

    <!--逆向工程生成库-->
    <dependency>
        <groupId>org.mybatis.generator</groupId>
        <artifactId>mybatis-generator-core</artifactId>
        <version>1.4.0</version>
    </dependency>
</dependencies>
```

2、创建配置文件 generatorConfig.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <context id="testTables" targetRuntime="MyBatis3">
        <commentGenerator>
            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
            <property name="suppressAllComments" value="true" />
        </commentGenerator>
        <!--数据库连接的信息：驱动类、连接地址、用户名、密码 -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/FC2021?useSSL=false&amp;characterEncoding=utf8&amp;serverTimezone=Asia/Shanghai" 
                        userId="root"
                        password="root">
        </jdbcConnection>
        <!-- <jdbcConnection driverClass="oracle.jdbc.OracleDriver"
            connectionURL="jdbc:oracle:thin:@127.0.0.1:1521:FC2021"
            userId="root"
            password="root">
        </jdbcConnection> -->

        <!-- 默认false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer，为 true时把JDBC DECIMAL 和
            NUMERIC 类型解析为java.math.BigDecimal -->
        <javaTypeResolver>
            <property name="forceBigDecimals" value="false" />
        </javaTypeResolver>

        <!-- targetProject:生成PO类的位置 -->
        <javaModelGenerator targetPackage="com.fc.pojo"
                            targetProject=".\src\main\java">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
            <!-- 从数据库返回的值被清理前后的空格 -->
            <property name="trimStrings" value="true" />
        </javaModelGenerator>
        <!-- targetProject:mapper映射文件生成的位置 -->
        <sqlMapGenerator targetPackage="com.fc.mapper"
                         targetProject=".\src\main\resources">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
        </sqlMapGenerator>
        <!-- targetPackage：mapper接口生成的位置 -->
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="com.fc.dao"
                             targetProject=".\src\main\java">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
        </javaClientGenerator>
        <!-- 指定数据库表 -->
        <table schema="" tableName="person"/>
        <!--<table schema="" tableName="tb_sheet"></table>-->
        <!-- <table schema="" tableName="city_info"></table> -->

        <!-- 有些表的字段需要指定java类型
         <table schema="" tableName="">
            <columnOverride column="" javaType="" />
        </table> -->
    </context>
</generatorConfiguration>
```

3、GeneratorSqlMap.java

```java
import java.io.File;
import java.util.ArrayList;
import java.util.List;

import org.mybatis.generator.api.MyBatisGenerator;
import org.mybatis.generator.config.Configuration;
import org.mybatis.generator.config.xml.ConfigurationParser;
import org.mybatis.generator.internal.DefaultShellCallback;

/**
 * 逆向工程
 */
public class GeneratorSqlMap {

    private void generator() throws Exception {

        List<String> warnings = new ArrayList<>();
        boolean overWrite = true;
        //指定 逆向工程配置文件
        File configFile = new File("./src/main/resources/generatorConfig.xml");
        ConfigurationParser cp = new ConfigurationParser(warnings);
        Configuration config = cp.parseConfiguration(configFile);
        DefaultShellCallback callback = new DefaultShellCallback(overWrite);
        MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config,
                callback, warnings);
        myBatisGenerator.generate(null);

    }

    public static void main(String[] args) {
        try {
            GeneratorSqlMap generatorSqlmap = new GeneratorSqlMap();
            generatorSqlmap.generator();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
