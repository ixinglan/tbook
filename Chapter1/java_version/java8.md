# jdk8
* 速度更快
* 代码更少（增加了新的语法 Lambda 表达式） * 强大的 Stream API
* 便于并行
* 最大化减少空指针异常 Optional

## 速度更快
* HashMap,ConcurrentHashMap特性改变(链表+红黑树等) --详细看HashMap部分
* jvm永久区去掉,MetaSpace 元空间,使用物理内存

## Lambda表达式
Lambda 是一个匿名函数，我们可以把 Lambda 表达式理解为是一段可以传递的代码（将代码像数据一样进行传递）。可以写出更简洁、更灵活的代码。作为一种更紧凑的代码风格，使Java的语言表达能力得到了提升。

### 原来的写法与lambda表达式对比
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


```
3. 