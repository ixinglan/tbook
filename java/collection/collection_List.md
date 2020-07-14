# List
* 鉴于 Java 中数组用来存储数据 的局限性，我们通常使用 List 替代数组
* List 集合类中 元素有序、且可重复 ，集合中的每个元素都有其对应的顺序索引。
* List 容器中的元素都对应一个整数型的序号记载其在容器中的位置，可以根据序号存取容器中的元素。
* JDK API 中 List 接口的实现类常用的有： ArrayList 、 LinkedList 和 Vector
* 常用方法
    > void add(int index, Object ele) 在 index 位置插入 ele 元素  
    > boolean addAll (int index, Collection) eles 从 index 位置开始将 eles 中的所有元素添加进来  
    > Object get( int index): 获取指定 index 位置的元素  
    > int indexOf (Object obj) 返回 obj 在集合中首次出现的位置  
    > int lastIndexOf (Object obj 返回 obj 在当前集合中末次出现的位置  
    > Object remove( int index): 移除指定 index 位置的元素，并返回此元素  
    > Object set( int index, Object ele) 设置指定 index 位置的元素为 ele  
    > List subList int fromIndex , int toIndex) 返回从 fromIndex 到 toIndex 位置的子集合  

## ArrayList
ArrayList的源码分析：
* jdk 7情况下  
   ArrayList list = new ArrayList();//底层创建了长度是10的Object[]数组elementData  
   list.add(123);//elementData[0] = new Integer(123);  
   ...  
   list.add(11);//如果此次的添加导致底层elementData数组容量不够，则扩容。  
   默认情况下，扩容为原来的容量的1.5倍，同时需要将原有数组中的数据复制到新的数组中。  
   结论：建议开发中使用带参的构造器：ArrayList list = new ArrayList(int capacity)  
* jdk 8中ArrayList的变化：
   ArrayList list = new ArrayList();//底层Object[] elementData初始化为{}.并没有创建长度为10的数组  
   list.add(123);//第一次调用add()时，底层才创建了长度10的数组，并将数据123添加到elementData[0]  
   ...
   后续的添加和扩容操作与jdk 7 无异。  
* 小结：jdk7中的ArrayList的对象的创建类似于单例的饿汉式，而jdk8中的ArrayList的对象的创建类似于单例的懒汉式，延迟了数组的创建，节省内存  

## LinkedList
* LinkedList的源码分析：
   LinkedList list = new LinkedList(); 内部声明了Node类型的first和last属性，默认值为null  
   list.add(123);//将123封装到Node中，创建了Node对象。  

   其中，Node定义为：体现了LinkedList的双向链表的说法  
```java
   private static class Node<E> {  
       E item;  
       Node<E> next;  
       Node<E> prev;  

       Node(Node<E> prev, E element, Node<E> next) {  
           this.item = element;  
           this.next = next;  
           this.prev = prev;  
       }  
   } 
``` 

## Vector
jdk7和jdk8中通过Vector()构造器创建对象时，底层都创建了长度为10的数组。在扩容方面，默认扩容为原来的数组长度的2倍

## ArrayList 、 LinkedList 和 Vector之间的异同
* ArrayList 和 LinkedList 的 异同  
二者都线程不安全，相对线程安全的Vector ，执行效率高。此外，ArrayList 是实现了基于动态数组的数据结构， LinkedList 基于链表的数据结构。对于随机访问 get 和 set ArrayList 觉得优于 LinkedList ，因为 LinkedList 要移动指针。对于新增
和删除 操作 add( 特指 插入 和 remove LinkedList 比较占优势，因为 ArrayList 要移动数据。
* ArrayList 和 Vector 的区别  
Vector和 ArrayList 几乎是完全相同的 唯一的区别在于 Vector 是同步类 ( synchronized)，属于强同步类。因此开销就比 ArrayList 要大，访问要慢。正常情况下 大多数的 Java 程序员使用ArrayList 而不是 Vector, 因为同步完全可以由程序员自己来控制。 Vector 每次扩容请求其大
小的 2 倍空间，而 ArrayList 是 1.5 倍。 Vector 还有一个子 类 Stack
