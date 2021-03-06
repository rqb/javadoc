# Mybatis入门

## 什么是MyBatis

MyBatis 是一款优秀的持久层框架，它支持定制化 SQL、存储过程以及高级映射。MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。MyBatis 可以使用简单的 XML 或注解来配置和映射原生信息，将接口和 Java 的 POJOs(Plain Old Java Objects,普通的 Java对象)映射成数据库中的记录。 





Generator生成代码

1)引入mybatis-generator-maven-plugin

```xml
<dependency>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-maven-plugin</artifactId>
    <version>1.3.7</version>
</dependency>
```



2)配置generatorConfig.xml

```xml
<commentGenerator>
    <property name="suppressAllComments" value="true"></property>
</commentGenerator>
```



问题:

[ERROR] Failed to execute goal org.mybatis.generator:mybatis-generator-maven-plugin:1.3.7:generate (default-cli) on project mybatis-generator: Execution default-cli of goal org.mybatis.generator:mybatis-generator-maven-plugin:1.3.7:generate failed: Exception getting JDBC Driver: com.mysql.jdbc.Driver -> [Help 1]

问题产生原因有可能是：
1.xml配置文件中的location不正确
2.我这里的问题是，使用不正确的mysql-connector-java版本





## Mybatis中Mapper内置方法细解

selectByExample===>根据条件查询数据

```java
//下面是一个完整的案例
UserExample example = new UserExample();
Criteria criteria = example.createCriteria();
criteria.andUsernameEqualTo("joe");
criteria.andUsernameIsNull();
example.setOrderByClause("username asc,email desc");
List<?>list = userDAO.selectByExample(example);
相当于：select * from user where username = 'joe' and username is null order by username asc,email desc
 
//注：在iBator 生成的文件UserExample.java中包含一个static 的内部类 Criteria ，在Criteria中有很多方法，主要是定义SQL 语句where后的查询条件。
```

**insert===>插入数据** 

```java
int insert(Account record);
 
//下面是完整的案例
User user = new User();
 //user.setId(101);
 user.setUsername("test");
 user.setPassword("123456")
 user.setEmail("674531003@qq.com");
 userDAO.insert(user);
 //相当于：insert into user(ID,username,password,email)values(101,'test','123456','674531003@qq.com');
```



**insertSelective===>插入数据** 

```java

```

