# 重复注解与类型注解

```java
/**
 * 支持重复注解
 */
@Repeatable(MyAnnotations.class)
@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE, MODULE})
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotation {
    String value() default "";
}

/**
 *
 */
@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE, MODULE})
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotations {
    MyAnnotation[] value();
}

/**
 * @description: 测试重复注解
 */
public class AnnotatonTest {

    @Test
    public void test() throws NoSuchMethodException {
        Class<AnnotatonTest> clazz = AnnotatonTest.class;
        Method show = clazz.getMethod("show");

        MyAnnotation[] annotationsByType = show.getAnnotationsByType(MyAnnotation.class);
        for (MyAnnotation m : annotationsByType) {
            System.out.println(m.value());
        }
    }

    @MyAnnotation("thank")
    @MyAnnotation("you")
    public void show(){}
}
```