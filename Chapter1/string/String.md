# String
字符串,是一个final类,不可变字符序列,字符内容存储在一个字符数组value[]中
* 不可变性
* char[]存储

## 实例化方式
* 字面量
* 构造器new

## 常用方法
...略过

# StringBuffer
* 可变
* 线程安全,效率低
* char[]存储
* 默认capacity为16,不够时会继续扩容,默认扩容为capacity<<1 + 2,并将原有元素复制到新数组中

## 常用方法
...略过

# StringBuilder
* 可变
* 线程不安全,效率高
* jdk5.0新增
* char[]存储

## 常用方法
...略过

> 建议:
> 使用 StringBuffer(int capacity) 或 StringBuilder(int capacity)
