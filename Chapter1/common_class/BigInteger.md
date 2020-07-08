# BigInteger
java.math 包的 BigInteger 可以表示不可变的任意精度的整数 。 BigInteger 提供所有 Java 的基本整数操作符的对应物，并提供 java.lang.Math 的所有相关方法。
另外， BigInteger 还提供以下运算：模算术、 GCD 计算、质数测试、素数生成、位操作以及一些其他操作。
* 构造器 
    > BigInteger(String val)
* 常用 方法
    > public BigInteger abs ()()：返回此 BigInteger 的绝对值的 BigInteger 。  
    > BigInteger add (BigInteger val) ：返回其值为 (this + val) 的 BigInteger  
    > BigInteger subtract (BigInteger val) ：返回其值为 (this val) 的 BigInteger  
    > BigInteger multiply (BigInteger val) ：返回其值为 (this * val) 的 BigInteger  
    > BigInteger divide (BigInteger val) ：返回其值为 (this/val) 的 BigInteger 整数相除只保留整数部分    
    > BigInteger remainder (BigInteger val) ：返回其值为 (this % val) 的 BigInteger  
    > BigInteger [] divideAndRemainder (BigInteger val)：返回包含 (this / val) 后跟(this % val) 的两个 BigInteger 的数组  
    > BigInteger pow (int exponent) ：返回其值为 (this exponent ) 的 BigInteger   