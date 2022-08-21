[本文参考链接](https://www.cnblogs.com/justcooooode/p/7603381.html)

我们平时说的内存就是图中的**运行时数据区(Runtime Data Area)**，其中与字符串创建相关的是：
- 方法区（Method Area）
- 堆区（Heap Area）
- 栈区（Stack Area）

![运行时数据区](https://images2017.cnblogs.com/blog/1177828/201709/1177828-20170929204305559-896175948.png)

**具体每个区的属性可参考  [[堆、栈、方法区]]**

---

![图一](http://images2017.cnblogs.com/blog/1177828/201709/1177828-20170920153811462-862037386.jpg)

以上面这张图为例解释一些概念：

- 每当一个方法被执行就会在栈区中创建一个**帧栈（Stack Frame）**，**基本数据类型** 和 **对象引用** 就存在栈帧中的 **局部变量表(Local Variables)**。
- 在一个类被加载之后，类信息就会被存在非堆的方法区中。其中有一块地方叫做 **运行时常量池（Runtime Constant Pool）**，这是每个类私有的，每个 class 文件的“常量池”被加载后就映射存放在这里
- 和 String 关系最大的是 **字符串池（String Pool）**，它的位置在方法区上面的**驻留字符串（Interned Strings）的位置**，字符串调用了 String.intern() 方法后，引用就存放在 String Pool 中。
  - 注意区分它和运行时常量池，字符串池是全局共享的，而运行时常量池是每个类私有的


---

有了以上的概念，就来看一下两种字符串创建方法的区别

1. 字面量赋值

这是一个 Test.java 文件，字面量赋值的方法得到 str 的值是 Hello
```java
public class Test {
    public static void main(String[] args) {
        String str = "Hello";
    }
}
```

这里的实现流程如下：

1.1 Test.java 文件编译后得到 .class 文件，里面会包含类的信息，其中有一块叫做**常量池（Constant Pool）**（即上面提到的运行时常量池），常量池中就包含了**字面量**，而字面量包含类中定义的常量。这里的 "Hello" 就在字面量中（参考下图）

![class文件](https://images2017.cnblogs.com/blog/1177828/201709/1177828-20170929184033950-619493112.png)

1.2 当程序用到 Test 类的时候，Test.class 就会被解析到内存中的方法区，而 .class 文件中的常量池信息就会被加载到运行时常量池，**但 String 不是**

本例中 Hello 会在堆区中创建一个对象，并且在字符串池（String Pool）中存放它的一个引用，如下图

![第二步](https://images2017.cnblogs.com/blog/1177828/201709/1177828-20170920154211321-596178280.png)

1.3 此时 Test 类刚刚被加载，主函数中的 str 还没有创建，而堆中的 "Hello" 对象已经被创建

在主线程开始创建 str 变量的时候，虚拟机会去字符串池中查找是否有 equals("Hello") 的 String
- 如果有就把这个 String 的引用复制给 str。
- 如果没有，就会在堆中新建一个对象，并把引用驻留在字符串池中，再把引用赋给 str

![第三步](https://images2017.cnblogs.com/blog/1177828/201709/1177828-20170920212341228-1393425975.png)


补充：在用字面量赋值的方法创建字符串的时候，无论创建多少次，只要字符串的值相同，他们指向的都是堆中的同一个对象

![补充](https://images2017.cnblogs.com/blog/1177828/201709/1177828-20170920213030587-629195501.png)

2. 构造函数赋值

在用 new 关键字去创建字符串的时候，前面的加载过程一样，只是在运行时无论字符串池中有没有 equals("Hello") 的 String，都会在堆中新开辟一片内存来创建对象

```java
public class Test {
    public static void main(String[] args) {
        String str1 = "Hello";
        String str2 = "Hello";
        String str3 = new String("Hello");
    }
}
```

![new创建](https://images2017.cnblogs.com/blog/1177828/201709/1177828-20170920213937103-776364568.png)

---

### 总结

- 字面量创建字符串会先在字符串池中查找，看是否有相等的对象
  - 如果有，直接引用池中
  - 如果没有就在堆中创建，并且把地址驻留在字符串池中
- 而使用 new 关键字创建时，前面的操作和字面量一样，只是在最后运行时会创建一个新对象，变量所引用的都是这个新对象的地址

---

### intern() 方法

解释一下 intern() 方法，按注释，“如果常量池中存在当前字符串（还是用 Equals() 判断），就直接返回该字符串；如果没有，将此字符串放入常量池中，再返回对常量池中该 String 对象的引用”

**这是 JDK1.6 及之前的做法，在 JDK1.7 之后就不一样了。**

在 1.7 中，如果常量池中没有想找的字符串，那么就会**在常量池中生成一个对堆中该字符串的引用（如果堆中没有的话会创建）**
> - 1.6 是常量池中生成原字符串的拷贝
> - 1.7 是生成对原字符串的引用


---

### 常见题目

用一些栗子来分析：

```java
String s1 = "Hello";
String s2 = "Hello";
String s3 = "Hel" + "lo";
String s4 = "Hel" + new String("lo");
String s5 = new String("Hello");
String s6 = s5.intern();
String s7 = "H";
String s8 = "ello";
String s9 = s7 + s8;
          
System.out.println(s1 == s2);  // true
System.out.println(s1 == s3);  // true
System.out.println(s1 == s4);  // false
System.out.println(s1 == s9);  // false
System.out.println(s4 == s5);  // false
System.out.println(s1 == s6);  // true
```

补充： 做 Java 中，直接使用 == 操作符，比较的是两个字符串的引用地址，而不是比较内容，如果要比较内容还是用 equals()

- s1 = s2 很好理解，它俩在赋值的时候，都用的是字面量。在编译期间，这种字面量会直接放入 class 文件的常量池中，从而实现复用。在载入运行时常量池后，它俩指向的是同一个内存地址，所以相等。

- s3 虽然是字符串拼接操作，但是参与拼接的是字面量，且都是已知的，编译器会进行优化，所以在编译时已经是拼好的 "Hello" 了（即 String s3 = "Hello";）

- 而对于 s4，"lo" 是通过 new 关键字创建的，编译器无法知道它的地址，不能像 s3 一样优化，必须到运行时才可以确定结果。而新对象的地址和前面的必然不一样

- s9 的情况和上面类似，虽然 s7, s8 在赋值的时候用的是字符串字面量，但当它俩被用到的时候，实际上会创建 String 对象，分别指向堆上的一个 "H" 和 "ello"，它俩拼接出来的结果又是一个新的 String 对象了，不可能与 s1 地址相同
- s5 是 new 方式创建的，所以也不同

- 对于 s6，因为 s5 在堆中，值为 "Hello"，intern() 方法会尝试将 "Hello" 字符串添加到常量池中，然后返回它在常量池中的地址，但是因为常量池中已经有了 Hello 字符串，所以 intern() 方法会直接返回地址。因而 s1 和 s6 指向同一个地址。

**这里可以得到三个比较重要的结论：**
1. 通过分析编译时的行为才能更好地理解常量池
2. 运行时常量池中的常量，基本来自于各个 class 文件中的常量池
3. 程序运行时，除非手动向常量池添加常量（如 intern() 方法），否则 JVM 不会自动添加常量到常量池


Q: 下面程序的输出结果

```java
String s1 = "abc";
String s2 = "abc";
System.out.println(s1 == s2);
```

输出 true，均指向常量池中的同一个对象

Q: 下面程序的输出结果

```java
String s1 = new String("ABC");
String s2 = new String("ABC");
System.out.println(s1 == s2);
``` 

输出 false，分别指向堆中不同的位置

Q: 下面程序的输出结果

```java
String s1 = "abc";
String s2 = "a";
String s3 = "bc";
String s4 = s2 + s3;
System.out.println(s4 == s1);
```

输出 false，s2 + s3 实际上是使用 StringBuilder.append()	来完成的，会生成不同的对象

Q: 下面程序的输出结果

```java
String s1 = "abc";
final String s2 = "a";
final String s3 = "bc";
String s4 = s2 + s3;
System.out.println(s1 == s4);
```

输出 true，因为 final 变量在编译后会直接替换成对应的值，所以实际上第四行就变成了 s4 = "a" + "bc"，所以 s1 = s4

Q: 下面程序的结果

```java
String s = new String("abc");
String s1 = "abc";
String s2 = new String("abc");
String s3 = "abc";

System.out.println(s == s.intern());
System.out.println(s == s1.intern());
System.out.println(s == s2.intern());
System.out.rpintln(s1 == s2.intern());
System.out.println(s3 == s2.intern());
```

输出结果依次为：false, false, false, true, true

分析：
1. 第一行：在堆中新创建了一个 "abc" String 对象，s 指向它
2. 第二行：字符串池中并没有 equals("abc") 的字符串，所以在堆中创建一个 "abc" 字符串对象，然后在字符串池中添加对这个对象的引用
3. 第三行：在堆中新建一个 "abc" String 对象，s2 指向它
4. 第四行：字符串池中已有 "abc"，所以将该引用赋值给 s3

所以对于下面的intern方法，都是一样的，即对 "abc" 做 intern()，得到的都是字符串池中对 "abc" 的同一个引用