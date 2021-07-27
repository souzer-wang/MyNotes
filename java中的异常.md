# 概念

先梳理一下概念，什么是异常？

异常是指：**程序运行时发生的不被期望的事件，它阻止了程序按照预期正常执行**

那么什么是异常处理机制?

异常处理机制是指：**能让程序在异常发生的时候，按照代码的预先设定的异常处理逻辑，针对性地处理异常，让程序尽可能恢复正常并继续执行**

Java中的异常可以是函数中的语句执行时引发的，也可以是程序员通过throw语句手动抛出的。

## Java异常机制用到的几个关键字

- try 用于监听，将要被监听的代码（可能抛出异常的代码）放在try语句块中，当try语句块内发生异常时，异常就被抛出。
- catch 用于捕获异常，catch用来捕获try语句块中发生的异常。
- finally 总是会执行。它主要用于回收在try块中打开的物理资源
  - 注：只有 finally 块执行完成后，才会回来执行 try 或者 catch 块中的 return 或者 throw 语句，**but**，如果 finally 中使用了 return 或者 throw 等终止方法的语句，就不会跳回执行
- throw 用于抛出异常
- throws 用在方法签名中，用于声明该方法可能抛出的异常。

# 分类

三种类型的异常：
- **检查性异常**： 最具代表的检查性异常，是用户错误或问题引起的异常，这是无法预见的。（比如打开一个不存在的异常）
- **运行时异常**： 运行时异常是可能被程序员避免的问题，与检查性异常相反，运行时异常可以在编译时被忽略
- **错误**： 错误不是异常，而是脱离程序员控制的问题

异常的根接口是 Throwable，其下有两个子接口，Error 和 Exception

其中，
- Error 指的是JVM错误，这时的程序并没有执行，无法处理
- Exception 指的是程序运行中产生的异常，用户可以使用处理格式处理



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

# 异常的使用和执行流程

1. 异常的处理方案

try...catch、try...catch...finally、try...finally

try{
  可能会发生的异常
} catch(异常类型 异常名(变量)) {
  针对异常进行处理的代码
} catch(异常类型 异常名(变量)) {
  针对异常进行处理的代码
}
[finally{
  释放资源代码;
}]

注意点：
- catch 必须和 try 一起用
- catch 中必须有内容
- 在 try/catch 后面添加 finally 代码块非强制要求
- try 中的内容越少越好
- try 的后面不能同时没有 catch 和 finally 块
- try，catch，finally块之间不能添加任何代码
- finally里面的代码最终一定会执行
- 如果程序存在多个异常，那么需要多个catch去进行捕获
- 关于catch之间的顺序：
  - 如果异常之间是同级的关系，那么catch谁前谁后没有关系
  - 如果异常之间存在上下级关系，上级需要放在后面

# 概念梳理

## Error 和 Exception 的区别

Error 是系统中的错误，程序员是不能处理的，是在编译时出现的错误，只能通过修改程序才能修正。比如：系统崩溃、虚拟机错误、内存空间不足、方法调用栈溢等。

## catch 捕获异常的时候，为什么不考虑使用Throwable类型，而只是使用 Exception 来进行接收？

Throwable 表示的范围要比 Exception 大，实际上程序使用 Throwable 来处理，没有任何语法问题，但有逻辑问题。为什么呢？

因为此处出现的只有 Exception 类型，如果使用 Throwable 接收，还会表示可以处理 Error 的错误，但实际上用户是无法处理 Error 的错误的。**所以**，开发中用户可以处理的异常还是以 Exception 类为主

## 异常是一起处理好 还是 分开处理好？

**根据实际的开发要求是否严格来决定**。在实际的项目开发项目工作中，如果项目开发环境严谨，那么基本上会针对每一种异常进行处理，并且要详细记录下异常产生的时间和产生的位置，以便进行维护。

**注意：** 在处理多个异常的时候，捕获范围小的异常要放在捕获范围大的异常之前处理。

## throw 和 throws 的区别

throw: 指的是在方法中人为抛出一个异常对象（这个异常对象可能是自己实例化或者抛出已存在）
throws：在方法的声明上使用，表示**此方法在调用的时候必须处理异常**

## 检查型异常（Checked Exception）和非检查型异常（Unchecked Exception）的区别

- 所有的检查性异常都继承自 java.lang.Exception；所有的非检查性异常都继承自 java.lang.RuntimeException 
- 最主要的区别在于其处理异常的方法：
  - 检查型异常必须使用 try catch 或者 throws 等关键字来进行处理，否则编译器会报错
  - 非检查型异常一般是程序代码写的不够严谨而导致的问题，可以通过修改代码来规避
- 常见的运行时异常：空指针异常（NullPointerException）、除零异常（ArithmeticException）、数组越界异常（ArrayIndexOutOfBoundsException）等
- 常见的检查性异常：输入输出异常（IOException）、文件不存在异常（FileNotFoundException）、SQL语句异常（SQLException）等