# Map
* Map 与 Collection 并列存在。用于保存具有映射关系的数据 :key value
* Map 中的 key 和 value 都可以是任何引用类型的数据
* Map 中的 key 用 Set 来存放，不允许重复 ，即同一个 Map 对象所对应的类，须重写 hashCode 和 equals 方法
* 常用 String 类作为 Map 的“键”
* key 和 value 之间存在单向一对一关系，即通过指定的 key 总能找到唯一的、确定的 value
* Map 接口的常用实现类： HashMap 、 TreeMap 、 LinkedHashMap 和Properties 。 其中， HashMap 是 Map 接口使用频率最高的实现类

## 常用方法
* 添加,删除,修改
    > Object put(Object key, Object value)  
    > void putAll(Map m)  
    > Object remove(Object key)  
    > Object clear()  
* 查询
    > Object get(Object key)  
    > boolean containsKey(Object key)  
    > boolean containsValue(Object value)  
    > int size()  
    > boolean isEmpty()  
    > boolean equals(Object obj)  
* 无视图操作
    > Set keySet() --返回所有key构成的Set集合  
    > Collection values() --返回所有value构成的Collection集合  
    > Set entrySet() --返回所有key-value构成的Set集合  

## HashMap
* 存储结构
    > 1.7 数组+链表  
    > 1.8 数组+链表+红黑树  
* 实例化
    > 1.7 底层创建了长度是16的一维数组 Entry[] table  
    > 1.8 底层没有创建一个长度为16的数组(jdk8底层数组是Node[] ,而不是Entry[]),首次put时创建  
* put(k,v)
    > 首先,调用k所在类hashCode()计算k的 hash值,此hash值经过某种算法计算以后,得到Entry数组中的存放位置  
    > 如果此位置数据为空,则k-v添加成功  
    > 如果此位置数据不为空(存在1个或多个数据以链表形式存在),则比较k的hash值  
          >> 如果hash值不相同,则k-v添加成功  
          >> 如果hash值不相同,则通过k的equals方法比较  
                >>> 如果equals() 返回false, 则k-v添加成功  
                >>> 如果equals() 返回true, 则使用v替换相同k的value值  

    > 1.7: 在不断扩容过程中,当超过临界值且该位置不为Null,默认的扩容方式:扩容为原来容量的2位,并把旧的数据copy过来   
    > 1.8: 当数组的某一个索引位置上的元素以链表形式存在的 数据个数>8 && 当前数组长度>64时,此时索引位置上的所有数据改为使用红黑树存储  
* 源码关键参数
    > DEFAULT_INITIAL_CAPACITY : HashMap的默认容量，16  
    > DEFAULT_LOAD_FACTOR：HashMap的默认加载因子：0.75  
    > threshold：扩容的临界值，=容量填充因子：16  0.75 => 12  
    > TREEIFY_THRESHOLD：Bucket中链表长度大于该默认值，转化为红黑树:8  
    > MIN_TREEIFY_CAPACITY：桶中的Node被树化时最小的hash表容量:64  
* 加载因子的大小影响
    > 负载 因子的大小决定了 HashMap 的数据密度。  
    > 负载因子越大密度越大，发生碰撞的几率越高，数组中的链表越容易 长造成 查询或插入时的比较次数增多，性能会下降。  
    > 负载因子越小，就越容易触发扩容，数据密度也越小，意味着发生碰撞的几率越小，数组中的链表也就越短，查询和插入时比较的次数也越小，性能会更高。但是会浪费一定的内容空间。而且经常扩容也会影响性能，建议初始化预设大一点的空间。  
    > 按照其他语言的参考及研究经验，会考虑将负载因子设置为 0.7~0.75 ，此时平均检索长度接近于常数  

## LinkedHashMap extends HashMap
* LinkedHashMap 是 HashMap 的 子类  
* 在 HashMap 存储结构的基础上，使用了一对双向链表来记录添加元素的顺序  
    ```
    static class Entry<K,V> extends HashMap.Node<K,V> {
        Entry<K,V> before, after;
        Entry(int hash, K key, V value, Node<K,V> next) {
            super(hash, key, value, next);
        }
    }
    ```
* 与 LinkedHashSet 类似 LinkedHashMap 可以维护 Map 的迭代顺序：迭代顺序与 Key V alue 对的插入顺序一致  

```java
public class MapTest2 {
    @Test
    public void t2(){
        LinkedHashMap map = new LinkedHashMap();
        map.put(111, 1);
        map.put(222, 2);
        map.put(333, 3);

        System.out.println(map);//{111=1, 222=2, 333=3}
    }

    @Test
    public void t1(){
        HashMap map = new HashMap();
        map.put(111, 1);
        map.put(222, 2);
        map.put(333, 3);

        System.out.println(map);//{333=3, 222=2, 111=1}
    }
}
```

## TreeMap
* TreeMap 存储 Key Value 对时，需要根据 key value 对进行排序。TreeMap 可以保证所有的 Key Value 对处于有序状态 。
* TreeSet 底层使用红黑树结构存储数据
* TreeMap 的 Key 的排序：
    > 自然排序 TreeMap 的所有的 Key 必须实现 Comparable 接口，而且所有的 Key 应该是同一个类的对象，否则将会抛出 ClasssCastException
    > 定制排序 ：创建 TreeMap 时，传入一个 Comparator 对象，该对象负责对TreeMap 中的所有 key 进行排序。此时不需要 Map 的 Key 实现Comparable 接口
* TreeMap 判断 两个 key 相等的标准 ：两个 key 通过 compareTo() 方法或者 compare() 方法返回 0

```java
/**
 * 向TreeMap中添加key-value，要求key必须是由同一个类创建的对象
 * 因为要按照key进行排序：自然排序 、定制排序
 */
public class TreeMapTest {
    //定制排序
    @Test
    public void test2(){
        TreeMap map = new TreeMap(new Comparator() {
            @Override
            public int compare(Object o1, Object o2) {
                if(o1 instanceof User && o2 instanceof User){
                    User u1 = (User)o1;
                    User u2 = (User)o2;
                    return Integer.compare(u1.getAge(),u2.getAge());
                }
                throw new RuntimeException("输入的类型不匹配！");
            }
        });
        User u1 = new User("Tom",23);
        User u2 = new User("Jerry",32);
        User u3 = new User("Jack",20);
        User u4 = new User("Rose",18);

        map.put(u1,98);
        map.put(u2,89);
        map.put(u3,76);
        map.put(u4,100);

        Set entrySet = map.entrySet();
        Iterator iterator1 = entrySet.iterator();
        while (iterator1.hasNext()){
            Object obj = iterator1.next();
            Map.Entry entry = (Map.Entry) obj;
            System.out.println(entry.getKey() + "---->" + entry.getValue());

        }
    }

    //自然排序
    @Test
    public void test1(){
        TreeMap map = new TreeMap();
        User u1 = new User("Tom",23);
        User u2 = new User("Jerry",32);
        User u3 = new User("Jack",20);
        User u4 = new User("Rose",18);

        map.put(u1,98);
        map.put(u2,89);
        map.put(u3,76);
        map.put(u4,100);

        Set entrySet = map.entrySet();
        Iterator iterator1 = entrySet.iterator();
        while (iterator1.hasNext()){
            Object obj = iterator1.next();
            Map.Entry entry = (Map.Entry) obj;
            System.out.println(entry.getKey() + "---->" + entry.getValue());

        }
    }
}
```
## HashTable
* Hashtable 是个旧的 Map 实现类，JDK1.0 就提供了。不同于 HashMap,Hashtable 是线程安全的。
* Hashtable 实现原理和 HashMap 相同，功能相同。底层都使用哈希表结构，查询速度快，很多情况下可以互用 。
* 与 HashMap 不同， Hashtable 不允许使用 null 作为 key 和 value
* 与 HashMap 一样， Hashtable 也不能保证其中 Key Value 对的顺序
* Hashtable 判断两个 key 相等、两个 value 相等的标准与 HashMap一致

## Properties extends HashTable
* Properties 类是 Hashtable 的子类，该对象用于处理属性文件
* 由于属性文件里的 key、value 都是字符串类型，所以 Properties 里的 key和 value 都是字符串类型
* 存取数据时，建议使用 setProperty (String key,String value) 方法和getProperty (String) 方法

```java
/**
 * 处理配置文件
 * 
 * map包下放test.properties文件
 * name=zhjq
 * sex=female
 * age=18
 */
public class PropertiesTest {
    public static void main(String[] args) throws IOException {
        Properties properties = new Properties();
        properties.load(new FileInputStream("src/collection/map/test.properties"));
        String name = properties.getProperty("name");
        System.out.println(name);

        Set<Map.Entry<Object, Object>> entries = properties.entrySet();
        for (Map.Entry<Object, Object> entry : entries) {
            System.out.println(entry.getKey()+":"+entry.getValue());
        }
    }
}
```
