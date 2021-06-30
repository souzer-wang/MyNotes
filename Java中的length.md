```java
public class test {
    public static void main(String[] args){
        // 示例一
        String str = "abc";
        System.out.println(str.length());
 
        //示例二
        String[] str1 = {"abc","dsa"};
        System.out.println(str1.length);
 
        //示例三
        int[] arr = {1,2,3};
        System.out.println(arr.length);
    }
}
```

这里第一个String对象用的是length()，第二三个用的是length

通过查看 String 的源码，可以发现，String是通过char数组来存储的，具体是：

```java
public final class String
    ...
    /** The value is used for character storage. */
    private final char value[];
    ...

    public int length() {
        return value.length;
    }
```
length()方法实际上返回的是 value.length

### 为什么数组可以拥有自己的length属性呢？

因为数组是一个容器对象，它包含固定数量的同一类型的值，一旦数组创建，它的长度就固定了。所以length可以视为数组的一个属性。

### 小结

|类型|方法|含义|
|:-:|:-:|:-:|
|数组|length|属性，返回值 int|
|字符串|length()|方法，返回值 int|
|集合|size()|方法，返回值 int|