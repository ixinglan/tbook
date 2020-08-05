https://www.w3cschool.cn/architectroad/ 架构师之路
	1 severlet 生命周期 spring ioc aop怎么实现拦截代理 spring mvc请求流程  


	2 定时任务的实现原理  jvm调优(参考链接：https://www.cnblogs.com/andy-zhou/p/5327288.html) 


	3 dubbo与spring cloud区别 Hystrix 
              springcloud 相关组件及负载均衡算法  zuul filter 
             数据库索引类型  innodb myisam区别  乐观锁与悲观锁使用


	4 dubbo服务治理 


	5 常见注解，spring 


	6 分布式项目原理图 


	7 jvm优化常用工具 jvm调优参数哪些


	8 有序集合与无序集合的增删查找的时间复杂度 


	9 jdk8的新特性 

Lambda 表达式 − Lambda允许把函数作为一个方法的参数（函数作为参数传递进方法中。
方法引用 − 方法引用提供了非常有用的语法，可以直接引用已有Java类或对象（实例）的方法或构造器。与lambda联合使用，方法引用可以使语言的构造更紧凑简洁，减少冗余代码。
默认方法 − 默认方法就是一个在接口里面有了一个实现的方法。
Optional 类 − Optional 类已经成为 Java 8 类库的一部分，用来解决空指针异常。
Nashorn, JavaScript 引擎 − Java 8提供了一个新的Nashorn javascript引擎，它允许我们在JVM上运行特定的javascript应用。
新工具 − 新的编译工具，如：Nashorn引擎 jjs、 类依赖分析器jdeps。
Stream API −新添加的Stream API（java.util.stream） 把真正的函数式编程风格引入到Java中。
Date Time API − 加强对日期与时间的处理。

	11 concurrentHashMap和HashTable 


	12 HashMap和HashTable 


	13 string stringbuffer bulder 


	14 zk 有那些作用，用在什么场景， 


	  命名服务。配置管理，集群管理。分布式锁，队列    分布式协调 分布式锁 元数据/配置信息管理  HA高可用性


	15 zk 服务怎么发现的，分布式主键原理，
              UUID 雪花算法 redis 阿里-TDDL序列生成方式
	16 分布式某一个方法调用许多服务，某一个服务挂了怎么办。 

             可靠消息最终一致性，TCC，最大努力通知
	17 分布式服务某一台qps太多，怎么处理。 


	18 怎么分库分表，分库分表后怎么分页排序 


	19 volaicte 可见性，怎么实现的  思考 


	20 分布式应用接口测试200万没问题，正式环境60s就不行了 


	21 秒杀怎么设计实现的 


	22 类加载与双亲委派 


	23 lock与synchronized 


	24 lock 实现原理 


	25 current 包下 CountDownLatch 


	26 阻塞队列主要用与什么场景 


	27 事务嵌套怎么处理，事务传播行为有那些，对他的理解 


	    1----->@Transactional(propagation=Propagation.REQUIRED) ： 


	     内外层方法共用外层方法的事务 


	    2----->@Transactional(propagation=Propagation.REQUIRES_NEW) ： 


	    当执行内层被调用方法时，外层方法的事务会挂起。两个事务相互独立，不会相互影响。 


	    3----->@Transactional(propagation=Propagation.NESTED) : 


	    理解Nested的关键是savepoint。他与PROPAGATION_REQUIRES_NEW的区别是，PROPAGATION_REQUIRES_NEW另起一个事务，将会与他的父事务    相互独立， 


	     而Nested的事务和他的父事务是相依的，他的提交是要等和他的父事务一块提交的。也就是说，如果父事务最后回滚，他也要回滚的。 


	



	28 说说NIO 



	28   小程序授权过程 

	29 说说hashmap时间复杂度，怎么扩容的  思考 


	30 索引失效的场景，举例说明 


	31 jvm 调优，dump文件导出 


	32 mq使用 


	33 本地方法栈与栈的区别 


	34 分布式锁的使用（https://blog.csdn.net/xlgen157387/article/details/79036337分布式锁使用） 


	35 jvm调优命令 


	36 分布式缓存系统设计 


	    数据分片、数据存储类型、失效时间、io/cpu利用、容错机制、负载、剔除算法LRU、多线程、协议简单、分布式锁的使用、数据持久化与备份、主从复制 


	37 B树结构图 根节点及叶子存储什么 


	38 cms回收算法每个阶段具体说明 


	39 meacheca 与redis区别 


	40 redis3.2.3集群挂了怎么办， mq3.4.1集群挂了怎么办 


	41 分布式锁使用场景具体说明 


	42 redis批量拉取命令 


	43 mysql 数据同步延迟怎么处理,为什么延迟 


	44 数据库in会不会导致索引失效，为什么 


	45 hashmap为什么采用数组加链表 


	     数组的好处是可以根据下标快速的找到对应的元素，而链表的好处是只用知道插入位置的前后，不需要一个一个的位置。这样就提高了插入的速度或者删   除的速度 


	   提高了查找的速度 也提高了增删的速度 


	46 concurrenthashmap 为什么采取分段锁(HashMap Hashtable LinkedHashMap 和TreeMap) 


	47 事务的隔离级别、特性，传播行为 


	48 事务的传播行为默认requeid 怎么发现同属一个事务 


	49 为啥要分老年代，年轻代 


	：控制内存管理，减少gc时间，处理引用类型的实例化，提高效率 


	  就是优化GC性能 


	



	50 生产者和消费者解决了什么问题 削峰 解耦 异步 


	51 线程几种状态， 线程池的原理 大小、核数、时间 


	52 多线程如何通信(同步、wait/notify机制、)， 跨jvm如何通信？(共享内存机制redis和消息通信机制mq) 


	      wait/notify  synos   共享内存与消息通道 


	54 jar包冲突怎么解决，怎么查看依赖关系 


	55 如何保证幂等性 


	56 分库分表策略 


	     业务拆分、主从复制，数据库分库与分表 hash取模运算 


	57 volatile 原理如何保证可见性：CAS（比较与交换）、内存屏障指令重排序  cas ABA问题可以用版本号解决 


	58 数据库B+树 


	    用B+树的原因其实还是想让一个节点中包含更多的关键字，B+树把数据域都放到叶子节点上，这样内部节点只剩关键字了，这样一个节点（一个磁盘页）就能包含更多的关键字了，降低树深，减少读写磁盘次数 


	59 redis 实现原理 数据结构 


	     简单字符串、链表、字典、跳跃表、整数集合、压缩列表 


	60 ioc流程 aop流程 


	61 线程池原理流程及各个参数意义 


	62 线上sql慢查询定位 


	63 mysql数据同步数据延迟原因及怎么解决 


	



	64 手写实现hashmap 


	65 手写实现阻塞队列 


	66 redis 哨兵 


	67 搜索分词是怎么实现的 


	68 异地机房设计 


	69 分布式事务常用的解决方案的优缺点是什么？适用于什么场景？ 


	    XA资源横向扩展、TCC不仅可以横向扩展还可以用于数据分片(表的拆分)交易、支付、账务 


	   参考链接：https://www.jianshu.com/p/9e2670641119 


	70 分布式事务出现的原因？用来解决什么痛点？ 


	71 分布式事务两阶段提交，超时的影响，为什么要用MQ消息服务呢，目的就是消息补偿，消息的持久化和重试提供功能 


	数据库ACID:原子性、一致性、隔离性、持久化 两阶段提交 三阶段提交 base补偿机制saga//// 


	  


	72 类的加载顺序 


	      1、虚拟机在首次加载Java类时，会对静态初始化块、静态成员变量、静态方法进行一次初始化 


	      2、只有在调用new方法时才会创建类的实例 


	      3、类实例创建过程：按照父子继承关系进行初始化，首先执行父类的初始化块部分，然后是父类的构造方法；再执行本类继承的子类的初          始化块，最后是子类的构造方法 


	      4、类实例销毁时候，首先销毁子类部分，再销毁父类部分 


	73 谈谈设计模式：手写单例模式、观察者模式、工厂模式、动态代理模式 


	74  jvm原理 


	75  linux 常用命令：如 查看日志、查看文件关键字、查询端口占用情况、查看进程pid 


	76  rest服务优缺点 


	      优点：1 简单，轻量级  2 性能好 3 稳定可靠 4 易于开发维护 


	77 mysql 索引的应用 


	   https://www.zhihu.com/question/36996520 


	78 有如何解决慢查询 


	   a 页面静态化 b sql优化 子查询、嵌套查询  c redis d 分库分表、垂直水平拆分 


	79 redis和Memcached区别 


	    内存：redis 写入磁盘 memcached 内存 


	    IO：redis 单线程io复用模型 memcached 多线程io复用模型 


	    数据一致：redis 提供事务 memcached cas命令 可以保证多个并发 


	   支持类型： redis keyvalue list set hash 等 


	80  分布式应用数据一致性的处理 


	81 String hashmap的原理，数据结构 


	82 怎么开始就加载servlet 


	83 compareAndSet CAS 


	84 负载均衡：DNS F5 软负载 


	85 限流：单应用限流：令牌桶和漏桶 分布式应用：redis_lua 或nginx+lua 


	86 分布式事务：事务表、消息队列、补偿机制(执行、回滚)、TCC模式(预占、确认、取消)、 Sagas（拆分事务+补偿机制）来保证最终一致性 


	87 订单流程： 提交订单--扣减优惠券--扣减库存-保存订单 否则回滚优惠券和库存 


	88 系统压测：压测方案(压测接口、并发量、压测策略(突发、逐步加压、并发量)) 压测指标(机器负载、QPS/TPS、响应时间)，产出物：压测报告，进行系统优化和容灾 TCPCopy复制线上真实流量 


	89 分布式主键实现思路


	90 什么是不可变对象。不可变对象有什么好处。在什么情景下使用它， 


	91 多线程面试题：https://blog.csdn.net/yanpenglei/article/details/79603467 


	92 基础知识常见面试题：https://mp.weixin.qq.com/s?__biz=MzA3MTUzOTcxOQ==&mid=2452964547&idx=1&sn=ffd4eee6536356a79884b104a81d00b9&chksm=88ede9abbf9a60bd065d14f8e0c962aa817249409698075ccb619c9abc7f0ec908600a7fca7b&scene=21#wechat_redirect 


94 rabbitmq kabka activemq区别
93 redis分布式是如何寻址的，哨兵与集群的区别

95 rabbitmq工作流程
96 分布式key如果保证唯一
97 redis分布式锁如何处理过期时间
98 euraka与dubbo注册中心区别
99 数据库索引失效场景
100 mybatis 一级与二级缓存
101 多个节点怎么控制某一个用户的并发
102 RabbitMQ 消息顺序、消息幂等、消息重复、消息事务、集群 
103 mysql索引 btree hash
104 threadlocal 使用场景
105 mq 消费者挂了，怎么同步消息
106 eureka与consul区别
107 支付如何保持幂等和加密
108 spring bean 的生命周期
109 mysql  表锁与行锁
110 分布式事务一般怎么控制方案
111 rabbitmq 如何保证消息的幂等性，顺序性，重复消费问题，rabbitmq宕机了，生产者，消费者怎么处理？？
112 接口与抽象类
113 序列化与反序列化
114 字节流与字符流的区别
115 怎么设计jvm
116 zk顺序写 
117线程池工作队列类型，参数判断
118 数据库聚集索引与非聚集索引区别
119 分布式订单优惠券库存处理流程
120 explain mysql 结果分析各个参数意义
121 二分查找，快排，冒泡，算法
122 redis rabbitMq如何保证高可用
123 arraylist 与linklist区别 及如何扩容
124Java中hashSet与treeSet的去重原理
125 java 最小堆最大堆怎么设置及oom 现象如何解决、内存泄露与内存溢出
126 java 深拷贝与浅拷贝  值引用参数引用
127 redis rdb的工作流程，为什么采取单线程呢， hash set的区别
128 mongodb 的数据存储
129 spring boot常见面试 启动流程、配置、ribbon/fein 用户登录授权
130 深拷贝与浅拷贝  值传递与引用传递
131 redis 雪崩怎么解决
132 数据库怎么sql调优慢  数据库分页调优  
133 topk 最小堆流程处理文件
134 spring 初始化数据几种方式 135 innodb myisam 区别 136 聚族索引与非聚族索引区别 137 导致索引失效的原因
138 行锁变表锁mysql原因  139 mysql 普通索引流程，几种索引类型 140 索引类型 141 redis 内存慢了会怎么样？
142 mysql gap锁造成死锁 143 如何避免死锁
144 euraka协议 config服务端客户端流程   145 pagehelper分页原理  拦截器   分页优化   
145 hashmap 为什么是2次幂  147 幂等处理 
148 mysql like 最左原则原理  149 euraka 心跳的实现 150 引起脑裂现象  151 mvc 返回json几种方式
152 redis 数据库底层实现原理  153 线程池线程结束后为什么不用手动创建
154 怎么确定线程池的最大，核心线程量
155 mysql 死锁的场景  157 mysql in含有null 158 mysql trastion的使用 159 确保集合不被修改
160 怎么确保一个集合不能被修改
161 HashMap与ConcurrentHashMap的区别
155 分布式session使用
162 线程池的原理 163 spring cloud 注册中心 断路器：熔断降级
164 核心线程数设置怎么 
165  事务隔离级别 幻读、脏读，可重复读  165 mysql执行原理 16 spring 注入循环引用
166 三次握手四次握手  
167 如何保证消息的可靠性
服务限流 

    前端ip/线程隔离，可以通过线程数 + 队列大小限制/如果是信号量隔离，可以设置最大并发请求数/qps 服务限制 

   令牌桶法 单应用限流：令牌桶和漏桶 分布式应用：redis_lua 或nginx+lua 
 


	深入学习Redis高可用架构：哨兵原理及实践(https://www.javazhiyin.com/18137.html) 


	电商常见面试问题https://blog.csdn.net/qq_39924485/article/details/79949232 


	多线程面试题：https://blog.csdn.net/seu_calvin/article/details/52411531 


	异地多活方案：http://www.cnblogs.com/jaychan/p/9242325.html?utm_source=debugrun&utm_medium=referral

 


	



	



	



	





