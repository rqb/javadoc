# 一：字符串和数组

## 1.什么是字符串不可变?

Diagram to show Java String’s Immutability,Here are a set of diagrams to illustrate Java String's immutability.

### 1.1声明一个字符串

The following code initializes a string s.

```java
String s = "abcd";
```

The variable *s* stores the reference of a string object as shown below. The arrow can be interpreted as "store reference of". 

![String-Immutability-1](D:\private\document\javadoc\文章\gif\String-Immutability-1.jpeg) 

### 1.2. 字符串变量指向另外一个字符串变量

The following code assign *s* to *s2*.

```java
String s2 = s;
```

*s2* stores the same reference value since it is the same string object. 

![String-Immutability-2](D:\private\document\javadoc\文章\gif\String-Immutability-2.jpeg) 

### 1.3 连接字符串

When we concatenate a string "ef" to *s*, 

```java
s = s.concat("ef");
```

s stores the reference of the newly created string object as shown below. 

![string-immutability](D:\private\document\javadoc\文章\gif\string-immutability-650x279.jpeg) 

### 1.4.总结

In summary, once a string is created in [memory](https://www.programcreek.com/2013/04/jvm-run-time-data-areas/)(heap), it can not be changed. All methods of String do not change the string itself, but rather return a new String.

If we need a string that can be modified, we will need *StringBuffer* or *StringBuilder*. Otherwise, there would be a lot of time wasted for Garbage Collection, since each time a new String is created. [Here](https://www.programcreek.com/2011/11/java-read-file-into-a-string/) is an example of using *StringBuilder*.



## 2.How does substring() work?



## 3.为什么字符串是不可变的？



## 4.Create Java string Using ” ” or constructor?



## Is string passed by reference?



## length vs. length()



## How to check if an array contains a value efficiently?



## What is varargs?



## null到底是什么?

Let's start from the following statement: 

```java
String x = null;
```

### 1.What exactly does this statement do?


Recall what is a variable and what is a value. A common metaphor is that a variable is similar to a box. Just as you can use a box to store something, you can use a variable to store a value. When declaring a variable, we need to set its type.

There are two major categories of types in Java: primitive and reference. Variables declared of a primitive type store values; variables declared of a reference type store references. In this case, the initialization statement declares a variables “x”. “x” stores String reference. It is null here.

The following visualization gives a better sense about this concept.

![what-is-null](D:\private\document\javadoc\文章\gif\what-is-null-150x150.png) 

If x = "abc", it looks like the following: 

![variable-reference](D:\private\document\javadoc\文章\gif\variable-reference-300x144.png) 

### **2. What exactly is null in memory?**

What exactly is null in memory? Or What is the null value in Java?

First of all, null is not a valid object instance, so there is no memory allocated for it. It is simply a value that indicates that the object reference is not currently referring to an object.

From [JVM Specifications](https://docs.oracle.com/javase/specs/jvms/se7/html/jvms-2.html#jvms-2.4):

> The Java Virtual Machine specification does not mandate a concrete value encoding null. 

I would assume it is all zeros of something similar like it is on other C like languages. 

### **3. What exactly is x in memory?**

Now we know what null is. And we know a variable is a storage location and an associated symbolic name (an identifier) which contains some value. Where exactly x is in memory?

From [the diagram of JVM run-time data areas](https://www.programcreek.com/2013/04/jvm-run-time-data-areas/), we know that since each method has a private stack frame within the thread's stack, the local variable are located on that frame.

# 二：公共方法

## Comparator vs. Comparable

## The contract between hashCode() and equals()

## Java passes object by reference or by value?

## Iteration vs. recursion

# 三：类和接口

## Overloading vs. overriding

## What is instance initializer?

## Fields can not be overridden?

## Inheritance vs. composition

## How to use Java Enum?

## How many types of inner classes?

## What is inner interface?

## Constructors of sub and super classes?

## 4 access levels

## When to use private constructors?

# 4.1 集合

## Class hierarchy of Collection and Map

## ArrayList vs. LinkedList vs. Vector

## HashSet vs. TreeSet vs. LinkedHashSet

## HashMap vs. TreeMap vs. HashTable vs. LinkedHashMap

## Sort Map By Value

## A TreeSet example

## Efficient counter

## Frequently used methods of HashMap

## Deep understanding of Arrays.sort(T[], Comparator < ? super T > c)

## How do developers sort?

## java.util.ConcurrentModificationException

# 4.2 泛型

## Why do we need generic types?

## Why do we need generic methods?

## What is type erasure?

## Set vs. Set<?>

## What's the best way of converting Array to ArrayList?

# 异常

## How exception handling works?

## Class hierarchy of exceptions

# 并发

## Monitors – the key idea of Java synchronization

## How to make a method thread-safe?

## join()

## notify() and wait()

## Create thread by overriding

# 7.I/O & 数据库

## Read file line by line

## Write file line by line

## FileOutputStream vs. FileWriter

## Should .close() be put in finally block or not?

## Java serialization

## How to use properties file?

# 编译器和java虚拟机

## JVM run-time data areas

## How does Java handle aliasing？

## What does an array look like in memory?

## What is memory leak?

## What can be learned from "Hello World"?

## Whan and how a class is loaded and initialized?

## Static type checking

## Generate code for overloaded and overridden methods?

# 9Top collections

## Top 10 mistakes Java developers make

## Top 10 methods for Java arrays

## Top 10 questions of Java strings

## Top 10 questions for Java regular expression

## Top 10 questions about Java exceptions

## Top 10 questions about Java collections

## Top 9 questions about Java maps

## The most widely used Java libraries

## Top 10 websites for advanced level Java developers

## Top 10 books for advanced level Java developers

## Top 100 High-quality Java blogs

## Top 10 algorithms for coding interview

## Top 8 diagrams for understanding Java

## Top 100 Java Classes

# 10.Popular Libraries, Frameworks and Tools

## Reflection tutorial

## What is framework?

## Why do we need Java web frameworks like Struts 2?

## What is Servlet Container? What is Tomcat?

## What is aspect-oriented Programming?

## Library vs. framework?

## Spring

## Open source projects using Spring framework

## Design patterns in stories

## Struts 2

## How CVS works?

## Why do we need software testing?

## Convert java jar file to exe

## Guava

## Log4j

## JSoup

## Swing

# 11.Java 8

# 12.Others

## Compile and Run Java in Command Line with External Jars

## How to build your own library?

## How to get double?

## Java and computer science courses

## Java vs. Python: Basic Syntax, Date Types

## How to write a crawler?

## 8 things programmers can do at weekends

## Declaration, initialization and scoping

## Date formatting(add more)

## Path of package and class





