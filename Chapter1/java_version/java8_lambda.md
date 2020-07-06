# Lambda表达式
Lambda 是一个匿名函数，我们可以把 Lambda 表达式理解为是一段可以传递的代码（将代码像数据一样进行传递）。可以写出更简洁、更灵活的代码。作为一种更紧凑的代码风格，使Java的语言表达能力得到了提升。

## 原来的写法与lambda表达式对比
1. 匿名内部类
```java
//原来的匿名内部类
@Test
public void test1(){
    Comparator<String> com = new Comparator<String>(){
        @Override
        public int compare(String o1, String o2) {
            return Integer.compare(o1.length(), o2.length());
        }
    };
    
    TreeSet<String> ts = new TreeSet<>(com);
    
    TreeSet<String> ts2 = new TreeSet<>(new Comparator<String>(){
        @Override
        public int compare(String o1, String o2) {
            return Integer.compare(o1.length(), o2.length());
        }
        
    });
}

//现在的 Lambda 表达式
@Test
public void test2(){
    Comparator<String> com = (x, y) -> Integer.compare(x.length(), y.length());
    TreeSet<String> ts = new TreeSet<>(com);
}
```
2. 集合操作
```java
List<Employee> emps = Arrays.asList(
        new Employee(101, "张三", 18, 9999.99),
        new Employee(102, "李四", 59, 6666.66),
        new Employee(103, "王五", 28, 3333.33),
        new Employee(104, "赵六", 8, 7777.77),
        new Employee(105, "田七", 38, 5555.55)
);

//需求：获取公司中年龄小于 35 的员工信息
public List<Employee> filterEmployeeAge(List<Employee> emps){
    List<Employee> list = new ArrayList<>();
    
    for (Employee emp : emps) {
        if(emp.getAge() <= 35){
            list.add(emp);
        }
    }
    
    return list;
}

@Test
public void test3(){
    List<Employee> list = filterEmployeeAge(emps);
    
    for (Employee employee : list) {
        System.out.println(employee);
    }
}

//需求：获取公司中工资大于 5000 的员工信息
public List<Employee> filterEmployeeSalary(List<Employee> emps){
    List<Employee> list = new ArrayList<>();
    
    for (Employee emp : emps) {
        if(emp.getSalary() >= 5000){
            list.add(emp);
        }
    }
    
    return list;
}

//优化方式二：匿名内部类, 需写接口和实现类,具体看java-learning代码
@Test
public void test5(){
    List<Employee> list = filterEmployee(emps, new MyPredicate<Employee>() {
        @Override
        public boolean test(Employee t) {
            return t.getId() <= 103;
        }
    });
    
    for (Employee employee : list) {
        System.out.println(employee);
    }
}

//优化方式三：Lambda 表达式
@Test
public void test6(){
    List<Employee> list = filterEmployee(emps, (e) -> e.getAge() <= 35);
    list.forEach(System.out::println);
    
    System.out.println("------------------------------------------");
    
    List<Employee> list2 = filterEmployee(emps, (e) -> e.getSalary() >= 5000);
    list2.forEach(System.out::println);
}

//优化方式四：Stream API
@Test
public void test7(){
    emps.stream()
        .filter((e) -> e.getAge() <= 35)
        .forEach(System.out::println);
    
    System.out.println("----------------------------------------------");
    
    emps.stream()
        .map(Employee::getName)
        .limit(3)
        .sorted()
        .forEach(System.out::println);
}
```

## lambda语法
* Java8中引入了一个新的操作符 "->" 该操作符称为箭头操作符或 Lambda 操作符箭头操作符将 Lambda 表达式拆分成两部分, 其中:
> 左侧:ambda 表达式的参数列表
> 右侧:Lambda 表达式中所需执行的功能， 即 Lambda 体
* 语法格式
> 1. 无参数,无返回值 () -> System.out.println("Hello Lambda!")
> 2. 有1个参数,无返回值,有1个参数时,小括号可不写 (x) -> System.out.println(x)/ x-> System.out.println(x);
> 3. 有两个以上的参数，有返回值，并且 Lambda 体中有多条语句
     Comparator<Integer> com = (x, y) -> {  
          System.out.println("函数式接口");  
          return Integer.compare(x, y);  
     };
> 4. 若 Lambda 体中只有一条语句， return 和 大括号都可以省略不写 `Comparator<Integer> com = (x, y) -> Integer.compare(x, y)`
> 5. Lambda 表达式的参数列表的数据类型可以省略不写，因为JVM编译器通过上下文推断出，数据类型，即“类型推断” `(Integer x, Integer y) -> Integer.compare(x, y)`
* lambda表达式需要“函数式接口”的支持
> 函数式接口：接口中只有一个抽象方法的接口，称为函数式接口。 可以使用注解 @FunctionalInterface 修饰,可以检查是否是函数式接口

```java
public class TestLambda2 {
	
	@Test
	public void test1(){
		int num = 0;//jdk 1.7 前，必须是 final
		
		Runnable r = new Runnable() {
			@Override
			public void run() {
				System.out.println("Hello World!" + num);
			}
		};
		
		r.run();
		
		System.out.println("-------------------------------");
		
		Runnable r1 = () -> System.out.println("Hello Lambda!");
		r1.run();
	}
	
	@Test
	public void test2(){
		Consumer<String> con = x -> System.out.println(x);
		con.accept("我和我的祖国！");
	}
	
	@Test
	public void test3(){
		Comparator<Integer> com = (x, y) -> {
			System.out.println("测试多行代码");
			return Integer.compare(x, y);
		};
	}
	
	@Test
	public void test4(){
		Comparator<Integer> com = (x, y) -> Integer.compare(x, y);
	}

	//类型抢断示例
	@Test
	public void test5(){
		String[] strs = {"aaa", "bbb", "ccc"};
		
		List<String> list = new ArrayList<>();
		
		show(new HashMap<>());
	}

	public void show(Map<String, Integer> map){
		
	}
	
	//需求：对一个数进行运算
	@Test
	public void test6(){
		Integer num = operation(100, (x) -> x * x);
		System.out.println(num);
		
		System.out.println(operation(200, (y) -> y + 200));
	}
	
	public Integer operation(Integer num, Function<Integer, Integer> function){
		return function.apply(num);
	}
}
```

## java8内置的四大核心函数式接口
1. Consumer<T> : 消费型接口  
`void accept(T t)`
2. Supplier<T> : 供给型接口  
`T get()`
3. Function<T, R> : 函数型接口  
`R apply(T t)`
4. Predicate<T> : 断言型接口  
`boolean test(T t)`

```java
public class TestLambda3 {
	
	//Predicate<T> 断言型接口：
	@Test
	public void test4(){
		List<String> list = Arrays.asList("Hello", "atguigu", "Lambda", "www", "ok");
		List<String> strList = filterStr(list, (s) -> s.length() > 3);
		
		for (String str : strList) {
			System.out.println(str);
		}
	}
	
	//需求：将满足条件的字符串，放入集合中
	public List<String> filterStr(List<String> list, Predicate<String> pre){
		List<String> strList = new ArrayList<>();
		
		for (String str : list) {
			if(pre.test(str)){
				strList.add(str);
			}
		}
		
		return strList;
	}
	
	//Function<T, R> 函数型接口：
	@Test
	public void test3(){
		String newStr = strHandler("\t\t\t 我爱我的祖国   ", (str) -> str.trim());
		System.out.println(newStr);
		
		String subStr = strHandler("我爱我的祖国", (str) -> str.substring(2, 5));
		System.out.println(subStr);
	}
	
	//需求：用于处理字符串
	public String strHandler(String str, Function<String, String> fun){
		return fun.apply(str);
	}
	
	//Supplier<T> 供给型接口 :
	@Test
	public void test2(){
		List<Integer> numList = getNumList(10, () -> (int)(Math.random() * 100));
		
		for (Integer num : numList) {
			System.out.println(num);
		}
	}
	
	//需求：产生指定个数的整数，并放入集合中
	public List<Integer> getNumList(int num, Supplier<Integer> sup){
		List<Integer> list = new ArrayList<>();
		
		for (int i = 0; i < num; i++) {
			Integer n = sup.get();
			list.add(n);
		}
		
		return list;
	}
	
	//Consumer<T> 消费型接口 :
	@Test
	public void test1(){
		happy(10000, (m) -> System.out.println("买西瓜花了：" + m + "元"));
	} 
	
	public void happy(double money, Consumer<Double> con){
		con.accept(money);
	}
}
```

5. 其他接口  

|函数式接口|参数类型|返回类型|用途|
|:---|:---|:---|:---|
|BiFunction<T,U,R>|T,U|R|对类型为T,U参数应用操作，返回R类型的结果。包含方法为R apply(T t,U u);|
|UnaryOperator<T>(Function子接口)|T|T|对类型为T的对象进行一元运算，并返回T类型的结果。包含方法为T apply(T t);|
|BinaryOperator<T>(BiFunction子接口)|T,T|T|对类型为T的对象进行二元运算，并返回T类型的结果。包含方法为T apply(T t1,T t2);|
|BiConsumer<T,U>|T,U|void|对类型为T,U参数应用操作。包含方法为void accept(T t,U u);|
|ToIntFunction<T>  ToLongFunction<T> ToDoubleFunction<T>|T|int,long,double|分别计算int、long、double、值的函数|
|IntFunction<R> LongFunction<R> DoubleFunction<R>|int,long,double|R|参数分别为int、long、double类型的函数|
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 