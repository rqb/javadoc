# Spring Cloud预备知识

## 设计模式

### 发布/订阅模式



![](D:\技术文档\面试资料整理\git\文章\gif\20180131204721514.png)





### 事件/监听模式

java.util.EventObject

java.util.EventListener



## Spring事件/监听

org.springframework.context.ApplicationEvent:应用事件

org.springframework.context.ApplicationListener:应用监听器

```java
@FunctionalInterface
public interface ApplicationListener<E extends ApplicationEvent> extends EventListener {
	/**
	 * Handle an application event.
	 * @param event the event to respond to
	 */
	void onApplicationEvent(E event);
}
```



### 示例

```java
public class MyApplicationEvent extends ApplicationEvent {

    //自定义事件，可扩展。例如：可以方便传递上下文
    private final ApplicationContext applicationContext;

    public MyApplicationEvent(ApplicationContext applicationContext, Object source) {
        super(source);
        this.applicationContext=applicationContext;
    }

    public ApplicationContext getApplicationContext() {
        return applicationContext;
    }
}
```



```java
public class SpringEventListenerDemo {
    public static void main(String[] args) {
        //Annotation驱动Spring上下文
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext();
        //注册监听器
        context.addApplicationListener(new ApplicationListener<MyApplicationEvent>() {
            @Override
            public void onApplicationEvent(MyApplicationEvent event) {
                System.out.println("接收到事件：" + event.getSource()+" @ "+ event.getApplicationContext());
            }
        });
        context.refresh();
        //发布事件
        context.publishEvent(new MyApplicationEvent(context,"Hello Word!"));
        context.publishEvent(new MyApplicationEvent(context,"Hello RenQingBin!"));

    }
}
```

# Spring Cloud Config Client

## Spring Boot事件/监听

### ConfigFileApplicationListener

org.springframework.boot.context.config.ConfigFileApplicationListener

管理配置文件，比如：application.properties 以及 application.yaml

application-{profile}.properties:

*profile  = dev 、test*


application-{profile}.properties
application.properties


Spring Boot 在相对于 ClassPath ： /META-INF/spring.factories

Java SPI : java.util.ServiceLoader

*Spring SPI：*

Spring Boot "/META-INF/spring.factories"

```properties
# Application Listeners
org.springframework.context.ApplicationListener=\
org.springframework.boot.ClearCachesApplicationListener,\
org.springframework.boot.builder.ParentContextCloserApplicationListener,\
org.springframework.boot.context.FileEncodingApplicationListener,\
org.springframework.boot.context.config.AnsiOutputApplicationListener,\
org.springframework.boot.context.config.ConfigFileApplicationListener,\
org.springframework.boot.context.config.DelegatingApplicationListener,\
org.springframework.boot.context.logging.ClasspathLoggingApplicationListener,\
org.springframework.boot.context.logging.LoggingApplicationListener,\
org.springframework.boot.liquibase.LiquibaseServiceLocatorApplicationListener
```

如何控制顺序

实现Ordered 以及 标记@Order，在 Spring 里面，数值越小，越优先

## Spring Cloud事件/监听

### BootstrapApplicationListener

Spring Cloud "/META-INF/spring.factories":

```properties
# Application Listeners
org.springframework.context.ApplicationListener=\
org.springframework.cloud.bootstrap.BootstrapApplicationListener,\
org.springframework.cloud.bootstrap.LoggingSystemShutdownListener,\
org.springframework.cloud.context.restart.RestartListener
```



> BootstrapApplicationListener加载的优先级 高于 ConfigFileApplicationListener，所以 application.properties 文件即使定义也配置不到！
>
> 原因在于：
>
> BootstrapApplicationListener 第6优先
>
> ConfigFileApplicationListener 第11优先

证据详见代码：

```java
	ConfigFileApplicationListener.java 
	/**
	 * The default order for the processor.
	 */
	public static final int DEFAULT_ORDER = Ordered.HIGHEST_PRECEDENCE + 10;

    BootstrapApplicationListener.java
    public static final int DEFAULT_ORDER = Ordered.HIGHEST_PRECEDENCE + 5;
```

负责加载bootstrap.properties 或者 bootstrap.yaml

负责初始化 Bootstrap ApplicationContext ID = "bootstrap"

Bootstrap 是一个根 Spring 上下文，parent = null

> 类比联想 ClassLoader：
>
> ExtClassLoader <- AppClassLoader <- System ClassLoader -> Bootstrap Classloader(null)

## ApplicationContext层次性

### ApplicationContext和BeanFactory的区别

1.从层次性上ApplicationContext扩展了ListableBeanFactory，HierarchicalBeanFactory，即ApplicationContext继承了BeanFacotry

```java
public interface ApplicationContext extends EnvironmentCapable, ListableBeanFactory, HierarchicalBeanFactory,
      MessageSource, ApplicationEventPublisher, ResourcePatternResolver {
}
```

2.从结构而言ApplicationContext关联了BeanFactory实现

```java
public abstract class AbstractRefreshableApplicationContext extends AbstractApplicationContext {
    ......
    /** Bean factory for this context */
        @Nullable
        private DefaultListableBeanFactory beanFactory;
    ......
}
```



3.BeanFactory才是真正的Bean容器，管理bean生命周期；ApplicationContext包含了BeanFactory的职责，并且还有其他功能。

### ApplicationContext的层次性

ApplicationContext继承了HierarchicalBeanFactory

ApplicationContext Bean生命周期管理能力来自于BeanFactory，并且有是HierarchicalBeanFactory子接口，说明它具有BeanFactory的层次性关系。

同时他有getParent()方法返回双亲ApplicationContext，其子接口ConfigurableApplicationContext拥有设置双亲Application的能力。



```java
@EnableAutoConfiguration
public class SpringBootApplicationBootstrap {

    public static void main(String[] args) {

        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext();
        context.setId("小马哥");
        context.refresh();

        new SpringApplicationBuilder(SpringBootApplicationBootstrap.class)
                .parent(context) // 显式地设置双亲上下文
                .run(args);

    }
}
```

bootStrap要提早加载某些资源？？

- application-1的parentId=小马哥
- 小马哥的parentId=application-1
- bootStrap的ParrentId=null

子上下文必须双亲上下文启动，上下文启动顺序：bootStrap（ready）-->小马哥（ready）-->application-1（ready）

## EnvironmentEndpoint

### Environment 有两种实现方式

普通类型：StandardEnvironment

Web类型：StandardServletEnvironment

Environment->AbstractEnvironment->StandardEnvironment

Enviroment 关联着一个PropertySources 实例

PropertySources 关联着多个PropertySource，并且有优先级

其中比较常用的PropertySource 实现：

Java System#getProperties 实现：名称"systemProperties"，对应的内容 System.getProperties()

Java System#getenv 实现(环境变量）： 名称"systemEnvironment"，对应的内容System.getProperties()

###  Spring Boot 优先级顺序

1. [Devtools global settings properties](https://docs.spring.io/spring-boot/docs/2.0.0.BUILD-SNAPSHOT/reference/htmlsingle/#using-boot-devtools-globalsettings) on your home directory (`~/.spring-boot-devtools.properties` when devtools is active).
2. [`@TestPropertySource`](https://docs.spring.io/spring/docs/5.0.4.RELEASE/javadoc-api/org/springframework/test/context/TestPropertySource.html) annotations on your tests.
3. [`@SpringBootTest#properties`](https://docs.spring.io/spring-boot/docs/2.0.0.BUILD-SNAPSHOT/api/org/springframework/boot/test/context/SpringBootTest.html) annotation attribute on your tests.
4. Command line arguments.
5. Properties from `SPRING_APPLICATION_JSON` (inline JSON embedded in an environment variable or system property).
6. `ServletConfig` init parameters.
7. `ServletContext` init parameters.
8. JNDI attributes from `java:comp/env`.
9. Java System properties (`System.getProperties()`).
10. OS environment variables.
11. A `RandomValuePropertySource` that has properties only in `random.*`.
12. [Profile-specific application properties](https://docs.spring.io/spring-boot/docs/2.0.0.BUILD-SNAPSHOT/reference/htmlsingle/#boot-features-external-config-profile-specific-properties) outside of your packaged jar (`application-{profile}.properties` and YAML variants).
13. [Profile-specific application properties](https://docs.spring.io/spring-boot/docs/2.0.0.BUILD-SNAPSHOT/reference/htmlsingle/#boot-features-external-config-profile-specific-properties) packaged inside your jar (`application-{profile}.properties` and YAML variants).
14. Application properties outside of your packaged jar (`application.properties` and YAML variants).
15. Application properties packaged inside your jar (`application.properties` and YAML variants).
16. [`@PropertySource`](https://docs.spring.io/spring/docs/5.0.4.RELEASE/javadoc-api/org/springframework/context/annotation/PropertySource.html) annotations on your `@Configuration` classes.
17. Default properties (specified by setting `SpringApplication.setDefaultProperties`).

#  服务注册与发现

## Eureka



自我保护模式： 默认配置下，如果Eureka Server每分钟收到心跳续约的数量低于一个阈值（instance的数量(60/每个instance的心跳间隔秒数)自我保护系数），并且持续15分钟，就会触发自我保护。在自我保护模式中，Eureka Server会保护服务注册表中的信息，不再注销任何服务实例。当它收到的心跳数重新恢复到阈值以上时，该Eureka Server节点就会自动退出自我保护模式。它的设计理念就是宁可保留错误的服务注册信息，也不盲目注销任何可能健康的服务实例。该模式可以通过eureka.server.enable-self-preservation = false来禁用，同时eureka.instance.lease-renewal-interval-in-seconds可以用来更改心跳间隔，eureka.server.renewal-percent-threshold可以用来修改自我保护系数（**默认0.85**）。 



## Zookeeper



# 客户端负载均衡



## 16.1 How to Include Ribbon

To include Ribbon in your project, use the starter with a group ID of `org.springframework.cloud` and an artifact ID of `spring-cloud-starter-netflix-ribbon`. See the [Spring Cloud Project page](https://projects.spring.io/spring-cloud/) for details on setting up your build system with the current Spring Cloud Release Train.

```xml
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
        </dependency>
```





# Spring Cloud 服务熔断

## 微服务容错手段

为网络请求设置超时



使用断路器模式



## 使用Hystrix实现容错





