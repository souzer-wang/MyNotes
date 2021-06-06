# Synchronized
1. 什么是 synchronized ？

synchronized 是 jvm 的一种 **重量级锁**。

特点： 使用简单，不用手动释放锁，所以在开发中使用的比较多。

2. synchronized 锁的三种形式：

   - **普通方法：** 锁是当前对象实例
   - **静态方法：** 锁是当前类的 Class 对象
   - **同步块方法：** 锁是 synchronized 括号里的对象

3. 实现原理：

    - 同步代码块在编译后会在前后插入 monitorenter 和 monitorexit 指令
    - 每个对象在同一时刻只和一个 monitor 相关联
    - 当线程执行到 monitorenter 指令时会尝试获取对象所对应 monitor 的所有权
    - 如已经被其它线程获取，则需要等待锁释放

4. 




