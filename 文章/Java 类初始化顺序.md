# Java类的初始化顺序

## 无继承关系的Java 类初始化

一个java方法中的定义主要有以下几种：

普通变量、普通方法块，静态变量、静态方法块、构造函数。类在初始化时他们的初始化顺序为：

（1）静态属性初始化，静态变量和静态方法块根据定义的先后顺序进行初始化；

（2）普通属性初始化，普通变量和普通方法块根据定义的先后顺序进行初始化；

（3）构造函数初始化

示例：

```java
public class ClassInitTest {

    //普通方法块
    {
        System.out.println("普通方法块初始化......");
    }

    //普通属性
    private String normalFiled = getNormalFiled();

    public ClassInitTest() {
        System.out.println("构造函数初始化......");
    }

    //静态属性
    private static String staticFiled = getStaticFiled();

    //静态块
    static {
        System.out.println("静态方法块初始化......");
    }

    public static String getStaticFiled(){
        System.out.println("静态属性初始化......");
        return "Static Filed";
    }

    public static String getNormalFiled(){
        System.out.println("普通属性初始化......");
        return "Normal Filed";
    }

    public static void main(String[] args) {
        new ClassInitTest();
    }
}
```

以上代码的输出结果为：

> 静态变量初始化......
> 静态方法块初始化......
> 普通方法块初始化......
> 普通变量初始化......
> 构造函数初始化......

## 有继承关系的Java类初始化

初始化顺序为：

（1）父类静态属性（静态变量和静态方法块）按照定义顺序初始化；

（2）子类静态属性（静态变量和静态方法块）按照定义顺序初始化；

（3）父类普通属性（普通变量和普通方法块）按照定义顺序初始化；

（4）父类构造函数初始化；

（5）子类普通属性（普通变量和普通方法块）按照定义顺序初始化；

（6）子类构造函数初始化；

```java
public class FatherClass {
    //普通方法块
    {
        System.out.println("父类普通方法块初始化......");
    }

    //普通变量
    private String normalFiled = getNormalFiled();
    
    public FatherClass() {
        System.out.println("父类构造函数初始化......");
    }

    //静态变量
    private static String staticFiled = getStaticFiled();

    //静态块
    static {
        System.out.println("父类静态方法块初始化......");
    }
    
    public static String getStaticFiled(){
        System.out.println("父类静态变量初始化......");
        return "Static Filed";
    }

    public static String getNormalFiled(){
        System.out.println("父类普通变量初始化......");
        return "Normal Filed";
    }
}
```



```
public class SubClass extends FatherClass {
    //普通方法块
    {
        System.out.println("子类普通方法块初始化......");
    }

    //普通变量
    private String normalFiled = getNormalFiled();

    public SubClass() {
        System.out.println("子类构造函数初始化......");
    }

    //静态变量
    private static String staticFiled = getStaticFiled();

    //静态块
    static {
        System.out.println("子类静态方法块初始化......");
    }

    public static String getStaticFiled(){
        System.out.println("子类静态变量初始化......");
        return "Static Filed";
    }

    public static String getNormalFiled(){
        System.out.println("子类普通变量初始化......");
        return "Normal Filed";
    }

    public static void main(String[] args) {
        new SubClass();
    }
}
```



> 以上代码的输出结果为：
>
> 父类静态变量初始化......
> 父类静态方法块初始化......
> 子类静态变量初始化......
> 子类静态方法块初始化......
> 父类普通方法块初始化......
> 父类普通变量初始化......
> 父类构造函数初始化......
> 子类普通方法块初始化......
> 子类普通变量初始化......
> 子类构造函数初始化......