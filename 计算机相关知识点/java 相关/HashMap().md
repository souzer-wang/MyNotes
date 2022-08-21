### entrySet()

entrySet() 方法返回映射中包含的映射的 Set 视图，每个键值对就是一个 Entry

使用方法： hashmap.entrySet()

示例代码：

```java
import java.util.HashMap;

class main {
	public static void main(String[] args) {
		HashMap<Integer, String> sites = new HashMap<>();
		
		//添加元素
		 
		sites.put(1, "Google");  
		sites.put(2, "Runoob");  
		sites.put(3, "Taobao");  
		System.out.println("sites HashMap: " + sites);  

		// 返回映射关系中 set view  
		System.out.println("Set View: " + sites.entrySet());
	}
}
```

以上程序的输出结果是：

sites HashMap: {1=Google, 2=Runoob, 3=Taobao}
Set View: \[1=Google, 2=Runoob, 3=Taobao]

**另外，entrySet() 还可以与 for-each 循环一起使用，用来进行遍历每个映射项**

```java
import java.util.HashMap;
import java.util.Map.Entry;

......

for (Entry<Integer, String> entry: sites.entrySet()) {
	System.out.println(entry.getKey());
	System.out.println(entry.getValue());
	System.out.println(entry);
}
```


一共有四种遍历 map 的方法：

1. 使用 keySet() 来遍历，二次取值

```java
for (String key : map.keySet()) {
	System.out.println("key= "+ key + " and value= " + map.get(key));
}
```

2. 通过 entrySet() 方法结合 iterator 来遍历

```java
Iterator<Entry<String, String>> it = map.entrySet().iterator();
while (it.hasNext()) {
	Entry<String, String> entry = it.next();
	System.out.println("key= " + entry.getKey() + " and value= " + entry.getValue());
}
```

3. 通过entrySet() 方法（推荐）

```java
for (Entry<String, String> entry : map.entrySet()) {
	System.out.println("key = " + entry.getKey() + " and value = " + entry.getValue());
}
```

4. 通过 Map.values() 方法

```java
for (String v : map.values()) {
	System.out.println("value = " + v);
}
```