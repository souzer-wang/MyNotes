# 什么是包装类

JAVA为基本数据类型提供了对应的类，这些类被称作**包装类**

数据的包装类可以提供该类最大、最小值查阅和类型转化的功能

对应关系如下：

|基本数据类型|包装类|
|:-:|:-:|
|byte|Byte|
|short|Short|
|int|Integer|
|long|Long|
|float|Float|
|double|Double|
|char|Character|
|boolean|Boolean|

# 为什么需要包装类

1. 包装类中封装了一些很实用的方法和变量

例如：Byte.MIN\_VALUE是Byte类中的一个常量，存放了byte类型数据的最小值。

2. 包装类在集合中来定义集合元素的类型

举例： Map<Integer,Integer>

# 包装类的常用方法和常量

1. Integer.MIN\_VALUE:int类型的最小值:-2^31  

2. Integer.MAX\_VALUE:int类型的最大值：2^31-1  

3. int Integer.parseInt(String sInteger);  
作用：将字符串类型的整数转换为int类型的数据。  

4. String Integer.toBinaryString(int value);  
作用：将十进制数转换为二进制，返回结果String类型。  

5. String Integer.toHexString(int value);  
作用：将十进制数转换为十六进制,返回结果String类型