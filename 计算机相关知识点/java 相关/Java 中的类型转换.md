### StringBuilder 和 String 互转

```java
//String 转 StringBuilder
String str = "abc";
StringBuilder stb = new StringBuilder(str);

//StringBuilder 转 String
StringBuilder stb = new StringBuilder("abc");
String str = stb.toString();
```

### 整形 和 字符串 互转

```java
//整形转字符串(Double, Float, Long 的方法类似)
String str = Integer.toString(num);
String s = String.valueOf(num);
String s = "" + num;

//字符串转整形
int num = Integer.valueOf(str).intValue();
int num = Integer.parseInt(str);
```

### 字符串 和 字符数组 互转

```java
//字符串转字符数组
String str = "123456";
char[] c = str.toCharArray();

//字符数组转字符串
char[] c = {'1','a','b'};
String str = new String(c);
```

### 整形数组 和 字符串 互转

```java
//整型数组转字符串
StringBuilder s = new StringBuilder();
for (int i = 0; i < nums.length; i++) {
	s.append(String.valueOf(nums[i]));
}
String str = s.toString();

//字符串转整型数组
String str = "abc";
int[] a = new int[str.length()];
for (int i = 0; i < str.length(); i++) {
	a[i] = str.charAt(i) - '0';
}
```

### 
