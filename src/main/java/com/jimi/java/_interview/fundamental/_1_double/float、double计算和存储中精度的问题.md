## float和double型运算时精度的问题
 
浮点数是无法在计算机中准确表示的，例如0.1在计算机中只是表示成了一个近似值，因此，对付点数的运算时结果具有不可预知性。在进行数字运算时，如果有double或float类型的浮点数参与计算，偶尔会出现计算不准确的情况。所以一般不要用float和double型变量做简单运算，会出现精度的问题。
如下
```java
0.1d+0.7d=0.7999999999999999;
0.2d+0.8d=1.0;
```

### 原因
计算机并不能识别除了二进制数据以外的任何数据。无论我们使用何种编程语言，在何种编译环境下工作，都要先把源程序翻译成二进制的机器码后才能被计算机识别。如源程序里的0.8是十进制的，计算机不能直接识别，要先编译成二进制。但问题来了，0.8的二进制表示并非是精确的0.8，反而最为接近的二进制表示是0.7999999999999999。

### 解决方案
《Effective Java》中提到一个原则，那就是float和double只能用来作科学计算或者是工程计算，但在商业计算中我们要用java.math.BigDecimal。

```java
BigDecimal b1=new BigDecimal(Double.toString(0.1));  
BigDecimal b2=new BigDecimal(Double.toString(0.7));  
System.out.println(b1.add(b2).doubleValue());  
```

**注意**
BigDecimal 也有一些令人奇怪的行为。尤其在使用 equals() 方法来检测数值之间是否相等时要小心。 equals() 方法认为，两个表示同一个数但换算值不同（例如， 100.00 和 100.000 ）的 BigDecimal 值是不相等的。然而， compareTo() 方法会认为这两个数是相等的，所以 **在从数值上比较两个 BigDecimal 值时，应该使用 compareTo() 而不是 equals()** 。

### 在mysql中如何存储浮点数

参考[Mysql中如何存储浮点数.md](com/jimi/java/_interview/mysql/Mysql中如何存储浮点数.md)

#### MySQL的浮点数类型和定点数类型如下表所示：

|类型名称|字节数|负数的取值范围|非负数的取值范围|
|---|---|---|---|
|FLOAT	|4	|-3.402823466E+38～-1.175494351E-38	|0和1.175494351E-38～3.402823466E+38|
|DOUBLE	|8	|-1.7976931348623157E+308～-2.2250738585072014E-308	|0和2.2250738585072014E-308～1.7976931348623157E+308|
|DECIMAL(M,D)或DEC(M,D)	|M+2	|同DOUBLE型	|同DOUBLE型|
从上表中可以看出，DECIMAL型的取值范围与DOUBLE相同。但是，DECIMAL的有效取值范围由M和D决定，而且DECIMAL型的字节数是M+2，也就是说，定点数的存储空间是根据其精度决定的。

### 浮点数和定点数区别
相对于浮点数的定点数（Fixed Point Number）。在这种表达方式中，小数点固定的位于实数所有数字中间的某个位置。货币的表达就可以使用这种方式，比如 99.00 或者 00.99 可以用于表达具有四位精度（Precision），小数点后有两位的货币值。由于小数点位置固定，所以可以直接用四位数值来表达相应的数值。
定点数表达法的缺点在于其形式过于僵硬，固定的小数点位置决定了固定位数的整数部分和小数部分，不利于同时表达特别大的数或者特别小的数。最终，绝大多数现代的计算机系统采纳了所谓的浮点数表达方式。这种表达方式利用科学计数法来表达实数，即用一个尾数（Mantissa ），一个基数（Base），一个指数（Exponent）以及一个表示正负的符号来表达实数。比如 123.45 用十进制科学计数法可以表达为 1.2345 × 102 ，其中 1.2345 为尾数，10 为基数，2 为指数。浮点数利用指数达到了浮动小数点的效果，从而可以灵活地表达更大范围的实数。
