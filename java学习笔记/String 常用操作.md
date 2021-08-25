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





