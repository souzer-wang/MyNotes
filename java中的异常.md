# 分类

![异常分类](./Throwable.png)

## Error

一般指程序无法处理的错误，大多数和代码编写者无关。

比如java虚拟机运行错误，当 JVM 不再具备能继续执行操作所需的内存资源的时候，将出现 OutOfMemoryError。当这样的情况出现的时候，JVM 一般会选择线程终止。

## Exception

一般指程序本身可以处理的异常，又可以细分为 checked exceptions 和 unchecked exceptions

### checked exceptions 非运行时异常（编译异常）

通常是写代码的时候，需要去 try catch 的那种 exception，比如 IOException。

这类 Exception 是Java的设计者要求你的程序去处理的，这种异常一般不会影响程序的主体，容易手动诊断修复（所以需要在try catch 下面给出处理异常的代码）。

如果不处理，那么程序的编译就无法通过

### unchecked exceptions 运行时异常

都是 RuntimeException 类及其子类异常，即程序运行时终止，控制台出现的异常，举一些例子：

- NullPointerException： 空指针异常：程序试图访问**空数组中的元素**，或访问**空对象中的方法和变量**产生的异常
- IndexOutOfBoundsException： 下标越界异常：**数组下标越界** 或 **字符串访问越界** 引起异常
- ArrayIndexOutOfBoundsException： 数组元素下标越界异常
- StringIndexOutOfBoundsException： 字符串序号越界异常
- ClassCastException： 类转换异常：把对象归为某个类，但实际上这个对象并不是这个类创建的，也不是它的子类创建的，会引起异常
- ArithmeticException：由于除数为 0 引发的异常
- ArrayStoreException：由于数组**存储空间不够**引起的异常
- IllegalMonitorStateException：监控器状态出错引起的异常
- NegativeArraySizeException：数组长度是负数，引起异常
- OutOfMemoryException：用new创建对象时，如系统**无法**为其**分配内存空间**，则引起异常
- SecurityException：访问了**不应访问的指针**，引起安全性问题，导致异常
- IOException：文件**未找到、未打开 或 I/O 操作不能进行** 引起的异常
- ClassNotFoundException：未找到指定名字的类或接口引发的异常
- FileNotFoundException：未找到指定文件引发异常
- EOFException：未完成输入操作就遇到文件结束引起的异常
- ……