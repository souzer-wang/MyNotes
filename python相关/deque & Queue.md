## 区别
1. 概念不同：
   - Queue 是单向队列
   - deque 是双向队列
2. 来源不同：
   - Queue 是来自 queue 模块的类
   - deque 是来自 collections 模块的类

---

## 功能

### deque
1. 尾部添加 append()
2. 头部添加 appendleft()
3. 清空队列 clear()
4. 统计元素 count() 举例：
```python
newdeque.count("2")
```
5. 一次添加多个元素 extend()
```python
toextend = ['a','b','c']
newdeque.extend(toextend)
```
6. 一次添加多个元素到左边 extendleft()
7. 获取元素的索引 index()
```python
newdeque.index("a")
```
8. 指定位置插入元素 insert()
newdeque.insert(1,"c")
9. 删除元素 pop()
10. 从左边删除 popleft()
11. 移除指定元素 remove() （如果有多个相同元素，移除最左边的）
12. 反转队列 reverse()
```python
newdeque.reverse()
```
13. 旋转队列 rotate(n) 把最右边的n个元素移动到最左边，如果参数为空，则默认为1

### Queue
1. 创建时可指定长度 如:
```python
newQueue = queue.Queue(5)
```
2. 增加元素 
```python
newQueue.put("a")
```
3. 统计长度
```python
newQueue.qsize()
```
4. 取出最右边的元素
```python
newQueue.get()
```
5. 判定队列是否满了(结合前面提到的 Queue 可以限定长度)
```python
newQueue.full()
```
6. 判定队列是否为空
```python
newQueue.empty()
```