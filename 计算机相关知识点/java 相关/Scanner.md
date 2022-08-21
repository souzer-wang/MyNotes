### 什么是 Scanner

Scanner 类主要是用来读取字符及字符串的

一般使用默认的System.in对象来得到Scanner对象，如下：
```java
Scanner scanner = new Scanner(System.in);
```
其中 System.in 本身是一个 InputStream 类型对象，且只要是 InputStream 对象都可以作为参数创建 Scanner 对象

举栗说明输入操作：

1. 输入字符串

可以有 nextLine() 和 next() 两种方法，读取前可以分别用hasNext 与 hasNextLine 判断是否还有输入的数据，这两个的区别在于：
- next():
  - 一定要读取到有效字符才开始算输入，即对于有效字符前面的空白，next() 方法会自动将其去掉
  - 在读取到有效字符后，会把有效字符后面的空格作为分隔符或结束符
  - 由第二点可知：next() 方法不能得到有空格的字符串
以代码为例：
```java
Scanner sc = new Scanner(System.in);
String str = sc.next();
String str2 = sc.next();
System.out.println(str);
System.put.println(str2);
```

我们输入：  123  456
得到的输出是：
123
456

即因为空格的存在，别读取为了两个字符串

- nextLine()
  - 以 Enter 为结束符，即读取换行前的所有内容
  - 因而可以读到空格

```java
Scanner sc = new Scanner(System.in);
String str = sc.nextLine();
System.out.println(str);
```
输入： 123  456
输出： 123  456

即整行都读作了字符串

2. 输入数字

如果要输入 int 或者 float 类型的数据，在 Scanner 类中也有支持。可以在输入之前使用 hasNextXxx() 方法来验证，再使用 nextXxx() 来读取。

如果是一行就一个数字，既可以使用nextInt(), 也可以使用nextLine()，再转换为int
```java
int num = sc.nextInt();

String str = sc.nextLine();
int num = Integer.parseInt(str);
```

或者可以一行读取多个数字
```java
int num1 = sc.nextInt();
int num2 = sc.nextInt();
int num3 = sc.nextInt();

System.out.println(num1);
System.out.println(num2);
System.out.println(num3);
```

输入三个数字，无论是空格还是换行隔开，都可以读取到
举栗：
输入：12 23 换行 34
输出：12 换行 23 换行 34