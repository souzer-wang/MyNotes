|String|StringBuffer|StringBuilder|
|:-:|:-:|:-:|
|String的值是不可变的，所以每次对String的操作都会产生新的String对象。这样的话效率低下，且浪费大量的内存|StringBuffer是可变类，线程安全的字符操作类，任何它指向的字符操作都不会产生新的对象。每个BufferString对象都有一定的缓冲区容量，当字符串的大小没有超出容量时，不会分配新的容量；当字符串大小超出容量时，会自动增加容量|可变类，速度更快|
|不可变|可变|可变|
||线程安全|线程不安全|
||多线程操作字符串|单线程操作字符串|


## String

先说 String， 它和其它两个主要的区别在于它 不可变，而其它两个都是可变的。

具体是怎么样个 **不可变** 呢？

当我们每次对 String 类型的变量进行赋值的时候，其实都是生成了一个新的 String 对象，这样 **效率低下，浪费大量的内存空间**。

**举例：**
```java
str = '123';
str = str + '456';
```

在上面的两行代码中，开辟了三次空间：
1. 第一句中显示开辟了 ‘123’ 的空间
2. 然后第二句中的 ‘456’ 又开辟了一块空间
3. 最后 str 的赋值又开辟了 ‘123456’ 的空间

所以上述过程中的，看上去是str的值发生了变化，**实际上真正变化的是str对堆内存空间的指向**

那么，在str获得最新的值 ‘123456’ 之后， 原来的‘123’ 和 ‘456’ 两块空间如果接下来没有被再次引用，那么只能等待着被回收（JVM的GC）。


## StringBuffer 和 StringBuilder

**所以**，  **经常改变内容的字符串不要使用String**，可以用 StringBuffer  和 StringBuilder：

它们都**可以被多次的修改，且不会产生新的未使用对象。**

它们俩之间的区别在于 StringBuilder 不是线程安全的（即不能同步访问）

StringBuilder 相较于StringBuffer 有**速度优势**，所以**多数情况下建议使用 StringBuilder 类。**

**BUT**， 如果强调线程安全的话，还是使用 StringBuffer。

### StringBuffer

代表一个字符序列可变的字符串，当一个StringBuffer被创建以后，通过StringBuffer提供的append()「尾部添加」、insert()「插入」、reverse()「反转」、setCharAt()、setLength()等方法可以改变这个字符串对象的字符序列。

> 一旦通过StringBuffer生成了最终想要的字符串，就可以调用它的toString()方法将其转换为一个String对象。

所以说StringBuffer对象是一个字符序列可变的字符串，它没有重新生成一个对象，而且在原来的对象中可以连接新的字符串。

### StringBuilder

StringBuilder类也代表可变字符串对象。实际上，StringBuilder和StringBuffer基本相似，两个类的构造器和方法也基本相同。不同的是：**StringBuffer是线程安全的，而StringBuilder则没有实现线程安全功能，所以性能略高。**

#### StringBuffer是如何实现线程安全的呢？

StringBuffer类中的方法都添加了**synchronized关键字**，也就是给这个方法添加了一个锁，用来保证线程安全。

---
简单比较:

|StringBuilder|StringBuffer|
|:-:|:-:|
|线程不安全|线程安全|
|执行速度快|执行速度慢|


## 总结

1. 如果操作少量的数据, 使用 String
2. 多线程操作字符串缓冲区下操作大量数据, 使用StringBuffer
3. 单线程操作字符串缓冲区下操作大量数据, 使用StringBuilder




