## arraylist和linkedlist的区别
**1、数据结构不同  **
  
ArrayList是Array(动态数组)的数据结构，LinkedList是Link(链表)的数据结构。  
  
**2、效率不同  **
  
当随机访问List（get和set操作）时，ArrayList比LinkedList的效率更高，因为LinkedList是线性的数据存储方式，所以需要移动指针从前往后依次查找。  
  
当对数据进行增加和删除的操作(add和remove操作)时，LinkedList比ArrayList的效率更高，因为ArrayList是数组，所以在其中进行增删操作时，会对操作点之后所有数据的下标索引造成影响，需要进行数据的移动。【视频教程推荐：Java视频教程】  
  
**3、自由性不同  **
  
ArrayList自由性较低，因为它需要手动的设置固定大小的容量，但是它的使用比较方便，只需要创建，然后添加数据，通过调用下标进行使用；而LinkedList自由性较高，能够动态的随数据量的变化而变化，但是它不便于使用。  
  
**4、主要控件开销不同  **
  
ArrayList主要控件开销在于需要在lList列表预留一定空间；而LinkList主要空间开销在于需要存储结点信息以及结点指针信息。

## LinkedList 常用方法

### 增加

- add(e)： 链表尾部添加，通用方法
  - 从源码看，add 不仅把元素链接到队列尾部，还返回 true
```java
public boolean add(E e) {
  linkLast(e);
  return true;
}
```
- addFirst(e)： 在链表头部添加，特有方法
```java
public void addFirst(E e) {
  linkFirst(e);
}
```
- addLast(e)： 在链表尾部添加，特有
  - 从源码来看，addLast 就是把元素链接到队列尾部
```java
public void addLast(E e) {
  linkLast(e);
}
```
- push(e)： 与addFirst一致
- offer(e): 与addLast一致
  - offer 直接调用了 add 方法，所以在 LinkedList 中 add() 和 offer() 使用起来是一样的
```java
public boolean offer(E e) {
  return add(e);
}
```
> 补充说明 add 和 offer 的区别：
> LinkedList 是继承了 Deque 接口，而 Deque 接口继承了 Queue 接口，在 Queue 中对于 add 和 offer 的 区别是这样介绍的
> 
> add(): 
> - 在不违背队列容量限制的情况下，往队列中添加一个元素
>   - 如果添加成功，返回 true
>   - 如果因为容量限制添加失败，抛出异常
> 
> offer():
> - 在不违背队列容量限制的情况下，往队列中添加一个元素
>   - 如果添加成功，返回 true
>   - 如果因为容量限制添加失败，返回 false
> 
> 在容量限制的队列中，offer 方法优于 add 方法，因为处理抛出的异常更加耗时，offer 直接返回 false 的方法更好

- add(idx, e)：在指定位置插入一个元素
- offerFirst(e)：在头部添加
- offerLast(e): 在尾部添加
  - 将元素添加到队尾并返回 true，可以看出 offerLast 和 add 的效果是一样的
```java
public boolean offerLast(E e) {
  addLast(e);
  return true;
}
```

```java
LinkedList<Integer> t = new LinkedList<>();
t.add(1); // [1]
t.addFirst(2); //[2, 1]
t.addLast(3); //[2, 1, 3]
t.push(4); //[4, 2, 1, 3]
t.offer(5); //[4, 2, 1, 3, 5]
t.add(2,6); //[4, 2, 6, 1, 3, 5]
t.offerFirst(7); //[7, 4, 2, 6, 1, 3, 5]
```

### 删除

- remove()：移除第一个元素，通用
- remove(idx)：移除指定元素，通用
- removeFirst()：删除头，获取元素并删除，特有
  - 删除并返回队首元素，若队列为空，则抛出异常
```java
public E removeFirst() {
  final Node<E> f = first;
  if (f == null) {
    throw new NoSuchElementException();
  }
  return unlinkFirst(f);
}
```
- removeLast()：删除尾，特有方法
- pollFirst()：删除头，特有方法
  - 删除并返回队首元素，若队列为空，返回 null
```java
public E pollFirst() {
  final Node<E> f = first;
  return (f == null) ? null:unlinkFirst(f);
}
```
- pollLast()：删除尾，特有方法
- pop()：和removeFirst一致
- poll()：查询并移除第一个元素，特有

### 查询

- get(idx)：按下标获取元素，通用
- getFirst()：获取第一个元素，特有
- getLast()：获取最后一个元素，特有
- peek()：获取第一个元素，但是不移除，特有
- peekFirst()：获取第一个元素，不移除
- peekLast(): 获取最后一个元素，不移除
- pollFirst()：查询并删除头，特有方法
- pollLast()：查询并删除尾，特有
- poll()： 查询并删除头，特有

#### 小结

- 需要链接元素到队列尾时优先使用 offer()
- 查看元素优先使用 peek()
- 删除元素优先使用 poll()

**Others**
- 想要在指定位置链接元素可以使用 add(int index, E element)
- 获取指定索引的元素可以使用 get(int index)
- 修改指定索引的元素可以使用 set(int index, E newElement)

## ArrayList 用法详解

1. 什么是 ArrayList

ArrayList 是 Java 集合框架中的一个重要的类，它继承自 AbstractList，实现了 List 接口，具体如下：

![结构层次](https://img-blog.csdn.net/20160311083300211)

除此以外，它还实现了 RandomAccess、Cloneable、Serializable接口，因而它分别支持 快速访问、复制、序列化 

其实就是传说中的动态数组，可以理解为Array的复杂版本，它的优点在于：
- 动态地增加和减少元素
- 实现了 ICollection 和 IList 接口
- 灵活地设置数组大小

数组是静态的，在初始化之后就不能再改长度了。 而 ArrayList 是可以动态改变大小的，所以简单的判断标准就是知道多少个数据时就用数组，不知道的时候用ArrayList

2. 如何构建 ArrayList

2.1 ArrayList 支持三个类构造方法：
- ArrayList() 无参，构造了一个空的
- ArrayList(Collection\<? extends E> c) 这个构造方法构造了一个包含指定元素集合的
- ArrayList(int initialCapacity) 构造一个指定大小但内容为空的

举例：
```java
//创建空的
ArrayList<String> list = new ArrayList<String>();
//指定长度
ArrayList<Integer> list = new ArrayLisy<Integer>(7);
```
2.2 初始化 ArrayList 的方法

方法一：
```java
ArrayList<String> list = new ArrayList<String>();
String str01 = new String("str01");
String str02 = new String("str02");
list.add(str01);
list.add(str02);
```

方法二：双括号法
```java
ArrayList<String> list = new ArrayList<String>(){{
    add("1");
    add("2");
}};
```
法一和法二是类似的

方法三：Arrays.asList
```java
List<Integer> test = Arrays.asList(1, 2, 3);//这种方法生成的数组无法修改
List<Integer> test = new ArrayList<>(Arrays.asList(1, 2, 3));
```

方法四：stream

```java
List<Integer> test = Stream.of(1, 2, 3).collect(Collectors.toList());
```

方法五：Lists
```java
List<Integer> test = Lists.newArrayList(1, 2, 3);
```

3. ArrayList 常用方法

- 增加元素
  - boolean add(Element e) 增加指定元素到尾部
  - void add(int index, Element e) 增加指定元素到指定位置

- 删除元素
  - void clear() 删除所有
  - E remove(int index) 删除指定位置的元素
  - protected void removeRange(int start, int end) 删除一个范围内的元素

- 获取元素
  - E get(int index) 获取指定位置的元素
  - Object[] toArray() 将链表转为某个数组

- 修改某个元素
  - E set(int index, E element) 替换指定位置的元素

- 搜索元素
  - boolean contains(Object o) 是否包含某元素
  - int indexOf(Object o) 返回元素第一次出现的位置，没有的话返回 -1
  - int lastIndexOf(Obejct o) 返回元素最后一次出现的位置，没有返回-1

- 检查链表是否为空
  - boolean isEmpty()

- 获取链表大小
  - int size()

