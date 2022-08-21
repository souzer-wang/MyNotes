## 输入

Java 中输入一般是通过 Scanner 类 来实现的

创建 Scanner 对象， 接受从控制台输入, 按类型接受，然后输出

```java
Scanner input = new Scanner(System.in);

//接受 String 类型
String str = input.next();

//接受 int 类型
int n = input().nextInt();

//输出结果
System.out.println(str);

System.out.println(n);
```

### 示例

```java
import java.util.Scanner;

public class Demo {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        String str = input.next();
        System.out.println(str);
    }
}
```

再举例，输入一个指定长度的数组
```java
import java.util.Scanner;

public class Demo {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        int len = input.nextInt();
        int[] nums = new int[len];
        for (int i = 0; i < len; i++) {
            nums[i] = input.nextInt();
        }
        //打印数组
        for (int num : nums) {
            System.out.print(num + " ");
        }
    }
}
```




## 输出

记录一下Java中的打印

Java中目前常用的打印方法有三种：
1. println
2. printf
3. print

### println

主要的特点是：可以换行输出

### print

一般输出，不换行，也不能精度格式转换

### printf

通常用于格式转换

具体的使用方法如下：

先详细讲一下f格式：
- %f：不指定宽度，整数部分全部输出并输出6位小数
- %m.nf：输出共占 m 列，其中有 n 位小数，若数值宽度小于 m，左端补齐空格（比如n为0时就是取整）
- %-m.nf：输出共占 m 列，其中有 n 位小数，若数值宽度小于 m，右端补齐空格

一些常用的：
- %c 一个字符
- %d 有符号十进制整数(int) （%ld、%Ld：长整型数据(long)，%hd：输出短整型）
- %u 无符号十进制整数
- %f 单精度浮点数（默认 float）
- %s 对应字符串 char*
- %n 换行
- %% 输出百分号