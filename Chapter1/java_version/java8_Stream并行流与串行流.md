# 并行流与串行流
并行流就是把一个内容分成多个数据块，并用不同的线程分别处理每个数据块的流。  
Java 8 中将并行进行了优化，我们可以很容易的对数据进行并行操作。Stream API 可以声明性地通过 parallel() 与sequential() 在并行流与顺序流之间进行切换

```java
//求和
@Test
public void test3() {
    long start = System.currentTimeMillis();

    Long sum = LongStream.rangeClosed(0L, 10000000000L)
            .parallel() //切换并行
            .sum();

    System.out.println(sum);

    long end = System.currentTimeMillis();

    System.out.println("耗费的时间为: " + (end - start)); 
}
```