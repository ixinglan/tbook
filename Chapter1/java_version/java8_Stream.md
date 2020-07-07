# Stream
流(Stream) 到底是什么呢？  
是数据渠道，用于操作数据源（集合、数组等）所生成的元素序列。“集合讲的是数据，流讲的是计算！”  
>注意：  
> Stream 自己不会存储元素  
> Stream 不会改变源对象。相反，他们会返回一个持有结果的新Stream  
> Stream 操作是延迟执行的。这意味着他们会等到需要结果的时候才执行

## 创建stream

```java
@Test
public void test1(){
    //1. Collection 提供了两个方法  stream() 与 parallelStream()
    List<String> list = new ArrayList<>();
    Stream<String> stream = list.stream(); //获取一个顺序流
    Stream<String> parallelStream = list.parallelStream(); //获取一个并行流
    
    //2. 通过 Arrays 中的 stream() 获取一个数组流
    Integer[] nums = new Integer[10];
    Stream<Integer> stream1 = Arrays.stream(nums);
    
    //3. 通过 Stream 类中静态方法 of()
    Stream<Integer> stream2 = Stream.of(1,2,3,4,5,6);
    
    //4. 创建无限流
    //迭代
    Stream<Integer> stream3 = Stream.iterate(0, (x) -> x + 2).limit(10);
    stream3.forEach(System.out::println);
    
    //生成
    Stream<Double> stream4 = Stream.generate(Math::random).limit(2);
    stream4.forEach(System.out::println);
    
}
```

## 中间操作
多个中间操作可以连接起来形成一个流水线，除非流水线上触发终止操作，否则中间操作不会执行任何的处理！而在终止操作时一次性全部处理，称为“惰性求值”

|方法|描述|
|:---|:---|
|filter(Predicate p)|接收Lambda ，从流中排除某些元素|
|distinct()|筛选，通过流所生成元素的hashCode() 和equals() 去除重复元素|
|limit(long maxSize)|截断流，使其元素不超过给定数量|
|skip(long n)|跳过元素，返回一个扔掉了前n 个元素的流。若流中元素不足n 个，则返回一个空流。与limit(n) 互补|
|map(Functionf)|接收一个函数作为参数，该函数会被应用到每个元素上，并将其映射成一个新的元素。|
|mapToDouble(ToDoubleFunction f)|接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的DoubleStream。|
|mapToInt(ToIntFunction f)|接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的IntStream。|
|mapToLong(ToLongFunction f)|接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的LongStream。|
|flatMap(Function f)|接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流|
|sorted()|产生一个新流，其中按自然顺序排序|
|sorted(Comparatorcomp)|产生一个新流，其中按比较器顺序排序|

```java
public class TestStreamaAPI {
	
	//1. 创建 Stream
	@Test
	public void test1(){
		//1. Collection 提供了两个方法  stream() 与 parallelStream()
		List<String> list = new ArrayList<>();
		Stream<String> stream = list.stream(); //获取一个顺序流
		Stream<String> parallelStream = list.parallelStream(); //获取一个并行流
		
		//2. 通过 Arrays 中的 stream() 获取一个数组流
		Integer[] nums = new Integer[10];
		Stream<Integer> stream1 = Arrays.stream(nums);
		
		//3. 通过 Stream 类中静态方法 of()
		Stream<Integer> stream2 = Stream.of(1,2,3,4,5,6);
		
		//4. 创建无限流
		//迭代
		Stream<Integer> stream3 = Stream.iterate(0, (x) -> x + 2).limit(10);
		stream3.forEach(System.out::println);
		
		//生成
		Stream<Double> stream4 = Stream.generate(Math::random).limit(2);
		stream4.forEach(System.out::println);
		
	}
	
	//2. 中间操作
	List<Employee> emps = Arrays.asList(
			new Employee(102, "李四", 59, 6666.66),
			new Employee(101, "张三", 18, 9999.99),
			new Employee(103, "王五", 28, 3333.33),
			new Employee(104, "赵六", 8, 7777.77),
			new Employee(104, "赵六", 8, 7777.77),
			new Employee(104, "赵六", 8, 7777.77),
			new Employee(105, "田七", 38, 5555.55)
	);
	
	/*
	  筛选与切片
		filter——接收 Lambda ， 从流中排除某些元素。
		limit——截断流，使其元素不超过给定数量。
		skip(n) —— 跳过元素，返回一个扔掉了前 n 个元素的流。若流中元素不足 n 个，则返回一个空流。与 limit(n) 互补
		distinct——筛选，通过流所生成元素的 hashCode() 和 equals() 去除重复元素
	 */
	
	//内部迭代：迭代操作 Stream API 内部完成
	@Test
	public void test2(){
		//所有的中间操作不会做任何的处理
		Stream<Employee> stream = emps.stream()
			.filter((e) -> {
				System.out.println("测试中间操作");
				return e.getAge() <= 35;
			});
		
		//只有当做终止操作时，所有的中间操作会一次性的全部执行，称为“惰性求值”
		stream.forEach(System.out::println);
	}
	
	//外部迭代
	@Test
	public void test3(){
		Iterator<Employee> it = emps.iterator();
		
		while(it.hasNext()){
			System.out.println(it.next());
		}
	}
	
	@Test
	public void test4(){
		emps.stream()
			.filter((e) -> {
				System.out.println("短路！"); // &&  ||
				return e.getSalary() >= 5000;
			}).limit(3)
			.forEach(System.out::println);
	}
	
	@Test
	public void test5(){
		emps.parallelStream()
			.filter((e) -> e.getSalary() >= 5000)
			.skip(2)
			.forEach(System.out::println);
	}
	
	@Test
	public void test6(){
		emps.stream()
			.distinct()
			.forEach(System.out::println);
	}
	
	/*
		映射
		map——接收 Lambda ， 将元素转换成其他形式或提取信息。接收一个函数作为参数，该函数会被应用到每个元素上，并将其映射成一个新的元素。
		flatMap——接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流
	 */
	@Test
	public void test1(){
		Stream<String> str = emps.stream()
			.map((e) -> e.getName());
		
		System.out.println("-------------------------------------------");
		
		List<String> strList = Arrays.asList("aaa", "bbb", "ccc", "ddd", "eee");
		
		Stream<String> stream = strList.stream()
			   .map(String::toUpperCase);
		
		stream.forEach(System.out::println);
		
		Stream<Stream<Character>> stream2 = strList.stream()
			   .map(TestStreamAPI1::filterCharacter);
		
		stream2.forEach((sm) -> {
			sm.forEach(System.out::println);
		});
		
		System.out.println("---------------------------------------------");
		
		Stream<Character> stream3 = strList.stream()
			   .flatMap(TestStreamAPI1::filterCharacter);
		
		stream3.forEach(System.out::println);
	}

	public static Stream<Character> filterCharacter(String str){
		List<Character> list = new ArrayList<>();
		
		for (Character ch : str.toCharArray()) {
			list.add(ch);
		}
		
		return list.stream();
	}
	
	/*
		sorted()——自然排序
		sorted(Comparator com)——定制排序
	 */
	@Test
	public void test2(){
		emps.stream()
			.map(Employee::getName)
			.sorted()
			.forEach(System.out::println);
		
		System.out.println("------------------------------------");
		
		emps.stream()
			.sorted((x, y) -> {
				if(x.getAge() == y.getAge()){
					return x.getName().compareTo(y.getName());
				}else{
					return Integer.compare(x.getAge(), y.getAge());
				}
			}).forEach(System.out::println);
	}

}
```

## 终止操作
终止操作会从流的流水线生成结果。其结果可以是任何不是流的值，例如：List、Integer，甚至是void

### 查找与匹配

|方法|描述|
|:---|:---|
|allMatch(Predicate p)|检查是否匹配所有元素|
|anyMatch(Predicate p)|检查是否至少匹配一个元素|
|noneMatch(Predicatep)|检查是否没有匹配所有元素|
|findFirst()|返回第一个元素|
|findAny()|返回当前流中的任意元素|
|count()|返回流中元素总数|
|max(Comparatorc)|返回流中最大值|
|min(Comparatorc)|返回流中最小值|
|forEach(Consumerc)|内部迭代(使用Collection 接口需要用户去做迭代，称为外部迭代。相反，Stream API 使用内部迭代——它帮你把迭代做了)|

```java
public class TestStreamAPI2 {
	
	List<Employee> emps = Arrays.asList(
			new Employee(102, "李四", 59, 6666.66, Employee.Status.BUSY),
			new Employee(101, "张三", 18, 9999.99, Employee.Status.FREE),
			new Employee(103, "王五", 28, 3333.33, Employee.Status.VOCATION),
			new Employee(104, "赵六", 8, 7777.77, Employee.Status.BUSY),
			new Employee(104, "赵六", 8, 7777.77, Employee.Status.FREE),
			new Employee(104, "赵七", 8, 7777.77, Employee.Status.FREE),
			new Employee(105, "田七", 38, 5555.55, Employee.Status.BUSY)
	);
	
	//3. 终止操作
	/*
		allMatch——检查是否匹配所有元素
		anyMatch——检查是否至少匹配一个元素
		noneMatch——检查是否没有匹配的元素
		findFirst——返回第一个元素
		findAny——返回当前流中的任意元素
		count——返回流中元素的总个数
		max——返回流中最大值
		min——返回流中最小值
	 */
	@Test
	public void test1(){
			boolean bl = emps.stream()
				.allMatch((e) -> e.getStatus().equals(Employee.Status.BUSY));
			
			System.out.println(bl);
			
			boolean bl1 = emps.stream()
				.anyMatch((e) -> e.getStatus().equals(Employee.Status.BUSY));
			
			System.out.println(bl1);
			
			boolean bl2 = emps.stream()
				.noneMatch((e) -> e.getStatus().equals(Employee.Status.BUSY));
			
			System.out.println(bl2);
	}
	
	@Test
	public void test2(){
		Optional<Employee> op = emps.stream()
			.sorted((e1, e2) -> Double.compare(e1.getSalary(), e2.getSalary()))
			.findFirst();//有可能为空即封装到Optional里
		
		System.out.println(op.get());
		
		System.out.println("--------------------------------");
		
		Optional<Employee> op2 = emps.parallelStream()
			.filter((e) -> e.getStatus().equals(Employee.Status.FREE))
			.findAny();
		
		System.out.println(op2.get());
	}
	
	@Test
	public void test3(){
		long count = emps.stream()
						 .filter((e) -> e.getStatus().equals(Employee.Status.FREE))
						 .count();
		
		System.out.println(count);
		
		Optional<Double> op = emps.stream()
			.map(Employee::getSalary)
			.max(Double::compare);
		
		System.out.println(op.get());
		
		Optional<Employee> op2 = emps.stream()
			.min((e1, e2) -> Double.compare(e1.getSalary(), e2.getSalary()));
		
		System.out.println(op2.get());
	}
	
	//注意：流进行了终止操作后，不能再次使用
	@Test
	public void test4(){
		Stream<Employee> stream = emps.stream()
		 .filter((e) -> e.getStatus().equals(Employee.Status.FREE));
		
		long count = stream.count();
		
		stream.map(Employee::getSalary)
			.max(Double::compare);
	}
}
```

### 归约

|方法|描述|
|:---|:---|
|reduce(T iden, BinaryOperator b)|可以将流中元素反复结合起来，得到一个值。返回T|
|reduce(BinaryOperator b)|可以将流中元素反复结合起来，得到一个值。返回Optional<T>|

备注：map 和reduce 的连接通常称为map-reduce 模式，因Google 用它来进行网络搜索而出名。

```java
/*
    归约
    reduce(T identity, BinaryOperator) / reduce(BinaryOperator) ——可以将流中元素反复结合起来，得到一个值。
 */
@Test
public void test1(){
    List<Integer> list = Arrays.asList(1,2,3,4,5,6,7,8,9,10);
    
    Integer sum = list.stream()
        .reduce(0, (x, y) -> x + y);
    
    System.out.println(sum);
    
    System.out.println("----------------------------------------");
    
    Optional<Double> op = emps.stream()
        .map(Employee::getSalary)
        .reduce(Double::sum);
    
    System.out.println(op.get());
}

//需求：搜索名字中 “六” 出现的次数
@Test
public void test2(){
    Optional<Integer> sum = emps.stream()
        .map(Employee::getName)
        .flatMap(TestStreamAPI1::filterCharacter)
        .map((ch) -> {
            if(ch.equals('六'))
                return 1;
            else 
                return 0;
        }).reduce(Integer::sum);
    
    System.out.println(sum.get());
}
```

### 收集

|方法|描述|
|:---|:---|
|collect(Collector c)|将流转换为其他形式。接收一个Collector接口的实现，用于给Stream中元素做汇总的方法|

Collector 接口中方法的实现决定了如何对流执行收集操作(如收集到List、Set、Map)。但是Collectors 实用类提供了很多静态方法，可以方便地创建常见收集器实例，具体方法与实例如下表:

|方法|返回类型|作用
|:---|:---|:---|
|toList|List<T>|把流中元素收集到List|
|示例|List<Employee>emps=list.stream().collect(Collectors.toList());||
|toSet|Set<T>|把流中元素收集到Set|
|示例|Set<Employee>emps=list.stream().collect(Collectors.toSet());||
|toCollection|Collection<T>|把流中元素收集到创建的集合|
|示例|Collection<Employee>emps=list.stream().collect(Collectors.toCollection(ArrayList::new));|| 
|counting|Long|计算流中元素的个数|
|示例|longcount=list.stream().collect(Collectors.counting());||  
|summingInt|Integer|对流中元素的整数属性求和|
|示例|inttotal=list.stream().collect(Collectors.summingInt(Employee::getSalary));||  
|averagingInt|Double|计算流中元素Integer属性的平均值|
|示例|doubleavg=list.stream().collect(Collectors.averagingInt(Employee::getSalary));||  
|summarizingInt|IntSummaryStatistics|收集流中Integer属性的统计值。如：平均值|
|示例|IntSummaryStatisticsiss=list.stream().collect(Collectors.summarizingInt(Employee::getSalary));||
|joining|String|连接流中每个字符串|
|示例|Stringstr=list.stream().map(Employee::getName).collect(Collectors.joining());||
|maxBy|Optional<T>|根据比较器选择最大值|
|示例|Optional<Emp>max=list.stream().collect(Collectors.maxBy(comparingInt(Employee::getSalary)));||
|minBy|Optional<T>|根据比较器选择最小值|
|示例|Optional<Emp>min=list.stream().collect(Collectors.minBy(comparingInt(Employee::getSalary)));||
|reducing|归约产生的类型|从一个作为累加器的初始值开始，利用BinaryOperator与流中元素逐个结合，从而归约成单个值|
|示例|inttotal=list.stream().collect(Collectors.reducing(0,Employee::getSalar,Integer::sum));||
|collectingAndThen|转换函数返回的类型|包裹另一个收集器，对其结果转换函数|
|示例|inthow=list.stream().collect(Collectors.collectingAndThen(Collectors.toList(),List::size));||
|groupingBy|Map<K,List<T>>|根据某属性值对流分组，属性为K，结果为V|
|示例|Map<Emp.Status, List<Emp>> map= list.stream().collect(Collectors.groupingBy(Employee::getStatus));||
|partitioningBy|Map<Boolean,List<T>>|根据true或false进行分区
|示例|Map<Boolean,List<Emp>>vd=list.stream().collect(Collectors.partitioningBy(Employee::getManage));||

```java
//collect——将流转换为其他形式。接收一个 Collector接口的实现，用于给Stream中元素做汇总的方法
@Test
public void test3(){
    List<String> list = emps.stream()
        .map(Employee::getName)
        .collect(Collectors.toList());
    
    list.forEach(System.out::println);
    
    System.out.println("----------------------------------");
    
    Set<String> set = emps.stream()
        .map(Employee::getName)
        .collect(Collectors.toSet());
    
    set.forEach(System.out::println);

    System.out.println("----------------------------------");
    
    HashSet<String> hs = emps.stream()
        .map(Employee::getName)
        .collect(Collectors.toCollection(HashSet::new));
    
    hs.forEach(System.out::println);
}

@Test
public void test4(){
    Optional<Double> max = emps.stream()
        .map(Employee::getSalary)
        .collect(Collectors.maxBy(Double::compare));
    
    System.out.println(max.get());
    
    Optional<Employee> op = emps.stream()
        .collect(Collectors.minBy((e1, e2) -> Double.compare(e1.getSalary(), e2.getSalary())));
    
    System.out.println(op.get());
    
    Double sum = emps.stream()
        .collect(Collectors.summingDouble(Employee::getSalary));
    
    System.out.println(sum);
    
    Double avg = emps.stream()
        .collect(Collectors.averagingDouble(Employee::getSalary));
    
    System.out.println(avg);
    
    Long count = emps.stream()
        .collect(Collectors.counting());
    
    System.out.println(count);
    
    System.out.println("--------------------------------------------");
    
    DoubleSummaryStatistics dss = emps.stream()
        .collect(Collectors.summarizingDouble(Employee::getSalary));
    
    System.out.println(dss.getMax());
}

//分组
@Test
public void test5(){
    Map<Employee.Status, List<Employee>> map = emps.stream()
        .collect(Collectors.groupingBy(Employee::getStatus));
    
    System.out.println(map);
}

//多级分组
@Test
public void test6(){
    Map<Employee.Status, Map<String, List<Employee>>> map = emps.stream()
        .collect(Collectors.groupingBy(Employee::getStatus, Collectors.groupingBy((e) -> {
            if(e.getAge() >= 60)
                return "老年";
            else if(e.getAge() >= 35)
                return "中年";
            else
                return "成年";
        })));
    
    System.out.println(map);
}

//分区
@Test
public void test7(){
    Map<Boolean, List<Employee>> map = emps.stream()
        .collect(Collectors.partitioningBy((e) -> e.getSalary() >= 5000));
    
    System.out.println(map);
}

//
@Test
public void test8(){
    String str = emps.stream()
        .map(Employee::getName)
        .collect(Collectors.joining("," , "----", "----"));
    
    System.out.println(str);
}

@Test
public void test9(){
    Optional<Double> sum = emps.stream()
        .map(Employee::getSalary)
        .collect(Collectors.reducing(Double::sum));
    
    System.out.println(sum.get());
}
```


























