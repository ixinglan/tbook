# jdk14
`https://openjdk.java.net/projects/jdk/14/` --openjdk网站我们可以看到有以下16大特性
```http request
305:	Pattern Matching for instanceof (Preview)
343:	Packaging Tool (Incubator)
345:	NUMA-Aware Memory Allocation for G1
349:	JFR Event Streaming
352:	Non-Volatile Mapped Byte Buffers
358:	Helpful NullPointerExceptions
359:	Records (Preview)
361:	Switch Expressions (Standard)
362:	Deprecate the Solaris and SPARC Ports
363:	Remove the Concurrent Mark Sweep (CMS) Garbage Collector
364:	ZGC on macOS
365:	ZGC on Windows
366:	Deprecate the ParallelScavenge + SerialOld GC Combination
367:	Remove the Pack200 Tools and API
368:	Text Blocks (Second Preview)
370:	Foreign-Memory Access API (Incubator)
```

## 305:	Pattern Matching for instanceof (Preview) instanceof新特性
```java
//以前我们是这么使用的
if (obj instanceof String) {
    String s = (String) obj;
    // use s
}

//新特性我们可以这么使用,即将强制转换类型绑定给变量,但变量只能在true块中使用
if (obj instanceof String s) {
    // can use s here
} else {
    // can't use s here
}
if (!(obj instanceof String s)) {
    // can't use s here
} else {
    // can use s here
}

//可以在&&后使用, 但不能在||后使用
if (obj instanceof String s && s.length > 10) {
    // can use s here
}
```
## 359:	Records (Preview) 记录类型
```java
import static java.util.Calendar.*;

//有望替代lombok
public class Test {
    public record User(int x, int y) {
    }

    public static void main(String[] args) {
        User user = new User(1, 2);
        System.out.println(user.x);
        System.out.println(user.y);

    }
}

```
## 361:	Switch Expressions (Standard) switch语法形式改变
```java
import static java.util.Calendar.*;

public class Test {
    public static void main(String[] args) {
        formatWeek(0);
        formatWeek(1);
        formatWeek(2);
        formatWeek(3);
        formatWeek(4);
        formatWeek(5);
        formatWeek(6);

        System.out.println(formatWeek2(5));
        System.out.println(formatWeek2(6));
    }

    public static void formatWeek(int day) {
        switch (day) {
            case MONDAY -> System.out.println("MONDAY");
            case TUESDAY -> System.out.println("TUESDAY");
            case WEDNESDAY -> System.out.println("WEDNESDAY");
            case THURSDAY -> System.out.println("THURSDAY");
            case FRIDAY -> System.out.println("FRIDAY");
            case SATURDAY -> System.out.println("SATURDAY");
            case SUNDAY -> System.out.println("SUNDAY");
        }
    }

    public static String formatWeek2(int day) {
        String str = switch (day) {
            case MONDAY -> "MONDAY";
            case TUESDAY -> "TUESDAY";
            case WEDNESDAY -> "WEDNESDAY";
            case THURSDAY -> "THURSDAY";
            case FRIDAY -> "FRIDAY";
            case SATURDAY -> "SATURDAY";
            case SUNDAY -> "SUNDAY";
            default -> "NONE";
        };
        return str;
    }
}

```
## 368:	Text Blocks (Second Preview) 文本块
```java
//old
String html = "<html>\n" +
              "    <body>\n" +
              "        <p>Hello, world</p>\n" +
              "    </body>\n" +
              "</html>\n";
//new 
String html = """
              <html>
                  <body>
                      <p>Hello, world</p>
                  </body>
              </html>
              """;
```

## NullPointException
该特性可以更好地提示哪个地方出现的空指针，需要通过`-XX:+ShowCodeDetailsInExceptionMessages`开启

## 弃用ParallelScavenge和SerialOld GC组合

## 删除CMS垃圾回收器

## ZGC on macOS和windows

