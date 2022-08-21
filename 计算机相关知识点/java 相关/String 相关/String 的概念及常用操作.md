## String 的概念


### 创建字符串的操作

String 的赋值有两种方法，**字面量赋值** 和 **new 关键字创建新对象**

1. 字面量赋值

```java
String str = "test";
```

这里是一个字符串常量，编译器在使用到该值的时候会创建一个 String 对象

> 除此之外，可以使用 **关键字和构造方法** 来创建 String 对象

2. 构造方法

```java
String s1 = new String("test");
```

> **比较：** String 创建的字符串是存储在公共池中的，而 new 创建的字符串对象在堆上

String 类有 11 种构造方法，这些方法提供不同的参数来初始化字符串，比如可以通过一个字符数组参数来初始化

```java
public class StringDemo{
    public static void main(String args[]) {
        char[] helloArray = {'t', 'e', 's', 't'};
        String str = new String(helloArray);
        System.out.println(str);
    }
}
```

---

## 字符串常量池

上面提到了字符串常量池的概念，接下来进行详细的概念补充：
- JVM 为了提高性能和减少内存开销，在实例化字符串常量的时候进行了一些优化
  - 开辟了一个字符串常量池，类似于缓冲区
  - 在创建字符串常量的时候，优先检查字符串常量池中是否有该字符串
  - 如果存在该字符串，返回引用实例；如果不存在，实例化该字符串并放入池中

**字符串常量池位于方法区中**

---

## 字符串操作

### 查找字符串

有两种方法：

1. indexOf(String s)

用来返回目标字符串 s 在指定字符串中首次出现的位置，如果没有则返回 -1

```java
String str = "We are students";
int ans = str.indexOf("a");
```

2. lastIndexOf(String s)

用来返回字符串最后一次出现的索引位置，如果没有返回 -1

### 获取指定索引位置的字符

```java
String str = "hello world";
char mychar = str.charAt(3); //返回l
```

### 获取子字符串

通过 String 类的 substring() 方法可对字符串进行截取，有两种方法：
1. 指定开始位置

```java
String newstr = str.subString(3);
```

2. 指定开始和结束位置

```java
String newstr = str.subString(0, 3);
```

### 去除空格

```java
str.trim();
```

### 字符串替换

```java
String str = "abababaab";
String newstr = str.replace("a", "A");
```

### 判断字符串的开头和结尾

```java
String str = "abcdef";
return str.startsWith("ab");
return str.endsWith("ef");
```

### 判断字符串是否相等

```java
1. equals(String otherstr);
2. equalsIgnoreCase(String otherstr); 忽略大小写
```

### 按字典序比较两个字符串

```java
str.compareTo(String otherstr);
```

如果str的字典序在otherstr之前，结果为负，如果在之后，则为正。

### 大小写转换

```java
str.toLowerCase();
str.toUpperCase();
```

### 字符串分割

根据指定的 符号 或 字符 或 字符串 对字符串进行分割，也可以定义多个分隔符(中间用|连接)，如".|="代表 . 和 =

```java
str.split(String sign);
```





