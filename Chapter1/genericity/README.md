# 泛型 
* 所谓泛型，就是允许在定义类、接口时通过一个标识表示类中某个属性的类型或者是某个方法的返回值及参数类型。这个类型参数将在使用时（例如，继承或实现这个接口，用这个类型声明变量、创建对象时）确定（即传入实际的类型参数，也称为类型实参）。 
* 从JDK1.5以后，Java引入了“参数化类型（Parameterized type）”的概念，允许我们在创建集合时再指定集合元素的类型，正如：`List<String>`，这表明该List只能保存字符串类型的对象。 
* JDK1.5改写了集合框架中的全部接口和类，为这些接口、类增加了泛型支持，从而可以在声明集合变量、创建集合对象时传入类型实参

## 自定义泛型类,泛型接口

```java
public class Order<T> {}
public interface List<T> {}
```

* 泛型类可能有多个参数，此时应将多个参数一起放在尖括号内。比如：`<E1,E2,E3>`
* 泛型类的构造器如下：`public GenericClass(){}`。而下面是错误的：`public GenericClass<E>(){}`
* 实例化后，操作原来泛型位置的结构必须与指定的泛型类型一致。
* 泛型不同的引用不能相互赋值。
    > 尽管在编译时`ArrayList<String>`和`ArrayList<Integer>`是两种类型，但是，在运行时只有一个ArrayList被加载到JVM中。  
* 泛型如果不指定，将被擦除，泛型对应的类型均按照Object处理，但不等价于Object。经验：泛型要使用一路都用。要不用，一路都不要用。
* 如果泛型结构是一个接口或抽象类，则不可创建泛型类的对象。
* jdk1.7，泛型的简化操作：`ArrayList<Fruit> flist = new ArrayList<>()`;
* 泛型的指定中不能使用基本数据类型，可以使用包装类替换。
* 在类/接口上声明的泛型，在本类或本接口中即代表某种类型，可以作为非静态属性的类型、非静态方法的参数类型、非静态方法的返回值类型。但在静态方法中不能使用类的泛型。
* 异常类不能是泛型的
* 不能使用`new E[]`。但是可以：`E[] elements = (E[])new Object[capacity];`参考：ArrayList源码中声明：`Object[] elementData`，而非泛型参数类型数组。

* 父类有泛型，子类可以选择保留泛型也可以选择指定泛型类型：
    > 子类不保留父类的泛型：按需实现  
        >> 没有类型 擦除   
        >> 具体类型  

    > 子类保留父类的泛型：泛型子类  
        >> 全部保留  
        >> 部分保留  

```java
class Father<T1, T2> {
}
// 子类不保留父类的泛型
// 1)没有类型 擦除
class Son1 extends Father {// 等价于class Son extends Father<Object,Object>{
}
// 2)具体类型
class Son2 extends Father<Integer, String> {
}
// 子类保留父类的泛型
// 1)全部保留
class Son3<T1, T2> extends Father<T1, T2> {
}
// 2)部分保留
class Son4<T2> extends Father<Integer, T2> {
}

// 子类不保留父类的泛型
// 1)没有类型 擦除
class Son<A, B> extends Father{//等价于class Son extends Father<Object,Object>{
}
// 2)具体类型
class Son2<A, B> extends Father<Integer, String> {
}
// 子类保留父类的泛型
// 1)全部保留
class Son3<T1, T2, A, B> extends Father<T1, T2> {
}
// 2)部分保留
class Son4<T2, A, B> extends Father<Integer, T2> {
}
```

## 自定义泛型方法
[访问权限] <泛型> 返回类型 方法名([泛型标识 参数名称]) 抛出的异常

```java
public class ListUtil<T> {
    //泛型方法：在方法中出现了泛型的结构，泛型参数与类的泛型参数没有任何关系。
    //泛型方法所属的类是不是泛型类都没有关系。
    //泛型方法，可以声明为静态的。原因：泛型参数是在调用方法时确定的。并非在实例化类时确定。
    public static <E> List<E> copyFromArrayToList(E[] arr) {
        ArrayList<E> list = new ArrayList<>();

        for (E e : arr) {
            list.add(e);
        }
        return list;

    }
}
```

## 泛型在继承上的体现
* 虽然类A是类B的父类，但是`G<A> 和G<B>二者不具备子父类关系，二者是并列关系`
* 类A是类B的父类，`A<G> 是 B<G> 的父类`

## 通配符的使用 "?"
* 类A是类B的父类，`G<A>和G<B>是没有关系的，二者共同的父类是：G<?> `
  ```java
        //list是 list1和list2的父类
        List<Object> list1 = null;
        List<String> list2 = null;
        List<?> list = null;

        list = list1;
        list = list2;
  ```
* 不能写入, 但是写入null 可以, 通过get()方法获取的是Object类型
* 注意点
  ```java
    //注意点1：编译错误：不能用在泛型方法声明上，返回值类型前面<>不能使用?
    public static <?> void test(ArrayList<?> list){
    }

    //注意点2：编译错误：不能用在泛型类的声明上
    class GenericTypeClass<?>{
    }

    //注意点3：编译错误：不能用在创建对象上，右边属于创建集合对象
    ArrayList<?> list2 = new ArrayList<?>();
  ```
### 有限制条件的通配符使用
* 通配符指定上限
    > 上限extends：使用时指定的类型必须是继承某个类，或者实现某个接口，即<=   
    > `G<? extends A> 可以作为G<A>和G<B>的父类，其中B是A的子类`  
* 通配符指定下限
    > 下限super：使用时指定的类型不能小于操作的类，即>=  
    > `G<? super A> 可以作为G<A>和G<B>的父类，其中B是A的父类`  

```java
    @Test
    public void test4() {

        List<? extends Person> list1 = null;
        List<? super Person> list2 = null;

        List<Student> list3 = new ArrayList<Student>();
        List<Person> list4 = new ArrayList<Person>();
        List<Object> list5 = new ArrayList<Object>();

        list1 = list3;
        list1 = list4;
//        list1 = list5;//编译不通过

//        list2 = list3;//编译不通过
        list2 = list4;
        list2 = list5;

        //读取数据：
        list1 = list3;
        Person p = list1.get(0);

        //Student s = list1.get(0);//编译不通过

        list2 = list4;
        Object obj = list2.get(0);

//        Person obj = list2.get(0);//编译不通过

        //写入数据：
//        list1.add(new Student());//编译不通过

        //编译通过
        list2.add(new Person());
        list2.add(new Student());

    }
```
