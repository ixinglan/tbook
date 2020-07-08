# BigDecimal
* 一般的 Float 类和 Double 类可以用来做科学计算或工程计算，但在 商业计算中，要求数字精度比较高，故用到 java.math.BigDecimal 类
* BigDecimal 类支持不可变的、任意精度的有符号十进制定点数
* 构造器
    > public BigDecimal(double val)  
    > public BigDecimal (String val)
* 常用方法
    > public BigDecimal add (BigDecimal)  
    > public BigDecimal subtract (BigDecimal)  
    > public BigDecimal multiply (BigDecimal)  
    > public BigDecimal divide (BigDecimal divisor, int scale, int roundingMode)