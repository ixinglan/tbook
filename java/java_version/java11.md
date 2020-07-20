# jdk11

## jshell
除了表达式外,可以创建java类和方法

## 局部类型推断
var: 参考jdk10

## 集合新api
List.of(); List.copyOf(); 参考jdk9,jdk10

## Stream 加强
ofNullable(); 参考jdk9

## String
```java
    @Test
    public void t1(){
        String s = "\t \r\n";
        System.out.println(s.isBlank());//是否空白

        s="\t \r\n abc \t";
        System.out.println("---------------------");
        String s1 = s.strip();//去除首尾空白, trim()只能去除英文空格,strip()能去除任何空白字符
        System.out.println(s1);
        System.out.println(s1.length());//0
        System.out.println("---------------------");
        s.stripTrailing();//去除尾部
        s.stripLeading();//去除首部

        System.out.println("---------------------");
        "java".repeat(3);//复制
        System.out.println("---------------------");
        "A\nB\nC".lines().count(); // 行数统计
    }
```

## InputStream
```java
    //inputStream
    @Test
    public void t2() throws IOException {
        var classLoader = this.getClass().getClassLoader();
        var inputStream = classLoader.getResourceAsStream("jdk_version/java11/javastack");
        try (var outputStream = new FileOutputStream("src/jdk_version/java11/javastack2")) {
            inputStream.transferTo(outputStream);
        }
        inputStream.close();
    }
```

## java异步http客户端
```java
public class HttpClientTest {
    @Test
    public void t1() throws IOException, InterruptedException {
        var request = HttpRequest.newBuilder()
                .uri(URI.create("https://javastack.cn"))
                .GET()
                .build();
        var client = HttpClient.newHttpClient();
        // 同步
        HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
        System.out.println(response.body());
        // 异步
        client.sendAsync(request, HttpResponse.BodyHandlers.ofString())
                .thenApply(HttpResponse::body)
                .thenAccept(System.out::println);
    }
}
```

## 更简化的编译运行程序
java Javastack.java 直接可编译运行
* 执行源文件中的第一个类, 第一个类必须包含主方法
* 并且不可以使用别源文件中的自定义类, 本文件中的自定义类是可以使用的

## unicode10
Unicode 10 增加了8518个字符, 总计达到了136690个字符. 并且增加了4个脚本.同时还有56个新的emoji表情符号

## 新的Epsilon垃圾收集器
A NoOp Garbage Collector
JDK上对这个特性的描述是: 开发一个处理内存分配但不实现任何实际内存回收机制的GC, 一旦可用堆内存用完, JVM就会退出.  
如果有System.gc()调用, 实际上什么也不会发生(这种场景下和-XX:+DisableExplicitGC效果一样), 因为没有内存回收, 这个实现可能会警告用户尝试强制GC是徒劳  

用法 : -XX:+UnlockExperimentalVMOptions -XX:+UseEpsilonGC  
```java
public class EpsilonTest {
    public static void main(String[] args) {
        boolean flag = true;
        List<Garbage> list = new ArrayList<>();
        long count = 0;
        while (flag) {
            list.add(new Garbage());
            if (list.size() == 1000000 && count == 0) {
                list.clear();
                count++;
            }
        }
        System.out.println("程序结束");
    }
}

class Garbage {
    int n = (int) (Math.random() * 100);

    //gc在清除本对象时会调用一次
    @Override
    public void finalize() {
        System.out.println(this + " : " + n + " is dying");
    }
}
//如果使用选项-XX:+UseEpsilonGC, 程序很快就因为堆空间不足而退出
```

* 使用这个选项的原因 :
    > 提供完全被动的GC实现, 具有有限的分配限制和尽可能低的延迟开销,但代价是内存占用和内存吞吐量.  
    > 众所周知, java实现可广泛选择高度可配置的GC实现. 各种可用的收集器最终满足不同的需求, 即使它们的可配置性使它们的功能相交. 有时更容易维护单独的实现, 而不是在现有GC实现上堆积另一个配置选项

* 主要用途
    > 性能测试(它可以帮助过滤掉GC引起的性能假象)
	> 内存压力测试(例如,知道测试用例 应该分配不超过1GB的内存, 我们可以使用-Xmx1g –XX:+UseEpsilonGC, 如果程序有问题, 则程序会崩溃)
	> 非常短的JOB任务(对象这种任务, 接受GC清理堆那都是浪费空间)
	> VM接口测试
	> Last-drop 延迟&吞吐改进


## ZGC jdk11最瞩目特性,但是后面带了Experimental, 说明这还不建议用到生产环境
* GC暂停时间不会超过10ms
* 既能处理几百兆的小堆, 也能处理几个T的大堆(OMG)
* 和G1相比, 应用吞吐能力不会下降超过15%
* 为未来的GC功能和利用colord指针以及Load barriers优化奠定基础
* 初始只支持64位系统

### ZGC的设计目标
支持TB级内存容量，暂停时间低（<10ms),对整个程序吞吐量的影响小于15%。将来还可以扩展实现机制，以支持不少令人兴奋的功能，例如多层堆（即热对象置于DRAM和冷对象置于NVMe闪存），或压缩堆
  
GC是java主要优势之一. 然而, 当GC停顿太长, 就会开始影响应用的响应时间.消除或者减少GC停顿时长, java将对更广泛的应用场景是一个更有吸引力的平台. 此外, 现代系统中可用内存不断增长,用户和程序员希望JVM能够以高效的方式充分利用这些内存, 并且无需长时间的GC暂停时间

```java
public class ZGCTest {
    public static void main(String[] args) {
        List<Garbage2> list = new ArrayList<>();
        int count = 0;
        boolean flag = true;
        while (flag){
            list.add(new Garbage2());
            if(count++==200){
                list.clear();
            }
        }
    }

}
class Garbage2{
    double d1 = 1;
    double d2 = 2;

    @Override
    protected void finalize() throws Throwable {
        System.out.println(this + " is collection");
    }
}
```

## 完全支持Docker容器

## 支持G1上的并行完全垃圾收集
对于 G1 GC，相比于 JDK 8，升级到 JDK 11 即可免费享受到：并行的 Full GC，快速的 CardTable 扫描，自适应的堆占用比例调整（IHOP），在并发标记阶段的类型卸载等等。这些都是针对 G1 的不断增强，其中串行 Full GC 等甚至是曾经被广泛诟病的短板，你会发现 GC 配置和调优在 JDK11 中越来越方便

## Low-Overhead Heap Profiling免费的低耗能飞行记录仪和堆分析仪
通过JVMTI的SampledObjectAlloc回调提供了一个开销低的heap分析方式  
提供一个低开销的, 为了排错java应用问题, 以及JVM问题的数据收集框架, 希望达到的目标如下 :
* 提供用于生产和消费数据作为事件的API
* 提供缓存机制和二进制数据格式
* 允许事件配置和事件过滤
* 提供OS,JVM和JDK库的事件

## 实现RFC7539中指定的ChaCha20和Poly1305两种加密算法, 代替RC4

## Java Flight Recorder
Flight Recorder以前是商业版的特性，在java11当中开源出来，它可以导出事-XX:StartFlightRecording，或者在应用启动之后，使用jcmd来录制件到文件中，之后可以用Java Mission Control来分析。可以在应用启动时配置java -XX:StartFlightRecording，或者在应用启动之后，使用jcmd来录制