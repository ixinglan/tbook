# Annotation
## 概述
* 从 JDK 5.0 开始 , Java增加了对元数据 (MetaData ) 的支持 , 也就是Annotation( 注解)
* Annotation 其实就是代码里的 特殊标记 , 这些标记可以在编译,类加载,运行时被读取,并执行相应的处理。通过使用 Annotation, 程序员可以在不改变原有逻辑的情况下,在源文件中嵌入一些补充信息。代码分析工具、开发工具和部署工具可以通过这些补充信息进行验证
或者进行部署。
* Annotation 可以像修饰符一样被使用,可用于修饰包类,构造器,方法,成员变量,参数,局部变量的声明,这些信息被保存在 Annotation的 “name=value” 对中
* 在 JavaSE 中，注解的使用目的比较简单，例如标记过时的功能，忽略警告等。在 JavaEE/Android 中注解占据了更重要的角色，例如:用来配置应用程序的任何切面， 代替 JavaEE 旧版中所遗留的繁冗代码和 XML
* 未来的开发模式都是基于注解的，一定程度上可以说： 框架 = 注解 + 反射 + 设计模式。
* 使用 Annotation 时要在其前面增加 @ 符号 , 并 把该 Annotation 当成一个修饰符使用。 用于修饰它支持的程序元素

## 示例

### 文档相关
* @author 标明开发该类模块的作者 多个作者之间使用 分割
* @version 标明该类模块的版本
* @see 参考转向 也就是相关主题
* @since 从哪个版本开始增加的
* @param 对方法中某参数的说明 如果没有参数就不能写
* @return 对方法返回值的说明 如果方法的返回值类型是 void 就不能写
* @exception 对方法可能抛出的异常进行说明 如果方法没有用 throws 显式抛出的异常就不能写  
其中  
* @param @return 和 @exception 这三个标记都是只用于方法的
* @param 的格式要求：@param 形参名 形参类型 形参说明
* @return 的格式要求：@return 返回值类型 返回值说明
* @exception 的格式要求：@exception 异常类型 异常说明
* @param 和 @exception 可以并列多个

### 编译时进行格式检查(3个内置基本注解)
* @Override
* @Deprecated 用于表示所修饰的元素类,方法等已过时。通常是因为所修饰的结构危险或存在更好的选择
* @SuppressWarnings 抑制编译器警告

### 跟踪代码依赖性,实现替代配置文件功能
servlet的@webServlet 或spring的@requestMapping 代替xml文件配置

## 自定义注解
* 定义新的 Annotation 类型使用 @interface 关键字
* 自定义注解自动继承了 java lang annotation Annotation 接口
* Annotation 的成员变量在 Annotation 定义中以无参数方法的形式来 声明。其方法名和返回值定义了该成员的名字和类型。我们称为配置参数。类型只能是八种基本数据类型 、 String 类型 、 Class 类型 、 enum 类型 、 Annotation 类型 、以上所有类型的 数组 。
* 可以在定义 Annotation 的成员变量时为其指定初始值 指定成员变量的初始值可使用 default 关键字
* 如果只有一个参数成员 建议使用 参数名为 value
* 如果定义的注解含有配置参数 那么使用时必须指定参数值 除非它有默认值。格式是 参数名 参数值 如果只有一个参数成员 且名称为 value可以省略 value=
* 没有成员定义的 Annotation 称为 标记 包含成员变量的 Annotation 称为元数据 Annotation
* 注意：自定义注解必须配上注解的信息处理流程才有意义

```java
@interface MyAnnotation {
    String value();
}
```

## 元注解
* @Retention : 只能用于修饰一个 Annotation 定义,用于指定该Annotation的生命周期,Rentention包含一个 RetentionPolicy 类型的成员变量,使用Rentention 时必须为该 value 成员变量指定值
    > RetentionPolicy.SOURCE 在源文件中有效（即源文件保留) 编译器 直接丢弃这种策略的注释)  
    > RetentionPolicy.CLASS 在 class 文件中有效（即 class 保留）当运行 Java 程序时,JVM不会保留注解。这是默认值  
    > RetentionPolicy.RUNTIME 在运行时有效（即运行时保留) 当运行 Java 程序时 , JVM 会保留注释。程序 可以通过反射获取 该注释  
* @Target : 用于修饰 Annotation 定义 , 用于指定被修饰的 Annotation 能用于修饰哪些程序元素。 @Target 也包含一个名为 value 的
    > TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE, MODULE
* @Documented: 用于指定被该元 Annotation 修饰的 Annotation 类将被javadoc 工具提取成文档。默认情况下 javadoc 是不包括注解的.定义为 Documented 的注解必须设置 Retention 值为 RUNTIME
* @Inherited: 被它修饰的 Annotation 将具有继承性。如果某个类使用了被@Inherited 修饰的 Annotation, 则其子类将自动具有该注解。比如：如果 把标有 @Inherited 注解的自定义的注解标注在类级别上子类则可以
继承父类类级别的注解,实际应用中，使用较少

## 反射获取
```java
@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE, MODULE})
@Retention(RetentionPolicy.RUNTIME)
@interface MyAnnotation {
    String value() default "hello";
}

@MyAnnotation("Person")
public class Person {
}

public class AnnotationTest {

    //反射获取
    @Test
    public void t1(){
        Class<Person> clazz = Person.class;
        Annotation[] annotations = clazz.getAnnotations();
        Arrays.asList(annotations).forEach(System.out::println);

        //获取值
        MyAnnotation[] annotationsByType = clazz.getAnnotationsByType(MyAnnotation.class);
        for (MyAnnotation m : annotationsByType) {
            System.out.println(m.value());
        }
    }
}
```

## 可重复注解与类型注解
1.8新特性
