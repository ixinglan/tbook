# Collection

## 常用方法
* add()
* addAll()
* int size()
* void clear()
* boolean isEmpty()
* boolean contains(Object obj) --通过equals方法判断
* boolean containsAll(Collection c) --通过equals方法判断
* boolean remove(Object obj) --通过equals方法判断并删除找到的第一个
* boolean removeAll(Collection c) --取差集
* boolean retainAll(Collection c) --取交集
* boolean equals(Object obj) --集合是否相等
* Object[] toAarray() --转成对象数据
* hashCode() --获取hash值
* iterator() --遍历

## Iterator遍历
* Iterator 对象称为迭代器 设计模式的一种 ))，主要用于遍历 Collection 集合中的元素
* GOF 给迭代器模式的定义为：提供一种方法访问一个容器 ( 对象中各个元素，而又不需暴露该对象的内部细节。迭代器模式，就是为容器而生。类似于“公交车上的售票员”、“火车上的乘务员”、 “空姐
* Collection 接口继承了 java.lang.Iterable 接口，该接口有一个 iterator() 方法，那么所有实现了 Collection 接口的集合类都有一个 iterator() 方法，用以返回一个实现了Iterator 接口的对象
* Iterator 仅用于遍历集合 Iterator 本身并不提供承装对象的能力。如果需要 创建Iterator 对象，则必须有一个被迭代的集合
* 集合对象每次调用 iterator() 方法都得到一个全新的迭代器对象 ，默认游标都在集合的第一个元素之前

### 遍历

```java
public class CollectionTest {
    @Test
    public void t2(){
        //错误写法
        Collection collection = new ArrayList();
        collection.add(123);
        collection.add("abc");
        collection.add(false);
        collection.add(456);

        //错误1
        Iterator iterator = collection.iterator();
        while (iterator.next() != null){
            System.out.println(iterator.next());
        }

        //错误2: 每次iterator都是一个新的Iterator迭代器
        while (collection.iterator().hasNext()){
            System.out.println(collection.iterator().next());
        }
    }

    @Test
    public void t1(){
        Collection collection = new ArrayList();
        collection.add(123);
        collection.add("abc");
        collection.add(false);
        collection.add(456);

        Iterator iterator = collection.iterator();
//        System.out.println(iterator.next());
//        System.out.println(iterator.next());
//        System.out.println(iterator.next());
//        System.out.println(iterator.next());

        while (iterator.hasNext()){
            System.out.println(iterator.next());
        }
    }
}
```

### 移除 remove()
* Iterator 可以删除集合的元素 但是是遍历过程中通过迭代器对象的 remove 方法 不是集合对象的 remove 方法 。
* 如果还未调用 next() 或在上一次调用 next 方法之后已经调用了 remove 方法再调用 remove 都会报 IllegalStateException

```java
public class CollectionTest2 {
    @Test
    public void t1(){
        Collection collection = new ArrayList();
        collection.add(123);
        collection.add("abc");
        collection.add(false);
        collection.add(456);

        Iterator iterator = collection.iterator();
        while (iterator.hasNext()){
            Object next = iterator.next();
            if(next.equals(123)){
                iterator.remove();
            }
        }

        Iterator iterator2 = collection.iterator();
        while (iterator2.hasNext()){
            System.out.println(iterator2.next());
        }
    }
}
```

### foreach() 增强for循环
* 5.0新增, 底层调用iterator()

## Collections
操作Collection、Map的工具类  
1.操作排序  
* reverse(List)：反转 List 中元素的顺序
* shuffle(List)：对 List 集合元素进行随机排序
* sort(List)：根据元素的自然顺序对指定 List 集合元素按升序排序
* sort(List，Comparator)：根据指定的 Comparator 产生的顺序对 List 集合元素进行排序
* swap(List，int， int)：将指定 list 集合中的 i 处元素和 j 处元素进行交换

2.查找,替换
* Object max(Collection)：根据元素的自然顺序，返回给定集合中的最大元素
* Object max(Collection，Comparator)：根据 Comparator 指定的顺序，返回给定集合中的最大元素
* Object min(Collection)
* Object min(Collection，Comparator)
* int frequency(Collection，Object)：返回指定集合中指定元素的出现次数
* void copy(List dest,List src)：将src中的内容复制到dest中
* boolean replaceAll(List list，Object oldVal，Object newVal)：使用新值替换 List 对象的所有旧值

3.同步控制  
Collections 类中提供了多个 synchronizedXxx() 方法，该方法可使将指定集合包装成线程同步的集合，从而可以解决多线程并发访问集合时的线程安全问题  
* List list = Collections.synchronizedList(list);
* Map map = Collections.synchronizedMap(list);
* ...

```java
public class CollectionsTest {
    @Test
    public void test2(){
        List list = new ArrayList();
        list.add(123);
        list.add(43);
        list.add(765);
        list.add(-97);
        list.add(0);

        //报异常：IndexOutOfBoundsException("Source does not fit in dest")
//        List dest = new ArrayList();
//        Collections.copy(dest,list);
        //正确的：
        List dest = Arrays.asList(new Object[list.size()]);
        System.out.println(dest.size());//list.size();
        Collections.copy(dest,list);
        System.out.println(dest);

        /*
        Collections 类中提供了多个 synchronizedXxx() 方法，
        该方法可使将指定集合包装成线程同步的集合，从而可以解决
        多线程并发访问集合时的线程安全问题
         */
        //返回的list1即为线程安全的List
        List list1 = Collections.synchronizedList(list);


    }

    @Test
    public void test1(){
        List list = new ArrayList();
        list.add(123);
        list.add(43);
        list.add(765);
        list.add(765);
        list.add(765);
        list.add(-97);
        list.add(0);
        System.out.println(list);

//        Collections.reverse(list);
//        Collections.shuffle(list);
//        Collections.sort(list);
//        Collections.swap(list,1,2);
        int frequency = Collections.frequency(list, 123);

        System.out.println(list);
        System.out.println(frequency);

    }
}
```


























