# 异常
Java语言中,程序执行中发生的不正常的情况
## 分类
* Error  
java虚拟机无法解决的严重问题,如:系统内部错误,资源耗尽等,StackOverFlow和OOM(OutOfMemory)
* Exception  
因编程错误和外在因素导致的一般性问题,可以用针对性的代码进行处理,如空指针,试图读取的文件不存在,网络连接中断,数组下标越界等

## Exception
* 编译时异常(checked)  
> IOException
>> FileNotFoundException 
>
> ClassNotFoundException  
> ......
* 运行时异常(unchecked)  
> NullPointerException  
> ArrayIndexOutOfBoundsException  
> ClassCastException  
> NumberFormatException  
> InputMismatchException  
> ArithmeticException   
> ......  

* 处理方法  
> try-catch-finally  
> throws

