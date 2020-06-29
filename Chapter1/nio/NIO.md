# NIO 介绍
Java NIO（New IO）是从Java 1.4版本开始引入的一个新的IO API,可以替代标准的Java IO API.  
NIO与原来的IO有同样的作用和目的,但是使用的方式完全不同,NIO支持面向缓冲区的、基于通道的IO操作.  
NIO将以更加高效的方式进行文件的读写操作.
# NIO 与 IO 的主要区别
| IO | NIO |
| :--- | :---|
| 面向流(Stream Oriented) | 面向缓冲区(Buffer Oriented) |
| 阻塞IO(Blocking IO) | 非阻塞IO(NonBlocking IO) |
| 无 | 选择器(Selectors) |
# 通道与缓冲区
Java NIO系统的核心在于：通道(Channel)和缓冲区(Buffer)。通道表示打开到IO 设备(例如：文件、套接字)的连接。  
若需要使用NIO 系统，需要获取用于连接IO 设备的通道以及用于容纳数据的缓冲区。然后操作缓冲区，对数据进行处理.  
即channel负责传输, buffer负责存储




