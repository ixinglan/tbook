# jdk10

## 局部变量类型推断
```java
public class TenTest {
    @Test
    public void t1() {
        var num = 10;

        var list = new ArrayList<Integer>();
        list.add(1);
        list.add(2);
        list.add(3);

        for (var i : list) {
            System.out.println(i);
        }
    }

    @Test
    public void t2(){
        //局部变量不赋值,不能推断
//        var num;

        //lambda表达式,左边不能声明为var
        Supplier<Double> supplier = ()->Math.random();
//        var sup = ()->Math.random();

        //方法引用,左边不能为var
        Consumer<String> consumer = System.out::println;
//        var consumer = System.out::println;

        //数组的静态初始化,不能为var
        int[] arr = {1,2,3,4};
//        var arr2 = {1,2,3,4};
    }

    //方法的返回类型,方法参数,构造器参数,属性,catch块 都不能使用
}
```

## copyOf()
```java
    //集合中新增的copyOf(),用于创建一个只读的集合
    //如果本身是只读,则copyOf()是同一个,反之
    @Test
    public void t4(){
        var list1 = List.of(1,2,3);
        var list2 = List.copyOf(list1);
        System.out.println(list1 == list2);

        var list3 = new ArrayList<String>();
        var list4= List.copyOf(list3);
        System.out.println(list3==list4);
    }
```