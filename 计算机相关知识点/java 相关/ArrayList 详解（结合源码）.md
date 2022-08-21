# 简介

- ArrayList 继承自 AbstractList， 实现了 List 接口。

- 底层基于数组，实现容量大小的动态变化，允许 null 的存在。

- 同时实现了 RandomAccess、Cloneable、Serializable 接口，所以 ArrayList 是支持 快速访问、复制、序列化的

# 成员变量

ArrayList 的底层是基于数组来实现容量大小动态变化的

```java
private int size; // 实际元素个数
transient Object[] elementData; // elementData.length 为集合容量，即可以容纳多少个元素
```

默认的初始容量大小为 10

```java
/**
* Default initial capacity.
*/
private static final int DEFAULT_CAPACITY = 10;
```

接下来看两个变量

```java

private static final Object[] EMPTY_ELEMENTDATA = {};

private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
```

这两个变量分别对应空的构造函数和有参构造函数，具体解释如下：
- 如果是无参构造，那么 Object elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
- 如果是有参构造，若给定的初始容量为 0，或者传入集合为空集合（非 null），那么 Object elementData = EMPTY_ELEMENTDATA

具体可以参看下面的构造函数

### 无参构造函数

注释说：创建一个容量为 10 的空的 list 集合

but， 实际上，构造函数只是给**elementData 赋值了一个空的数组**，在第一次添加元素的时候，才把容量扩到到 10

```java
public ArrayList() {
	this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}
```

### 有参构造函数

**指定初始容量：**

```java
public ArrayList(int initialCapacity) {
	if (initialCapacity > 0) {
		this.elementData = new Object[initialCapacity]
	} else if (initialCapacity == 0) {
		this.elementData = EMPTY_ELEMENTDATA;
	} else {
		throw new IllegalArgumentException("Illegal Capacity: " +initialCapacity);
	}
}
```


**用指定 Collection 来构造：**

将 Collection 转化为数组并赋值给 elementData，把 elementData 中元素的个数赋值给 size

```java
public ArrayList(Collection<? extends E> c) {
	elementData = c.toArray();
	if ((size == elementData.length) != 0) {
		if (elementData.getClass() != Object[].class)
			elementData = Arrays.copyOf(elementData, size, Object[].class);
	} else {
		this.elementData = EMPTY_ELEMENTDATA;
	}
}
```

- 如果 size 不为 0，则判断 elementData 的类型是否为 Object[]，如果不是，做一次转换

- 如果 size 为 0，把 EMPTY_ELEMENTDATA 赋值给 elementData，相当于new ArrayList(0)

### add 函数源码

```java
public boolean add(E e) {
	ensureCapacityInternal(size + 1);  // Increments modCount!!
	elementData[size++] = e;
	return true;
}

private void ensureCapacityInternal(int minCapacity) {
	if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
		minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);
	}
	ensureExplicitCapacity(minCapacity);
}

private void ensureExplicitCapacity(int minCapacity) {
	modCount++;
	// overflow-conscious code
	if (minCapacity - elementData.length > 0)
		grow(minCapacity);
}

private void grow(int minCapacity) {
	// overflow-conscious code
	int oldCapacity = elementData.length;
	int newCapacity = oldCapacity + (oldCapacity >> 1);
	if (newCapacity - minCapacity < 0)
	    newCapacity = minCapacity;
	if (newCapacity - MAX_ARRAY_SIZE > 0)
	    newCapacity = hugeCapacity(minCapacity);
	// minCapacity is usually close to size, so this is a win:
	elementData = Arrays.copyOf(elementData, newCapacity);
}
```

每次添加元素：
1. 先判断集合容量的大小，取 10 和 minCapacity 的更大值（这也验证了无参函数构造时是在第一次添加元素的时初始化容量为 10）
2. 在 ensureExplicitCapacity 中记录操作次数，即 modCount++，如果 minCapacity 大于 elementData 的长度，则对集合扩容，用到了 grow 函数
3. 函数第二行默认将集合扩容至原来的 1.5 倍，但扩容后还需进一步的判断，
	1. 如果扩充后还是小了，那么将我们所需的容量赋值给 newCapacity
	2. 如果所需容量太大，用 hugeCapacity(minCapacity) 来复制，这个函数的具体含义：(minCapacity > MAX_ARRAY_SIZE) ? Integer.MAX_VALUE : MAX_ARRAY_SIZE

再看以下几个函数的源码：
```java
public void add(int index, E element) {
	rangeCheckForAdd(index);
	ensureCapacityInternal(size + 1);  // Increments modCount!!
	System.arraycopy(elementData, index, elementData, index + 1, size - index);
	elementData[index] = element;
	size++;
}

public boolean addAll(Collection<? extends E> c) {
	Object[] a = c.toArray();
	int numNew = a.length;
	ensureCapacityInternal(size + numNew);  // Increments modCount
	System.arraycopy(a, 0, elementData, size, numNew);
	size += numNew;
	return numNew != 0;
}

public boolean addAll(int index, Collection<? extends E> c) {
	rangeCheckForAdd(index);

	Object[] a = c.toArray();
	int numNew = a.length;
	ensureCapacityInternal(size + numNew);  // Increments modCount

	int numMoved = size - index;
	if (numMoved > 0)
		System.arraycopy(elementData, index, elementData, index + numNew, numMoved);

	System.arraycopy(a, 0, elementData, index, numNew);
	size += numNew;
	return numNew != 0;
}
```

从上面几个函数源码可以看出，`add(int index, E element)，addAll(Collection<? extends E> c)，addAll(int index, Collection<? extends E> c)` 操作是都是先对集合容量检查 ，以确保不会数组越界。然后通过 System.arraycopy() 方法将旧数组元素拷贝至一个新的数组中去。

### remove 函数源码

```java
public E remove(int index) {
	rangeCheck(index);
	modCount++;
	E oldValue = elementData(index);
	int numMoved = size - index - 1;
	if (numMoved > 0)
		System.arraycopy(elementData, index+1, elementData, index, numMoved);
	elementData[--size] = null; // clear to let GC do its work
	return oldValue;
}
	
public boolean remove(Object o) {
	if (o == null) {
		for (int index = 0; index < size; index++)
			if (elementData[index] == null) {
				fastRemove(index);
				return true;
			}
	} else {
		for (int index = 0; index < size; index++)
			if (o.equals(elementData[index])) {
				fastRemove(index);
				return true;
			}
	}
	return false;
}

private void fastRemove(int index) {
	modCount++;
	int numMoved = size - index - 1;
	if (numMoved > 0)
		System.arraycopy(elementData, index+1, elementData, index,numMoved);
	elementData[--size] = null; // clear to let GC do its work
}
```

- 对于 remove(int index)，首先判断 index 是否合法，然后再判断 index 是不是最后一位
	- 如果不是最后一位，拷贝数组，将 index 之后的全部向前移动一位
- 将数组的最后一位置空， size - 1


- 对于 remove(Object o)，根据 o 是否为空，分别处理
- 遍历数组，找到一个和 o 对应的下标 index（空不空都这样处理），调用 fastRemove 方法，删除下标为 index 的元素

### get 函数源码

```java
public E get(int index) {
	rangeCheck(index);
	return elementData(index);
}
```

因为 ArrayList 的底层基于数组，所以获取元素比较简单，直接调用数组随机访问

