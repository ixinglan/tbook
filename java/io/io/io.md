# IO流原理及分类
* I/O是Input/Output的缩写， I/O技术是非常实用的技术，用于处理设备之间的数据传输。如读/写文件，网络通讯等。
* Java程序中，对于数据的输入/输出操作以“流(stream)” 的方式进行。
* java.io包下提供了各种“流”类和接口，用以获取不同种类的数据，并通过标准的方法输入或输出数据

* 输入input：读取外部数据（磁盘、光盘等存储设备的数据）到程序（内存）中。
* 输出output：将程序（内存）数据输出到磁盘、光盘等存储设备中。

## 分类
* 按操作数据单位不同分为：字节流(8 bit)，字符流(16 bit)
* 按数据流的流向不同分为：输入流，输出流
* 按流的角色的不同分为：节点流，处理流
    > 节点流: 直接从数据源或目的地读写数据  
    > 处理流: 不直接连接到数据源或目的地，而是“连接”在已存在的流（节点流或处理流）之上，通过对数据的处理为程序提供更为强大的读写功能

| 分类 | 字节输入流 | 字节输出流 | 字符输入流 | 字符输出流 |
| :--- | :--- | :--- | :--- | :--- |
| 抽象基类 | InputStream | OutputStream | Reader | Writer |
| 访问文件 | FileInputStream | FileOutputStream | FileReader | FileWriter |
| 访问数组 | ByteArrayInputStream | ByteArrayOutputStream | CharArrayReader | CharArrayWriter |
| 访问管道 | PipedInputStream | PipedOutputStream | PipedReader | PipedWriter |
| 访问字符串 |  |  | StringReader | StringWriter |
| 缓冲流 | BufferedInputStream | BufferedOutputStream | BufferedReader | BufferedWriter |
| 转换流 |  |  | InputStreamReader | OutputStreamWriter |
| 对象流 | ObjectInputStream | ObjectOutputStream |  |  |
|       | FilterInputStream | FilterOutputStream | FilterReader | FilterWriter |
| 打印流 |  | PrintStream |  | PrintWriter |
| 推回输入流 | PushbackStream |  | PushbackReader |  |
| 特殊流 | DataInputStream | DataOutputStream |  |  |

## InputStream
* `int read()`  
从输入流中读取数据的下一个字节。返回 0 到 255 范围内的 int 字节值。如果因为已经到达流末尾而没有可用的字节，则返回值 -1。 
* `int read(byte[] b)`  
从此输入流中将最多 b.length 个字节的数据读入一个 byte 数组中。如果因为已经到达流末尾而没有可用的字节，则返回值 -1。否则以整数形式返回实际读取的字节数。 
* `int read(byte[] b, int off,int len)`  
将输入流中最多 len 个数据字节读入 byte 数组。尝试读取 len 个字节，但读取的字节也可能小于该值。以整数形式返回实际读取的字节数。如果因为流位于文件末尾而没有可用的字节，则返回值 -1。 
* `public void close() throws IOException`  
关闭此输入流并释放与该流关联的所有系统资源。

## Reader
* `int read()`  
读取单个字符。作为整数读取的字符，范围在 0 到 65535 之间 (0x00-0xffff)（2个字节的Unicode码），如果已到达流的末尾，则返回 -1 
* `int read(char[] cbuf)`  
将字符读入数组。如果已到达流的末尾，则返回 -1。否则返回本次读取的字符数。 
* `int read(char[] cbuf,int off,int len)`  
将字符读入数组的某一部分。存到数组cbuf中，从off处开始存储，最多读len个字符。如果已到达流的末尾，则返回 -1。否则返回本次读取的字符数。 
* `public void close() throws IOException`  
关闭此输入流并释放与该流关联的所有系统资源。

## OutputStream
* `void write(int b)`  
将指定的字节写入此输出流。write 的常规协定是：向输出流写入一个字节。要写入的字节是参数 b 的八个低位。b 的 24 个高位将被忽略。 即写入0~255范围的。 
* `void write(byte[] b)`  
将 b.length 个字节从指定的 byte 数组写入此输出流。write(b) 的常规协定是：应该与调用 write(b, 0, b.length) 的效果完全相同。 
* `void write(byte[] b,int off,int len)`  
将指定 byte 数组中从偏移量 off 开始的 len 个字节写入此输出流。 
* `public void flush()throws IOException`  
刷新此输出流并强制写出所有缓冲的输出字节，调用此方法指示应将这些字节立即写入它们预期的目标。 
* `public void close() throws IOException`  
关闭此输出流并释放与该流关联的所有系统资源

## Writer
* `void write(int c)`  
写入单个字符。要写入的字符包含在给定整数值的 16 个低位中，16 高位被忽略。 即写入0 到 65535 之间的Unicode码。 
* `void write(char[] cbuf)`  
写入字符数组。 
* `void write(char[] cbuf,int off,int len)`  
写入字符数组的某一部分。从off开始，写入len个字符
* `void write(String str)`  
写入字符串。 
* `void write(String str,int off,int len)`  
写入字符串的某一部分。 
* `void flush()`  
刷新该流的缓冲，则立即将它们写入预期目标。 
* `public void close() throws IOException`  
关闭此输出流并释放与该流关联的所有系统资源