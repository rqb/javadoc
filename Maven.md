# Maven之dependencyManagement



## 为什么要使用dependencyManagement

最近在找`lombok+log4j2`的依赖时，由于在国内怎么找都找不到，但是我意外的却发现Spring Boot中居然就已经集成了该依赖项，该依赖是集成在下面的依赖中： 

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-log4j2</artifactId>
    <version>1.5.4.RELEASE</version>
</dependency>
```

所以说我赶紧拿来分解研究，谁知遇到的第一个问题居然是依赖包冲突，虽然说Maven有自己的解决依赖包版本冲突的机制，但是对于有洁癖的人来说，当我们看到下面的信息时，仍然会感觉浑身不爽。 

![1533089442130](C:\Users\renqingbin\AppData\Local\Temp\1533089442130.png)

出现该问题的原因是`slf4j-api`依赖发现了有两个版本，分别是`1.7.25`和`1.7.21`，此时Maven不知道该要依赖哪个，所以说就报出了该故障。而Spring Boot其实是有解决该问题的方案的，就是在`spring-boot-starter-log4j2`中引入父模块，通过在父模块中采用`dependencyManagement`，在此定义子类中的Maven依赖版本号，正是通过如此，我们才能够解决上述问题。 

## dependencyManagement描述

对于dependencyManagement，我们可以参考其官方的描述信息，在[官方文档](http://maven.apache.org/ref/3.0.3/maven-model/maven.html#class_parent)中，其描述信息是这样的： 

> 对于从这一项继承的项目的默认依赖信息。这个部分的依赖关系没有立即得到解决。相反，当从这个派生的POM声明一个匹配的groupId和artifactId所描述的依赖项时，这个部分的版本和其它值将被用于该依赖项，如果它们还没有被指定的话。 

简而言之就是在父Maven模块中通过dependencyManagement来指定子模块中的Maven依赖版本号。 



## dependencyManagement举例

在`spring-boot-starter-log4j2`中，其实我们引入的依赖是这样的： 

```xml
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-slf4j-impl</artifactId>
            <version>2.7</version>
        </dependency>
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-api</artifactId>
            <version>2.7</version>
        </dependency>
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-core</artifactId>
            <version>2.7</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>jcl-over-slf4j</artifactId>
            <version>1.7.25</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>jul-to-slf4j</artifactId>
            <version>1.7.25</version>
        </dependency>
```



而产生版本冲突的原因是`slf4j-api`中包含了`1.7.25`和`1.7.21`两个版本号，因而在其父模块的解决方案中就是添加dependencyManagement，在该dependencyManagement中我们指定`slf4j-api`的版本号，如下： 

```xml
<dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>1.7.25</version>
      </dependency>
    </dependencies>
  </dependencyManagement>
```

下面是该冲突解决之后的结果： 

![1533089788623](C:\Users\renqingbin\AppData\Local\Temp\1533089788623.png)





