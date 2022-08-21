# 什么是ThreadLocal


- ThreadLocal类是一个线程内部的存储类，用来提供线程内部的局部变量
- 在多线程情况下保证各个线程内的变量独立于其它线程的变量
- 通常是private static 类型的，用于关联线程和线程上下文

主要作用：
- 把数据放到当前线程对象的map中，这个map是Thread类的实例变量
- ThreadLocal 不管理、不存储数据，只作为map和数据之间的桥梁，把数据放到map中
- 每个线程中，map的key存储的是ThreadLocal对象，value就是存储的值
- 每个Thread中的map仅对当前线程可见，线程如果销毁，map随之销毁，数据如果没被引用就被回收

总结：
**ThreadLocal的作用**：
- **线程并发：** 在多线程并发的情况下，
- **传递数据：** 在同一线程，不同组件间传递公共变量
- **线程隔离：** 每个线程的变量相互独立

## ThreadLocal 详解

作为存储数据的类，关键点在于 get 和 set 方法，

1. **每个线程持有一个ThreadLocalMap对象，叫threadLocals**，具体实现：
	- 每个新线程实例化一个 ThreadLocalMap，并且赋值给成员变量threadLocals
	- 如果 threadLocals存在，直接使用；如果不存在，新建一个，然后使用
2. **可理解为每个线程持有一个entry型的数组 table**，一切的数据读取通过操作这个table实现，可以这样理解：
	- 对于某个ThreadLocal，它的索引值是确定的（换句话说，对于同一个数据，不同线程的拷贝在各自线程的table中存放的位置是一样的），在不同线程之间访问时访问的是不同的table数组的同一位置即都为table\[i\]，只不过这个不同线程之间的table是独立的。
	- 对于同一线程的不同threadLocal，它们共享一个table数组，只是每个threadLocal在table中的索引i不同


# ThreadLocal 和 Synchronized的区别

它们都是用来处理多线程并发访问变量的问题

区别：

- ThreadLocal： 
	- 原理： 时间换空间，每个线程都提供了一份变量的副本，同时访问，互不打扰
	- 侧重点： 多线程情况下每个线程之间的数据隔离
- synchronized：
	- 原理： 同步机制，时间换空间，只提供一份变量，不同线程排队访问
	- 侧重点：多个线程间同步访问资源

